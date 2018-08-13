.. _functions_and_types:

Functions and types
====================

Exercise
-----------

Like almost any programming language, Lean has functions.

`double` for example is a function. When you're ready, press `try it!` in the top-right corner of the code box below. What you should see is an interactive Lean session. You can use for the the rest of the workshop. 

.. code-block:: lean

    -- Warning! It can take a minute or so
    -- to load up lean in your browser.

    -- BEGIN
    def double : nat → nat :=
        λ a : nat, a + a
    -- END


    -- Spoiler alert! By pressing "try it!"
    -- you'll always see the solution. If 
    -- you wand to avoid this (as you should,
    -- within reason!), copy and paste the 
    -- code from the tutorial into an active
    -- lean session, and use the "try it!"
    -- button only when you get stuck.

`nat → nat` is the function's type signature. It means that `double` accepts a non-negative integer (we'll call those "nat") and it produces another nat.

It's possible to use `def` to create simpler "functions", which we should really call "constants"

.. code-block:: lean
    
    def purpose_of_life : nat := 42

What is the type signature of `purpose_of_life`?

Exercise
-----------

Can you feed some nats into `double` and see what happens (hopefully nothing too surprising!). You'll have to use `#reduce func_name argument` to actually see the output of the function.

.. code-block:: lean

    def double : nat → nat :=
        λ a : nat, a + a    
    -- your code goes here
    #reduce double 2

Exercise
-----------

Can you use `#check` to check the type signature of `double`? Is it what you've expected? What's the type signature of `double 2`?.

.. code-block:: lean
    
    -- BEGIN
    def double : nat → nat :=
        λ a : nat, a + a

    -- check the type signatures here
    -- END
    #check double 

Exercise
------------

Can you make `triple_first_and_add` into a function, which takes two arguments, `a` and `b` and produces a sum of a triple of `a` and a `b`?

.. code-block:: lean

    -- BEGIN
    def triple_first_and_add : nat → nat → nat :=
        λ a : nat, λ b : nat, sorry
    -- END
    def triple_first_and_add_solution : nat → nat → nat :=
        λ a : nat, λ b : nat, 3 * a + 3 * b


If you're lazy to type λ (\lambda and press enter), there's a shortcut:

.. code-block:: lean

    -- BEGIN
    def triple_first_and_add2 : nat → nat → nat :=
        λ a b : nat, sorry
    -- END
    def triple_first_and_add2_solution : nat → nat → nat :=
        λ a b : nat, 3 * a + 3 * b

Exercise
------------

Can you check the type signature of the expression `triple_first_and_add 42` (Warning, that's a loaded question)? What do you expect it to be? What do you expect it to evaluate to when evaluated with `#reduce`?

.. code-block:: lean

    -- BEGIN
    -- your code checks go here
    -- END
    #check triple_first_and_add 42


Exercise
------------

What does the function `double_and_double` do? Can you confirm with `#reduce`? Can you think of a different way to implement this function?

.. code-block:: lean

    -- BEGIN
    def double_and_double : nat → nat :=
        λ a : nat, (a + a) + (a + a)

    -- check the behaviour of `double_and_double` with `#reduce` here.

    -- alternative implementation of `double_and_double` goes here. 
    -- END
    
    #reduce double_and_double 4

Exercise
-----------

Have you heard about function composition? Could you implement a `doubler`, a function, which takes a function, let's call it the `input_function` and produces another function, let's call it the `output_function`, which takes in an argument, passes it through the `input_function`, takes the result, and again passes it through the input function.

Honest warning: this is difficult and not worth spending more than 20 minutes on. You can always see a solution by pressing the "try it!" link in the top right corner of the code box. You should spend at least 5-10 minutes trying to do this before giving up.

.. code-block:: lean

    -- BEGIN
    def doubler : (nat → nat) → (nat → nat) :=
        sorry 
    -- END

    def doubler_solution : (nat → nat) → (nat → nat) :=
        λ input_function : (nat → nat),
        λ argument  : nat, input_function (input_function argument)

Exercise
-----------

Can you use your implementation of `doubler` to again implement `double_and_double`? How about `triple_and_double`?


.. code-block:: lean

    -- BEGIN
    def double_and_double_with_doubler : nat → nat:=
        sorry
    -- END
    def double_and_double_with_doubler_solution : nat → nat:=
        λ a : nat, doubler_solution double a
    
    #reduce double_and_double_with_doubler_solution 2