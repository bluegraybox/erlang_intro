!SLIDE center
![Detour](detour.jpg)
<span class="credit">*Image by [chrisdlugosz](http://www.flickr.com/photos/chrisdlugosz/)*</span>

!SLIDE code
        project
            action
            sub-project
                action
            action
        project
            sub-project
                sub-project
                    action

!SLIDE bullets small
* work on presentation [computer, quiet] (2011-08-24) !!

!SLIDE center
![Input](input.png)

!SLIDE center
![Output](structure.png)

!SLIDE center
![Ruby](ruby.png)
<span class="credit">*Image from [Wikimedia Commons](http://en.wikipedia.org/wiki/File:Ruby_logo.svg)*</span>

!SLIDE center
![ruby step 1](ruby_parser_1.png)

!SLIDE center
![ruby step 2](ruby_parser_2.png)

!SLIDE center
![ruby step 3](ruby_parser_3.png)

!SLIDE center
![ruby step 4](ruby_parser_4.png)

!SLIDE center
![ruby step 5](ruby_parser_5.png)

!SLIDE smaller commandline
    $ echo -e "a\n b\n c\n  d\n e\nf\n g\n  h\n   i\n j\n k\nl" | ./parse_indents.rb 
    a::b
    a::c::d
    a::e
    f::g::h::i
    f::j
    f::k
    l

!SLIDE center
![Hackers & Painters](hackers.gif)
<span class="credit">*Image from [O'Reilly Media](http://oreilly.com/catalog/9780596006624)*</span>

!SLIDE center
![Train Wreck](train.jpg)
<span class="credit">*Image from [IMLS DCC](http://www.flickr.com/photos/imlsdcc/5576592397/)*</span>

!SLIDE small
        String[] process(String[] input) {
            String[] output = new String[input.length];
            for (int i=0; i < input.length; i++) {
                output[i] = do_stuff(input[i]);
            }
            return output;
        }

!SLIDE small
        def process(input)
            output = []
            input.each() { |thing|
                output.put(do_stuff(thing))
            }
            output
        end

!SLIDE small
        def process(input)
            input.map { |thing| do_stuff(thing) }
        end

!SLIDE
        process(Output, [First|Rest]) ->
            NewFirst = do_stuff(First),
            process([NewFirst|Output], Rest);

!SLIDE
        Output = process([], Input).

        process(Output, [First|Rest]) ->
            NewFirst = do_stuff(First),
            process([NewFirst|Output], Rest);

        process(Output, []) ->
            lists:reverse(Output).

!SLIDE center
![thinking](thinker.jpg)
<span class="credit">*Image by [Brian Hillegas](http://www.flickr.com/photos/seatbelt67/502255276/)*</span>

!SLIDE center
![more thinking](despair.jpg)
<span class="credit">*Image by [Alex E. Proimos](http://www.flickr.com/photos/proimos/4199675334/)*</span>

!SLIDE center
![lisp step 1](lisp_parser_1.png)

!SLIDE center
![lisp step 2](lisp_parser_2.png)

!SLIDE center
![lisp step 3](lisp_parser_3.png)

!SLIDE
# Erlang: A kinder, gentler Lisp?

!SLIDE smaller
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

!SLIDE smaller
    split_list(MinIndent, Children, [First|Rest]) ->
        if 
            First#line.indent > MinIndent ->
                split_list(MinIndent, [First|Children], Rest);
            true ->
                {lists:reverse(Children), [First|Rest]}
        end;

    split_list(_Indent, Children, []) ->
        {lists:reverse(Children), []}.

