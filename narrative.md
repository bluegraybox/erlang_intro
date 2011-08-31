What's cool about Erlang?

Normally, if you ask "What's cool about Erlang?" people will talk about scalability.
They'll talk about performance, about servers that handle thousands of requests a second without breaking a sweat.
They'll say crazy things like "Nine 9s uptime."
They'll talk about how concurrency is baked in to the language,
how spawning, monitoring, and sending messages between process is trivial.
They'll talk about how you can upgrade your app *while it's running*,
with each process loading its new code when it's convenient.
They'll admit it has some quirks - weird syntax and awkward constraints - but that's what you have to put up with to get the good stuff.

That is in fact amazingly cool stuff, but what do I do with it?
I don't run into a lot of problems that require that massive scale of solution.
I run into a lot of problems that require some quick little scripty thing.
Maybe bigger ones might run to a few hundred lines of code.
If I *did* have to build something massively scalable, I'd be leary of doing it in a language I didn't know.
There's a chicken-and-egg problem here.

I'd like to take a different look at Erlang, one that might help break that deadlock.
I'm going to talk about how Erlang is actually a good language for small programs,
and how its "weird" syntax and language features actually help you write clearer, more concise, and more elegant code.
They take a bit of getting used to, but you'll adapt faster than you expect, and you won't look at other languages quite the same way again.
Part of what's cool about Erlang is what's cool about functional programming,
but it goes beyond that with its own features that make it even more powerful and expressive.


Procedural, OO, Functional

A little background in case you're not familiar with functional programming.
There are three basic styles of programming: Procedural, object-oriented, and functional.
(Wikipedia lists about 60, but in my mind, and for the purpose of this discussion, these are the big ones.)
Like other human languages, programming languages are not just rules for expressing thoughts;
they also embody a culture, a way of looking at and making sense of the world.
Most are not strictly one style, but creoles, mutts to varying degrees, blending influences from the others.

Procedural programming is very narrative.
It explains the world as a sequence of actions.
Do this, do that, then do this other thing; in that case, do this instead.
Functions are like anecdotes, little incidents that the narrative is built up from.
There are things - data structures - but they're fairly simple.
They're passive - things are done to them.
The narrative may be meandering and incoherent, but there's a continuity to it.
C is a procedural language.
Perl is a procedural language with objects bolted on as an afterthought.

This fits pretty well with how people see the world.
We make sense of it through stories.
Even the barest facts need narrative context to be meaningful.

Object-oriented programming focuses on things; things that do things.
This is another way that people explain the world, though in a narrower context.
It looks more at actors and elements in isolation.
We talk about how people behave, or animals; what their capabilities are.
We talk about the utility of things, their features and limits.

OO programming focuses on these actors and elements, what you can do with them and how they interconnect.
It can be powerful and flexible, but it also runs the risk of losing the narrative.
Instead of jumbled strands of procedural spaghetti code, you can end up with ravioli code - 
code where it's clear what each individual part does, but it's hard to piece together the whole.
By the time you've traced your way down a chain of a half-dozen object linkages, you have no idea of what they're actually accomplishing.
Java is a mostly OO language.
Python and Ruby are strongly OO, with some functional influence (stronger in Ruby).

Functional languages define a mapping from inputs to outputs.
This is not a normal human way of looking at the world; it's very abstract and mathematical.
It's hard to explain without examples, so we'll be getting into those soon.
It requires that you look at the problem in front of you in very abstract, mathematical terms.
All programming is ultimately about taking in some data and producing some sort of output, whether it's to a monitor, database, network.

In essence, it's about defining
You kinda work backwards: You start by defining what you're trying to produce, then you define what you have to start with, and 
It involves a relentless focus on what you're actually trying to accomplish.

So doing this requires that you kinda rewire your brain.
The good news is that that's much easier than you'd expect, and a lot more fun.
Especially at first, you'll get a lot of "a-ha!" moments, with that little burst of neuro-chemical reward for figuring something out.
As with most games, it can be bewildering at first, but there are patterns and strategies you learn that help you make sense of it.

The experience of writing functional code is also more mathematical.
Working with procedural languages, you can do this train of thought thing.
Just start at the first thing you do, then walk through it.
I think of OO programming as kinda like working with Play-Doh.
I figure out what the main objects are, the big chunks of functionality.
I can mush them around, occasionally ripping out some bit of functionality from one class and glomming it onto another.
As I work on them, they gradually get smoother and more coherent.
Both of these are very incremental processes.
I can just start writing code and see what comes out of it.

Functional programming requires a lot more up-front thought.
There's a lot more staring at a blank screen or scribbling tentative ideas on a whiteboard.
But when you figure it out, it really clicks.
It's that almost physical sensation as .


    Functional
        Mapping inputs to outputs
        Not a normal human way to look at the world
        more like doing proofs
        Rewire your brain: cool, not as hard as it sounds, "math revelation payoff"
        De Morgan's laws
    Math vs. Play Doh
        procedural like rambing stories
        OO - structure for the sake of structure; complexity begets complexity
            easy to write lot of code - "Wait, what was the question?"
        functional - focus: inputs & outputs
    Why?
        ok, enough philosophizing

Reading Erlang in 2 min
    Where are my curly braces?
        "weird" = "doesn't look like C"
        unlike Java, Javascript, C#, Php, Perl, Python, Ruby
    comment
    atom - lowercase; :atom in Ruby, Enum in Java
    Variable - Java final
        assignment - this is not a... , it's a pattern match
    list, tuple - don't worry about it
    multi-assign
    list splitting (double tap)
    function - return atom
    f(x)
        to mathematicians, this is awesome

Function Overloading, and why these features are cool
    by parameter number
    by value
    with guards!
        again, you're mapping inputs to outputs
        let you differentiate inputs, takes input matching a step further
        makes functions more declarative
        "for input of this type, this is what I do"
        we'll get into the Why? of this later
    if - implicit elseif else
    case: not just switch - pattern match
        List is unbound
    case - file open
        this is why you don't see a lot of try/catch exception handling
    escript - hello world
    escript - option handling

Indent Parser
    Detour
        First looked at Erlang ~3 years ago: cool, but what do I do with it?
        GTD task list: store in markup, parse
    project tree
    action markup
        easy to read, review, edit; script extracts next actions
    input lines
    output structure
    Ruby
        data structures: Parser, Lines, Trees, Nodes, Leaves
    tree walk 1-5
    Hackers & Painters
        essays by Paul Graham of y-combinator, hacker news
        transliterate Ruby to Lisp
        this is basically a recursive problem
    Train wreck
        lost track of where I was in tree and where I was in input - mismatches
        felt very square peg/round hole
        What's Lisp-y? Recursion! How?
        recursion - when a function calls itself
        break the problem into self-similar pieces
        what does that look like?
    Java iteration
        simpler example
    Erlang recursion 1
        how do I move to the next element in the list?
        like a proof: n -> n+1
    Erlang recursion 2
        how do I start? what do I do when I get to the end?
        recursion in OO/procedural is rare and tricky;
            in functional, it's common and easy
    Thinker
        back to the problem at hand - how do I do this thing recursively
        think think think
    Depair
        give up, go to bed; a-ha!
    recursive 1
        at first node, break the rest of the data into Your Children and Not-
        I have children, and I'm the first of my siblings
        do this at each node
    recursive 2-5
        what does the code look like? don't have time to teach you to read Lisp
    Erlang: A Kinder, Gentler Lisp?
        hey, Erlang is also a functional language
        escript!
        translation was easy
        reworked to use pattern matching and guard idioms: nicer!
    Erlang build_nodes
        input: list of lines
        create a node that knows its children,
            put it at the front of a list of its siblings
    Erlang split_list
        like the earlier recursion, except
            we're not changing the elements,
            but we are stopping at some point

Bowling
    Scoring
        Spares & Strikes
        OO awkwardness:
            Rolls sorta belong to frames, but may be counted for more than one
        Procedural: order matters
    Python
        Game, Frame - smart objects
        53 LOC; it'd be longer in Java because of curly braces alone
        tests: ~150 LOC
        felt pretty happy with it
    Erlang
        11 lines
        frame takes frame number, current score, remaining rolls
        consumes rolls, boosts score, recurses
        17 lines after handling invalid input, incomplete games

    Test code
        unit test framework? why bother?
        erlang functional test is simpler than the python unit tests

Mirror
In Python, you spend a lot of time on these peripheral issues: What is the nature of a Frame? How does it relate to the Frames around it? Does it bear responsibility for calculating its own value, or does that fall to the Game class? It gets very metaphysical.
    Python now: "It's clearly very busy, but what is it doing?!"
    ambiguous responsibility - who does what? who checks for bad data? everyone!
    model possible interactions, not actual
    easy to write a lot of "what if" code; let alone Java
    mind-altering, without that William Hurt turning into a monkey thing
Erlang is elegant & natural
    it's a little weird at first, but embrace the weirdness,
        it starts to feel natural in a surprisingly short time
    patterns & idioms
    I've only written a couple hundred lines of Erlang
Erlang is fun
    little brain chemical reward signals
You can start small
Know that you're writing code that will scale
    parser: new process for children and siblings add 2 lines of code
        func call becomes spawn & receive, which are essentially one-line cmds
The End!
Me

