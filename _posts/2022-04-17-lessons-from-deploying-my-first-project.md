
I'm in the middle of wrapping up the first deployment of a personal project I've been excited for - a flashcard trainer which stores lists of flashcards, tests you on them, and has built-in logic which helps you learn them and get faster at recognising them.

On the surface, this sounds pretty straightforward (at least to me). 

Architecturally, it looks like this:
- Frontend flashcard timer (basic HTML/CSS/JS, hosted on Github pages)
- Python backend (serves cards to frontend, receives results, and calculates statistics relating to different flashcards, hosted on Heroku)
- mongoDB database (stores flashcards and results, hosted on a free mongoDB instance)

On the face of it, it doesn't seem like that much to set up. In practice, it has taken easily tens of hours of effort on my side, to arrive at a product I'm still not entirely satisfied with.

I'll go into more detail on my observations and reflections below, but here are the main takeaways - and particular credit to the ArjanCodes Discord server for helping me arrive at them:

## MAIN TAKEAWAYS: 

Following a more object-oriented design would have made this much easier to write and keep mental track of of. The fact that tests were difficult/frustrating to write should have alerted me to the fact that my design was awkward.

1. "If you find it hard to write a test, its telling you that you need to do more work on your design."
2. "You think too much in terms of dicts and json. You want to almost immediately turn your json into classes that make sense from an implementation perspective." 

## POSSIBLE REFACTORING PATHS:

1. Separate unit/TDD tests and integration tests (make better use of pytest features for sharing variables/data in the process)
2. Make database updates run as batch jobs/tasks, perhaps using Luigi: https://luigi.readthedocs.io/en/stable/


<hr>

_further context and musings below_

<hr>

## Main problems:

### Slow pace of development - testing issues?

I think this is largely due to my use of Test-Driven Development. Obviously TDD does cause some inherent slowdown to the development process, but done well this should quickly make up for itself in saved time when making changes to the code.

The way I'm implementing TDD may be contributing to slower-than-warranted development pace. 

Issues which could be contributing to this:
#### 1. Confusing test layout

I write a lot of tests, and as a result things get REALLY difficult to follow really quickly. "How I want to structure my tests going forward" is probably an important post to write.

#### 2. Inefficient use of pytest features

I could definitely make things more efficient if I knew how to:
- separate variables into different modules/files
- use fixtures as part of parametrization data

#### 3. Slow generation of test data

This may be the single most significant thing which held me back. Writing tests and writing code doesn't actually take that long - but generating the data (and organising it well so that I don't do duplicate work) for relatively involved tests really does take a long time. 

There are two hurdles here:
1. figuring out which data I need in the first place (specifically - what format does it need to be in? list, dict, etc?)
2. generating that data efficiently - it may well be the case that I need to create tools that help me create data in a way that is easy to import into my program 
    _bit of a catch-22 with some of this - as sometimes the most efficient way to generate the data may be to use the function you're going to write to test... lol_

#### 4. Overtesting

This is something that I'm less keen to consider. It probably feels like I'm testing too much, because I'm slow at it. Would fixing the above 3 issues change that? Its worth focusing on them first and finding out.


### Confusing codebase (difficult to follow, difficult/scary to make changes)
- The readability/understandability of my code is deteriorating. I found some code I wrote about 6 months ago, and while it wasn't well-designed or structured, it was clearly and logically commented - which made it very easy to pick up again very quickly.
- The project I'm working on now is probably an order of magnitude more complex than the old one, but that shouldn't mean that my code becomes less readable - it just means that the costs for not ensuring readability are higher.

**Changes I can make in my development practice going forward:**

1. Number the different execution steps in a function - this makes it very readable and logical
2. Don't create premature abstraction - use TDD to create a long, "gross" function, and then refactor the code I've written. 
    _Hopefully_ this will help me keep better mental track of what I'm doing, and also lead more naturally to code that fits together logically and develops naturally.

_other ideas_
- use docstrings as I set up functions? this might just be a function of creating abstractions too early in the process

### VERY confusing tests and test data

## Unexpected timesinks

- A significant amount of the time spent on this project has been trying to set up test cases
- Another decent chunk of time has gone towards web-deployment related issues, specifically:
    - CORS issues between the frontend and backend
    - At the end of it all, deploying to Heroku and finding out that my database operations were too inefficient, causing a number of timeouts on Heroku's side
        - eg, looping through a set of data and making a single call to the database for each item, instead of building a bulk call to the database

## Learnings

### Worry about efficiency when it comes to data "leaving" the code, not so much within the code.
I've heard a lot of people make mention of the fact that engineers worry too much about efficiency in their code. Given the phenomenal power of modern computers, even things which scale with logarithmic are not likely to significantly impact execution speed. What I wasn't paying attention to, however, was that this only applies when performing operations on the same machine. **As soon as code leaves the environment - eg, in an API or database call - execution time and efficiency does start to matter a lot more.**

I still don't need to obsess over the minute efficiencies, but I should try to build sane operations where I can. A great example is the database operation that makes a call for each item in a list - I knew this was inefficient when I built it, but dismissed it on the advice that I should only worry about optimising things which become a problem. That advice held true, and I now have a much better intuition for what might become a problem in the future. 

### How to time my code

Practically speaking, this pattern was very useful when trying to figure out what parts of my code were performing slowly: 

```python
import timeit

start = timeit.default_timer()

run_code_you_want_to_time()

stop = timeit.default_timer()
print('Time: ', stop - start)
```