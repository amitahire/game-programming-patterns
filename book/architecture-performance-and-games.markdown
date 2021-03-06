^title Architecture, Performance, and Games
^section Introduction

This chapter frames the rest of the book. I thought it might be helpful to give you some larger context about how I think about software architecture and how it applies to games. It may help you understand the rest of this book better. When you find yourself dragged into an argument about whether design patterns and software architecture suck or are awesome, it will give you some <span name="ammo">ammo</span>.

<aside name="ammo">

Note that I didn't presume which side you're on in that fight. Like any arms dealer, I've got weapons for all combatants here.

</aside>

However, like all frames, this chapter isn't the painting itself. If you just want some concrete information you can apply immediately to become a better programmer, I won't be offended if you skip this. Well, not much.

## What is Software Architecture?

<span name="won't">If</span> you read this book cover to cover, you won't come away knowing the linear algebra behind 3D graphics, or the calculus behind game physics. It won't show you how to alpha-beta prune your AI's search tree or simulate a room's reverberation in your audio playback.

<aside name="won't">

Wow, this paragraph would make a terrible ad for the book.

</aside>

Instead, it's about the code *between* all of that. It's less about writing code than it is about *organizing* code. We've got a bunch of terms to talk about. "Software architecture" and "design" are the big ones but "abstraction", "modularity", and a slew of others some more tied to fad methodologies than others all gravitate around the same idea.

Every program has *some* organization, even if it's just "jam the whole thing into `main()` and see what happens", so it's a lot more interesting to talk about what makes *good* architecture. How do we tell a good design from a bad one?

I've been mulling this question over pretty much the entire time I've been writing this book. Of course, we all have a solid intuition about good design. Everyone has had experiences dealing with *bad* design. We've all suffered through codebases so bad the best you can do for them is take them out back and put them out of their misery. Most us created quite a few of those as we learned to program.

A lucky few have had the opposite experience, a chance to work with beautifully designed code. The kind of codebase that feels like a perfectly appointed luxury hotel just waiting for you to take off your shoes and settle in.

What's the <span name="brace">difference</span> between the two?

<aside name="brace">

We all know the most important property is "does the program use the indentation style I happen to prefer?"

</aside>

For me, good design means that when I make a change, it's as if the entire program was crafted in anticipation of that change. I barely have to write a line of code and it slots in perfectly, solving my task while leaving not the slightest ripple on the placid surface of the code.

That sounds pretty, but it's not exactly actionable. "Just write your code so that changes don't disturb its placid surface." Right. Let me try to break that down a bit more. The first key piece is that *design is all about change*.

Someone has to be modifying the codebase. If <span name="zen">no one</span> is touching the code --whether because it's perfect and complete, or so wretched no one will touch it -- its design doesn't matter. If no one is cracking open the source, its design is irrelevant.

<aside name="zen">

There's some Zen koan in here somewhere. "If a program falls in the woods and no one reads the source, does it make a sound?"

</aside>

Before you can change the code to add a new feature, or fix a bug, or whatever reason caused you to fire up your editor, you have to understand what the existing code is doing. You don't have to know the whole program, of course, but you need to <aside name="ocr">load</aside> all of the pieces of it that are relevant to your problem into your primate brain.

<aside name="ocr">

It's weird to think that that is literally an OCR process.

</aside>

We tend to gloss over this part, but it's often the most time-consuming part of programming. If you think paging some data from RAM into disk is slow, try paging it into a simian cerebrum over a pair of optical nerves.

- 3. have to make the change

- 4. may have to shift stuff around so it doesn't seem like change was bolted on

- good arch is about 2 and 3. tend to think about 3, but two just important
- many patterns here about decoupling
- one def for coupling is two pieces of code where can't understand one without
  other
- decoupling good then: reduces stuff have to understand
- lowers amount have to load into head
- to me, key goal of good arch: less stuff have to have in head

- next piece actually making change
- other half of decoupling
- decoupling mean *change* to one piece of code doesn't cause change to other
- need to change something, so less coupling, less knock-on change ripple through

- recap: good arch is don't have to load much into head to figure out how to make change
- change is easy once figured out

## at what cost?

- sounds great
- above section is why lots excited about arch and design patterns
- good arch is joyful and helps move much faster

- but not free
- good arch takes real time
- have to think deeply about how to organize code
- which things can be decoupled
- may mean adding abstractions and interfaces
- more complexity and code, which takes time

- worse, speculative
- already established arch about change
- when add / change more code to improve arch, making prediction
- add extensibility or decoupling to x because think will need to make change to that later
- guess wrong, and wasted time
- worse, wasted complexity
- this section why lots hate design patterns
- people get so excited about extensibility and arch that add tons of abstraction
- ends up very complicated, tons of code, takes forever to develop
- little of that complexity pays off because most things end up not being changed
- "can be extended to do anything": run away screaming

- where you see game devs get into weeds of making "engine" not game
- without real requirements, impossible to tell what parts of engine should and shouldn't be extensible

## dev perf vs runtime perf

- other critique of arch and abstraction
- can make code run slower
- messages, virtual dispatch, interfaces, pointers, almost all (not templates)
  tools of abstraction have some runtime cost

- abstraction can make much quicker to develop features
- but features may run slower
- trade-off -> dev speed for game speed
- hard trade-off to make

- dev speed critical for games
- no one can pull perfect game design from nothing
- only way to get to fun balanced game is iterate iterate iterate
- have to churn through lots of bad ideas, broken mechanics
- have to do tons of tuning

- well-designed codebase can really help here
- no easy answer
- but lot easier to optimize fun game then get optimized game and make fun
- can always tear abstractions out once code has settled and know what works

## deliberate bad code

- most of book is about good arch or perf so allegience clearly on doing things "right" way
- but important to realize often value in sloppy code too

- step 4 above was reorganize to seamlessly integrate change in
- important over time to ensure code doesn't build up historical cruft
- if weird inconsistency's in codebase simply because different parts were done at different times, those distract, confuse and slow down
- think of it like campsite: every change should leave code little cleaner than it started [maybe cleanup in second patch]

- but not free either
- sometimes, writing code just to prototype, try something out, experiment, etc.
- if going to throw away code anyway, what matters is getting it running fast, getting answer to question
- sometimes best thing is to just hack it out
- just be damn sure you won't be forced to maintain it for long time

## balance

- so have these forces in play
- want decoupling and abstraction to make changes easy and minimize amount of code have to load into head
- want fast runtime perf
- want to dev quickly

- forces always in opposition
- arch and perf take time
- abstraction costs perf
- simplest impl often not fastest
- opt takes time and punches through abstraction

- no simple right answer, just constraints and trade-offs
- to me, exciting
- look at every field where people passionately devote entire careers to it
- at center will almost always find opposing constraints
- if was simple solution, would just do that every time and field would be boring
- much like game design in fact

## simplicity

- lately, feel like if there is any solution, it's simplicity
- when write code today, always try simplest, most direct thing first
- use right algo and data struct, but don't rush into abstraction or opt
- simple code does pretty well at constraints

- with simple code, just less code in general, so less to load into head to make change
- often runs fast because simply not much code to execute
- if not much code, often not take long to write [though beware not always case, first drafts longest]

- so before get into patterns, leave with
- abstraction and decoupling in right places can help program evolve
- sometimes need to optimize and make concrete
- always good to iterate quickly to zero in on game design
- but shouldn't leave mess behind you
- unless its throwaway anyway

- but most of all, best way to make something fun is to have fun making it

---

notes

- se all about constraints
- never have enough time, enough cpu, etc.
- art is balancing

- goal is ship fun game, performs well, quickly
- all agree on that
- disagree on role of arch
- some hate it, thinks design patterns interfere with goal
- others love it, think help with goal
- truth is between

- when coding, think about three axes
- coding it quickly
- coding it "maintainably"
- coding it to run fast

- arch is about second point

- imagine try to write entire game ignoring one of those
- if just focus on code speed, always cut corners
  - do things simplest possible way
  - don't bother documenting
  - don't clean up
  - go go go
  - if a needs to get to b, just call directly
  - no interfaces, no abstraction
  - end up with giant ball of mud
  - super fast at first
  - but existing code slows you down
  - as soon as it doesn't fit in head, lack of structure and docs kills you
  - if game is small enough and cycle is fast enough, may actually be viable
  - game jams
  - but anything more than month or two, or more code than can hold in head,
    or more than few people working on, not sustainable

- if just focus on maintainably
  - constantly refactor
  - add points of extensbility everywhere
  - abstractions abstractions abstractions
  - tons of docs
  - tons of tests
  - everytime make a change, first reorganize entire program so change blends
    in seamlessly, then add in
  - code is edifice of beauty
  - never ship
  - probably runs slow

- if just focus on perf
  - every tiny change takes forever
  - optimize everything, even stuff doesn't matter
  - constantly thinking about packing data structures, etc.
  - game runs super fast
  - but not actually game
  - pace of dev so slow, didn't get to add any features

- people bring historical baggage with them
- when hear people hate on arch, means they got burned by project like above
- every person ever heard critique maintainability got burned by project
    that noodled arond and didn't ship or was dog slow

- almost all of us went through period where didn't know how to write good code
  so focused on dev speed, so most know that's bad
- but important sometimes. good se habits are bad when prototyping or writing
  throwaway stuff
- projs over focused on opt are less rare
  - lots of them people making "engines" who never get around to making game

- two axes not represented in triad above
- fun
- simplicity
- if game isn't fun, nothing else matters
  - if not designer not much you can do
- no magic bullet for fun
- no one can just pull beautiful fun design out of thin air
- only one process: trial and error
- have to iterate ton
- look at tetris, etc. and think so simple idiot could come up
  with
- idiot can *clone* those, but don't see the hundreds of non-fun things alexei
  tried before hit on that
- behind every good game is a graveyard of bad ideas

- this means dev speed is crucial
- must try lots of things
- need to be able to try them fast

- arch can help and hinder here

- what is arch?
- not features
- code between features
- what is good arch?
- programming is about change
- have pile of code does some stuff, want it to do something else
- how to get from what currently does to what want it to do?
- good arch part of process to make that easy
- when making change
  - need to understand part of code related to change
  - need to understand what existing code must change
  - need to understand what code to write
  - need to write it
  - need to validate that change is correct
  - need to make sure didn't break other stuff
  - circle back: need to leave it in state so next change is equally easy

- more than just arch here: docs, naming, tests, etc.
- part that i think of is arch is
- way to organize code to minimize how much need to load into head to make
  change
- why coupling so important
- when stuff is coupled, need to think about everything coupled to when touch it
- more stuff that is, more have to load into head
- also about abstraction
- hide thing behind interface, don't have to think about impl
- all about dealing with limited monkey brain

- good arch is joy to work with
- have change in mind, slots right in
- like program was designed in anticipation of change
- can make changes fast!

- but doesn't come free
- good arch itself takes time
- have to think deeply, spend time reorganizing
- in theory, time spent on arch amortized over time saved in later changes
- but have to make sure balance actually works out
- if architect for changes don't actually make, or total num changes small,
  may be waste of time

- also have to do right arch
- arch is about prediction: think we will need to change x, so organize to make
  that easy
- if don't change x, waste
- can actively get in way when need to change y

- prediction hard

---



