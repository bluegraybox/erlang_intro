# Erlang: Your new favorite scripting language?

## What's cool about Erlang?

Normally, if you ask "What's cool about Erlang?" people will talk about scalability.
They'll talk about performance, about servers that handle thousands of requests a second without breaking a sweat.
They'll say crazy things like "Nine 9s uptime."
They'll talk about how concurrency is baked in to the language,
how spawning processes, monitoring them, and sending messages between them is trivial.
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


## Procedural, OO, Functional

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

But all programming is ultimately about taking in some data and producing some sort of output,
whether it's to a screen, database, file, network, or whatever.
It's inherently abstract and mathematical.
A language that forces you to focus on that can be very powerful and expressive.
This is all hard to explain without examples, so we'll be getting into those soon.

Doing this requires that you kinda rewire your brain.
The good news is that that's much easier than you'd expect, and a lot more fun.
Especially at first, you'll get a lot of "a-ha!" moments, with that little burst of neuro-chemical reward for figuring something out.
As with most games, it can be bewildering at first, but there are patterns and strategies you learn that help you make sense of it.

Working with procedural languages, you can do this stream of consciousness thing.
Just start at the first thing you do, and step through it from there.
I think of OO programming as kinda like working with Play-Doh.
I figure out what the main objects are, the big chunks of functionality.
Then I mush those around, occasionally ripping out some bit of functionality from one class and glomming it onto another.
As I work on them, they gradually get smoother and more coherent.
Both of these are very incremental processes.
I can just start writing code and see what comes out of it.

The experience of writing functional code is kinda like doing math.
It requires a lot more up-front thought.
There's a lot more staring at a blank screen or scribbling tentative ideas on a whiteboard.
But when you figure it out, you *know* it; it really clicks.
It's an almost physical sensation.

So why would you bother?
Why would you code in an unituitive language?
Why would you take longer to figure out how to do something functionally,
when you could just slap it together quickly in an OO or procedural language?
Because what you end up with may be much simpler and clearer.
Sometimes, the procedural solution looks like this:

    1 + 2 + 3 + 4 + ... + n

and the functional solution looks like this:

    n(n+1)/2


## Reading Erlang

In case you're not familiar with Erlang code, I'll take a couple minutes to give you the basics -
just what you'll need to be able to read the examples I'm going to throw at you; not enough to really write Erlang from scratch.

Even people who like Erlang will talk about its "weird" syntax.
Really, "weird" just means "doesn't look like C."
Almost every mainstream language since C has adopted similar syntax.
Java, the current 800-pound gorilla, deliberately tried to look as familar as possible to the legions of existing C developers.
C# went as far as keeping the name.
Javascript, Perl, Php, Python, and even Ruby are all basically C-like.

So, to start with the basics of Erlang:

    %% This is a comment.
    Variable  % starts with a capital letter.
    atom  % this is a value, not a variable - like :atom in ruby
    [1,2,3]  % list
    {1,2,3}  % tuple

You can do multi-assignments with lists like so:

    [X, Y, Z] = [1, 2, 3]  % X=1, Y=2, Z=3
    [First, Second | Rest] = [1, 2, 3, 4]  % First=1, Second=2, Rest=[3,4]
    [First, Second | Rest] = [1, 2]  % First=1, Second=2, Rest=[]

Note that this will cause an error, because 3 & 4 have nowhere to go:

    [First, Second] = [1, 2, 3, 4]

If you want to ignore them, use an underscore, either by itself or in front of a variable name:

    [First, Second | _ ] = [1, 2, 3, 4]  % 
    [First, Second | _Rest ] = [1, 2, 3, 4]

Function definitions are simple, since neither the return value nor the parameters are typed:

    my_func(Param1, Param2) ->
        %% do stuff
        X = some_func(Param1),
        Y = some_other_func(Param2),
        Status = ok,  % assign value to variable
        {Status, X, Y}.  % return a tuple

That may look a bit quirky to most programmers,
but mathematicians would be happy that this is a valid function definition in Erlang.

    f(X) -> X * 2.

You can overload functions, defining different versions of a function with the same name but different numbers of parameters:

    my_func(Param1) ->
        my_func(Param1, "default").

    my_func(Param1, Param2) ->
        %% actually do stuff...

Erlang takes overriding a step further, and lets you do it by *value*.

    my_func([]) -> ...

    my_func([Only]) -> ...

    my_func([First, Second | Rest]) -> ...

It also lets you use "guards" - boolean expressions - to further break up your function definitions:

    my_func(0) -> ...

    my_func(X) where X > 0 -> ...

    my_func(X) where X < 0 -> ...

It may not make much practical difference, but this style of code feels more declarative.
The function definitions feel more distinct and self-contained than a sequence of if-else checks.

Guards also show up in 'if' expressions.
In Erlang, these are implicitly if-elseif checks.
Here, we'll go down the left side conditionals until we find one that matches;
then we'll return the corresponding value on the right.

    if
        X >= 100 -> 100;
        X >= 0   -> X;
        true     -> 0
    end

Case statements work similarly.
Here we get the return value of my_func(X) and go down the left side until we find a match;
then return the value on the right.
If the left side is an unassigned variable, it counts as a match, and the case value is assigned to it.
This example says, "if my_func(X) returns 'error', return an empty list; otherwise assign the value to List and return that."

    case my_func(X) of
        error -> [];
        List -> List
    end

That may seem a little bizarre, but it makes for very elegant error handling:

    Output = case file:open(Filename, [read]) of
        {ok, Handle} ->
            process_file(Handle);
        {error, Reason} ->
            log_error(Reason, Filename),
            []
    end.

Essentially, this says that if the return status of file:open() is 'ok', then 
    Output = process_file(Handle)
If it's 'error', then log it and set
    Output = []

Now, turning your Erlang code into a command-line script is as simple as this:

    #!/usr/bin/escript

    main(_Args) ->
        io:format("Hello World!~n").

If you want to handle command-line arguments, you can do this:

    #!/usr/bin/escript

    main(Args) -> main("World", Args).  % invoked first, sets Name="World"

    main(_, ["-h" | _Args]) ->
        io:format("Help! Help!~n");  % print message, exit.

    main(_, ["-n", Name | Args]) ->  % ignore old name, pop new one off Args.
        main(Name, Args);            % recurse to next arg with new Name.

    main(Name, []) ->
        io:format("Hello, ~s!~n", [Name]).  % print message, exit.

Note that this will handle any number of "-n name" options, and only keep the last one.
It will exit when it gets a "-h" arg, or when it runs out of args.
It will exit with an error if it gets any other kind of arg, or if "-n" is the last arg.


## Indent Parser
I first looked at Erlang about three years ago.
It was cool, and I was excited to build an app in it, but what to do?
I didn't have any personal projects that made sense as massively distributed, concurrent apps.
So it sat on the shelf.

I came back to Erlang in a roundabout way.
I was going through one of my periodic "get my act together" phases.
This time I decided that what I really needed was a project database that I could keep in a flat file in some sort of markup language.
That would allow me to review the whole thing easily, and print it all out in a compact form if need be.
I'd group projects, sub-projects, and actions just by indents.

    project
        action
        sub-project
            action
        action
    project
        sub-project
            sub-project
                action

And the actions would have markup to indicate context, due date, priority, and so on.

    work on presentation [computer, quiet] (2011-08-24) !!

Then I could write a script to parse it and list my next actions by context in the order I should tackle them.

I never got that far.
I got as far as writing a Ruby script to parse the indented text into a logical tree structure,
that would turn this
![Input](parser/input.png)
into this
![Output](parser/structure.png)

It had the OO structure you'd expect: Parser, Lines, Trees, Nodes, Leaves.
It took each input line, and tacked it on to the tree in the next appropriate spot,
building it out like so:

![ruby step 1](ruby_parser_1.png)
![ruby step 2](ruby_parser_2.png)
![ruby step 3](ruby_parser_3.png)
![ruby step 4](ruby_parser_4.png)
![ruby step 5](ruby_parser_5.png)

That much went well, and I felt pretty good about it.
I thought I had a nice clean separation of reponsibilities, and some pretty concise code.
But then I read *Hackers and Painters*.
It's a collection of essays by Paul Graham, of HackerNews and Y Combinator fame.
Very smart guy.
You probably won't agree with everything he says, but he'll make you think.
Throughout the book, he plugs Lisp as the most powerful language in the world and the choice of all True Programmers.
Fine.
I'll pick up that gauntlet and give it a shot.

I figured I'd take my little Ruby script and re-write it in Lisp, just sort of transliterate it.
Pull all the functions out, write them in Lisp; convert the objects to simpler data structures and pass them in as parameters.
That didn't go so well.
It's hard to explain quite why, but the end result was that the code was messy, convoluted, and incoherent,
and still didn't build certain structures correctly.
I'd lose track of where I was in tree and where I was in the input stream.
Trying to fix one case tended to break another.
Most of all, it *felt* wrong.
It felt like I was trying to jam a square peg into a round hole.
So how could I do this in a more "Lisp-y" way?

Lisp is all about recursion.
You'd think that anything involving a tree structure would have to be recursive, but not really.
The way I was going about it was actually very linear: taking the input one line at a time
and walking up and down the tree until I found where to attach it.
Recursion is really about breaking a problem into self-similar pieces, so you can define the whole in terms of its parts.
Ok, but what does that look like in the real world?

Well, for contrast, here's the procedural way of transforming a list of things.
You take an input array, create an output array, and iterate through, mapping each input element to its corresponding output element.

    String[] process(String[] input) {
        String[] output = new String[input.length];
        for (int i=0; i < input.length; i++) {
            output[i] = do_stuff(input[i]);
        }
        return output;
    }

The recursive approach is to focus on defining (1) how you transform each part, and (2) how each part relates to the whole.
The idiom here is to take the first thing off the input list, transform it, add it to the output list,
and then invoke yourself with the new lists.

        process([First|Rest], Output) ->
            NewFirst = do_stuff(First),
            process(Rest, [NewFirst|Output]);

This defines what you do for each element N, and how you get to N+1.
To round it out, you need to define your begin and end points.

        Output = process(Input, []).

        process([], Output) ->
            lists:reverse(Output).

(It's convention to add each element to the beginning of the output list,
so you need to reverse it when you're all done.
It's apparently more efficient.)

The key thing here is that this is a common idiom in Erlang.
Procedural and OO languages allow recursion, but you don't often have a good reason to use it,
so it remains this strange and intimidating concept.
If you do use it, you usually put comments all around it saying "Recursion happening here! Stand back! Do not touch!"
In Erlang, it's how you do *everything*.
Anything that you'd use any sort of loop structure for in another language, you do with recursion.
You get used to it pretty fast.

So, back to the problem at hand: How do I do this parsing thing in a more recursive way?
It ended up being the classic math problem experience.
I beat my head against it for a couple hours, sketching out ideas in code, all of them abject failures.
Eventually I gave up and went to bed.
Lay there staring at the ceiling.
About twenty minutes later, I was just starting to drift off, and it hit me.
It was one of those solutions where you immediately know you're right because it's so simple and obvious in retrospect,
and you feel like an idiot for not having thought of it sooner.

The first line in the input is the first node in the tree.
Every other line either belongs to one of its children, or one of its siblings.
So you can run through the lines until you hit one with the same indent as yours.
That'll be your first sibling; everything before it belongs to your children.

![lisp step 1](lisp_parser_1.png)

All you have to do now is recurse and apply the same logic to each block.
Your first child (if any) splits its list into its children and siblings.
Your first sibling (if any) does the same with its list.

![lisp step 2](lisp_parser_2.png)

And so on...

![lisp step 3](lisp_parser_3.png)

And so on...

![lisp step 4](lisp_parser_4.png)

And so on...

![lisp step 5](lisp_parser_5.png)

...Until you're all the way down at the leaf nodes.
Here, the logic is trivial: No lines, no children, no siblings.
And you return back up the tree.

So what does the code look like?
Well, I don't have time to teach you how to read Lisp.
Fortunately, at this point it occurred to me, "Hey, Erlang is a functional language, too."
Maybe porting from Lisp to Erlang would be fairly straightforward.
As it turned out, not only was the straight conversion easy,
but I was also able to go back and rework bits to take advantage of Erlang's pattern matching features to make it even cleaner and simpler.

Here's the core of the Erlang functionality.
We define records - data structures - for our input lines and output nodes.
We pop the first line off the list, and split the rest of it into child and sibling lines.
We then recurse on the child lines to get our child nodes, and on the sibling lines to get our sibling nodes.
We create a node with its list of children, add it to the front of its list of siblings, and return that.

    -record(line, {content, indent}).
    -record(node, {content, children}).

    build_nodes([First|Rest]) ->
        {ChildLines, SiblingLines} =
            split_list(First#line.indent, [], Rest),
        Children = build_nodes(ChildLines),
        Siblings = build_nodes(SiblingLines),
        Node = #node{content=First#line.content, children=Children},
        [Node|Siblings];

    build_nodes([]) -> [].

The split_list function follows much the same pattern as our earlier recursion example.
All the lines start out in the sibling list; the child list is empty.
Instead of transforming each sibling line, it pops it off and adds it to the child list.
When it hits a line that's not indented greater than the minimum, it pushes that back onto its siblings, and returns the two lists.
(Again, reversing the children because we've been adding them to the beginning of the list.)
If that doesn't happen before its sibling list is empty, it's all children and no siblings.

    split_list(MinIndent, Children, [First|Rest]) ->
        if 
            First#line.indent > MinIndent ->
                split_list(MinIndent, [First|Children], Rest);
            true ->
                {lists:reverse(Children), [First|Rest]}
        end;

    split_list(_Indent, Children, []) ->
        {lists:reverse(Children), []}.


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

