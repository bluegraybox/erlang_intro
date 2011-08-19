!SLIDE
# Erlang: Your New Favorite Scripting Language?

!SLIDE
# What's cool about Erlang?

!SLIDE center
![Scalability](factory-crop.jpg)
<span class="credit">*Image by [joguldi](http://www.flickr.com/photos/landschaft/)*</span>

!SLIDE
# Ok, but what do I do with it?

!SLIDE center
![Elegance](carving.jpg)
<span class="credit">*Image by [Stephen A. Wolfe](http://www.flickr.com/photos/swolfe/)*</span>

!SLIDE
# Where are my curly braces?!

!SLIDE bullets
* *% comment*

!SLIDE bullets
* atom
* 'atom'
* <span class="aside">*but not "atom"*</span>

!SLIDE bullets
* Variable

!SLIDE bullets
* [1, 2, 3]  <span class="aside">* list*</span>
* {1, 2, 3}  <span class="aside">* tuple*</span>

!SLIDE code
        my_func(Param1, ... ) ->
            %% do stuff
            ok.

!SLIDE bullets
* A = 10

!SLIDE bullets
* [X, Y, Z] = [1, 2, 3]

!SLIDE bullets incremental
* [First, Second | Rest] = [1, 2, 3, 4]
* [First, Second | _ ] = [1, 2, 3, 4]
* [First, Second | _Rest ] = [1, 2, 3, 4]

!SLIDE center
![Overload](overload.jpg)
<span class="credit">*Image by [Graeme Newcomb](http://www.flickr.com/photos/graemenewcomb/)*</span>

!SLIDE code
        my_func(Param1) ->
            my_func(Param1, "default");

        my_func(Param1, Param2) ->
            %% actually do stuff...

!SLIDE code
        my_func([]) -> ...

        my_func([Only]) -> ...

        my_func([First, Second | Rest]) -> ...

!SLIDE
# Guards! Guards!

!SLIDE
        my_func(0) -> ...

        my_func(X) where X > 0 -> ...

        my_func(X) where X < 0 -> ...

!SLIDE
        if
            X >= 100 -> 100;
            X >= 0   -> X;
            true     -> 0
        end

!SLIDE
        case my_func(X) of
            error -> [];
            List -> List
        end

!SLIDE
# Whew!

