**Welcome to Blind Tools Log, index 0. Inspired by Ben Awad and various encouragements around the web to "learn in public", I will _try_ to chronicle my foray into my first proper project here.** 

## The Problem I'm Solving

### Problem Context and My Background 
I solve Rubik's Cubes blindfolded. In one sense, this isn't as impressive as it sounds, in another its _very_ difficult to get good at. Highly complex - lots of different variables one has to try and control for and optimise. In my view, it necessitates a highly granular and data-driven form of practice that is very difficult to achieve using any currently-available tools. 

For a while now, I have been thinking of how great it would be to have a WebApp that enables the practice I have in mind. Until recently, I wrote this off as a pipe dream because:
1. I lacked the software development skills to make it happen
2. I lacked the time to build it - any time spent on developing it, I'd rather spent on actually practicing the craft of blindsolving. 

How have those things changed? 
1. Doing more development-related work in my day job has really helped me realise how much I love solving problems with software. And this seems like a great way to both develop a bit of a portfolio project, and build something I'd find incredibly useful. Also it just seems fun. 
2. I'm trying to have a slightly more healthy/less competitive relationship with my blindsolving practice - but also I know that in the long run having a set of tools like this will really help me improve more efficiently, and hopefully enjoy the process more. 

### The Problems I'm Trying to Solve

1. Identifying errors during solves is very difficult - reconstructions and solve parsing would make that much easier
2. Learning and getting fast at 1500 different letter pairs is a significant undertaking - understanding which I'm fast at/slow at, scheduling when to review and practice what, analysing comm mistakes/confusions
3. "What do I need to do to get faster" is a difficult question to answer
    1. Gathering data on comm recognition vs comm execution is almost impossible  
    2. Gathering real-world data on comm execution (which ones am I fast at? What am I slow at? Where during a comm do I pause/lock up?) during solves is very difficult 
    3. Getting the splits of a solve is very difficult
4. Even if you can get all this data, its a PHENOMENAL amount of data to process
   1. Excel starts slowing down noticeably trying to process results data
   2. Building a template that shows all the different results has been tricky and finnicky - some part of this is probably just not approaching it as formally as I could have (ie, proper design documents etc), but ultimately any Excel-based solution would be highly manual, and ultimately dependent on me doing a bunch of extra work to keep the results updated. 


### The Solutions I'm Looking to Implement

#### Flashcards
1. A timed flashcard system with correct/incorrect indications
2. An analysis engine that looks at flashcard results and determines which flashcards to drill first
3. A flashcard editor that gives me a GUI to change the contents of flashcards
4. A stats display which visualises/exposes the data relating to flashcards (distribution of speeds, what I get wrong most often, etc)

#### Bluetooth Cube
1. Do a full 3BLD solve with a suite of different analytics
   1. DNF reasons
   2. Solve splits
   3. Comm execution speeds
   4. Exec vs recog speed during execution stage
   5. Store comm performance data for long-term analysis
2. Comm drilling (show a flashcard of a comm and time the recog/exec speed)
   1.  This should be linked up to the flashcard display engine, so that I practice the comms I'm most slow at/mess up on most often



## Project Management and Software Design 

I think one of my biggest strengths coming in as a newbie developer are my project management and system design chops. I'd like to leverage those skills while building this - partially to show them off (such as they are lol), but mostly because I know they'll make this project a lot less painful in the long run. 

I'll be using draw.io to draw any diagrams, and probably Notion to run the project management. 

## My Progress so Far

So far, I've managed to build an MVP flashcard timer, which displays the current flashcard prompt, times how long it takes the user to answer, and then displays the correct answer.

Honestly, this is already more than I would have ever thought possible. I have to thank The Odin Project's web development fundamentals course, which has really given me a great environment to sharpen the vague, scattered CSS/HTML/JS knowledge I've picked up here and there. 

### Significant Learning - The Value of Notes

I am shocked by how much taking notes while learning has enhanced my learning process. I've started trying to get into the habit of writing into notes anything new I learn (basically, anything answers I look up on Google). Mostly, this includes syntax and notes on how to use different bits of technology. The confidence/comfort it has given me is **unreal**, though, and feels disproportionate to the simple act of taking notes. 

I think there are two things at play here: 
* Rewriting concepts I find on the internet into my own words is a form of active learning, that affirms internally the fact that I do understand what I've just read
* Collating things I would commonly Google (like string method syntax) both speeds up how quickly I code, and 
  * I've taken the time to actually understand/write out my different options
  * Consulting a single reference document where I know exactly what to find what I'm looking for is faster than Google
  * Consulting a reference document is much less mentally taxing/stressful - sounds weird, but true. At least for me. I think this is due to (a) less context switching, (b) less need for decision making/parsing information to find what you need

Ultimately, I just like the efficiency of "caching" whatever I learn. The alternative is that I keep Googling the same answers to the same questions, which violats DRY ("don't repeat yourself".)

Not having notes is just bad design. 
 
## Next Steps

Now that I have my little baby MVP set up, there are a number of different directions I can continue in. I'm hoping this post will help me choose between them. 

### Next Features to Add

My flashcard timer is a great start, and honestly very close (from the client perspective) to being useful. The things that need to be implemented are: 
* a database
  * store flashcard list
  * store results of flashcard training 
* analysis engine (looks at results in database and determines which flashcards subset to present to the user)
* means of passing flashcards to be trained from analysis engine/database to the frontend  
* flashcard training menu on frontend (practice options - what set, what type of practice, etc)
* frontend features
  * queue flashcards instead of randomising them (guarantee everything will be tested)
  * toggle timer display off
  * indicate if a card was correct/incorrect 

Ok. Maybe not as close as I thought. That's the way of these things, though! 

Another thing I probably want to do is review the architecture - change it from a thrown-together group of functions to a well-designed software project. 

### Decide on Tech Stack

Once we start talking about analysis and data storage, we're out of the realm of pure Javascript. My options for a stack are:
* Python backend, JS frontend
  * How do I connect them? 
* Node backend, JS frontend
* Python backend, Dash/Streamlit/Panel-powered frontend

I'm probably most partial to the pure-Javascript option. A big thing I'm going to want to include is Bluetooth cube support, and the Bluetooth cube modules I'm aware of are all written in Javascript. At the very least, I'd like be able to integrate them easily - which means a JS frontend is essential - but also being able to understand/troubleshoot/edit them as necessary would be ideal. And the best way to ensure that is probably a deeper knowledge of Javascript. 

### Decide How to Learn the Skills I Need

So, I need to learn Nodejs. I also need to learn how to link backend and frontend. And how to work with databases. 

There are two options here:
1. Self-guided learning (Googling what I need to do next)
2. The Odin Project Nodejs track

On the face of it, self-guided learning could help me get to where I want to go faster. But I don't think I can discount the value of learning in a structured way, and the confidence that project-based learning brings. So far, The Odin Project has been *the* primary thing that has helped me get this far. And if I keep on the track, my **potential** output definitely ends up being higher faster than if I were to try to self-teach. 

This does mean that work on the project itself will be delayed until I finish the Nodejs track. Although even that isn't necessarily true - I can probably think about things that can be done simultaneously, like a move parser or the flashcard analysis logic, database scheming and so on. 

## Action Points

1. Fully commit to the Nodejs path on The Odin Project, get through it as fast as possible (1-2 weeks?)
1. In the meantime, I can still do basic architecture/design and planning work. Work towards bldToolsLog[1], wherein I define MVP1, do feature planning (what are all the features I want and in what priority), and perhaps create a basic architecture diagram 

#### Future Topics

- bldToolsLog[1] - defining MVP1 and feature planning
- Tooling (vim, how I'm taking notes, etc)