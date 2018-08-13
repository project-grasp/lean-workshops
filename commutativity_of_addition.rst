.. _commutativity_of_addition:

Commutativity of addition
===========================


Inductive types
----------------

Recall that our original motivation is proving that `a + b = b + a`. This property of addition is called "commutativity", and if you decide to carry on with your mathematical education at University level, you'll hear much more about.


But for now, let's focus on the task. Technically, it would seem we've already got all the required ingeredients to make progress. We know about `eq` or `=`. We know that `a` and `b` are of type `nat`. We know about `+`. However, it turns out that in order to prove `a + b = b + a`, we have to deepen our understanding of both `nat` and `+`. It turns out that `nat`, unlike corresponding concepts in other programming languages, is not primitive, i.e. it is defined in Lean using more fundamental concepts (specifically, inductive types). Let's roll our own `nat`, and to avoid name clashing, let's call it `natural`.

.. code-block:: lean
    
    inductive natural : Type 
        | zero : natural
        | succ : natural → natural

What this definition is telling us, is that all natural numbers are of two kinds. One kind is `zero` (boring, but important!). The other kind is successors ("succs"), i.e. natural numbers which immediately follow another natural number (e.g. 4 or 42). Every natural number is of one kind or the other, and all natural numbers belong to one of the kinds. Whenever we introduce a new definition (e.g. `addition`), or try to construct a new proof (e.g. `a + b = b + a`), we will have to think about both kinds, `zero` and `succ`.

Exercise
----------

Although we still don't know enough about inductive types to define `addition`, we can use `natural` to do something useful. Specifically, we can construct any number we want (given sufficient patience). This is how we can construct 3:

.. code-block:: lean

    #check natural.succ (natural.succ (natural.succ natural.zero))

Can you construct 2, 4 and 0?

.. code-block:: lean

    -- your construction of 2, 4 and 0 goes here.

Defition of addition
---------------------

Let's define addition now. We will have to use an important "magic" ingredient of inductive types: recursors. These are automatically generated for any inductive type, and are part of the low-level machinery, which gives lean both of the capabilities discussed in the introduction. We can use `natural.rec_on x` to prove or define things about `x`, as long as we allow for the possiblity that `x` can be either of a `zero` kind or of a `succ` kind. This is one good way to start defining addition:

.. code-block:: lean

    def addition_sorry_sorry (a : natural) (b : natural) : natural :=
        natural.rec_on b sorry sorry

What's happening there is that we'll define addition by considering two separate cases, first case is when b is `zero`, and second case, when b is a `succ` of some other natural. In the first case things are really easy, if b is zero, the result of addition is simply `a`.

.. code-block:: lean

    def addition_sorry (a : natural) (b : natural) : natural :=
        natural.rec_on b a sorry

Exercise
----------

Getting rid of the second sorry is harder. What can be of some help is replacing it with `_` and hovering over it with your pointer. This will tell you that you have to replace the underscore with a function typed `natural → natural → natural`. Just to make it really clear, this means a function with two arguments, both `nat`s, returing a nat. What this type signature fails to tell you is the meaning behind the first two arguments that the function accepts.

The first argument is going to stand for, effectively :math:`b - 1`. We can safely assume that one exists, because we've dealt with the `zero` case in the first sorry, and now we're dealing with a situation, where `b` can be looked at as `natural.succ prev_b`. Let's call this first argument `prev_b`

The second argument is a bit more magic. It's called the "inductive hypothesis", and it's the result of feeding `prev_b` into our definition of addition. This can be very difficult to comprehend, but as usual, trying a couple of examples should help to develop understanding, and you'll see a few more examples before the end of this tutorial. For now, let's just call the second argument `result_on_prev_b`. From here it's not hard to work out that what our definition has to return is `natural.succ result_on_prev_b`. Can you put all these details together into a working implementation of `addition`? 

.. code-block:: lean

    -- BEGIN
    def addition (a : natural) (b : natural) : natural :=
        natural.rec_on b a sorry
    -- END

    def addition_solution (a : natural) (b : natural) : natural :=
        natural.rec_on b a (λ (prev_b : natural) (result_on_prev_b : natural), natural.succ result_on_prev_b)

    #reduce addition (natural.succ (natural.succ natural.zero)) (natural.succ natural.zero)

Can you think of using `#reduce` to verify the correctness of your definition? Think about testing some simple things, such as 2 + 1.


Exercise
----------

There is exactly one more good way to define addition (at least as far as I can tell!). This is to recurse on the first argument. Can you finish this definition and test that it works as expected using #reduce

.. code-block:: lean

    def addition2 (a : natural) (b : natural) : natural :=
        natural.rec_on a sorry sorry

Back to nat
-------------

The good news is that all these definitions are already provided in lean, together with some syntactic sugar, such as allowing us to type `a + b` instead of `addition a b`, and use actual Hindu-Arabic numerals as opposed to chains of `succ`s, so for the rest of the tutorial we can just revert back to the good old `nat`.

We can repeat our experiment with addition and `#reduce`, this time using more human-friendly notation. What is the most important observation you can take from this experiment?

Exercise
----------

.. code-block:: lean

    #reduce 2 + 0
    #reduce 0 + 2

Exercise
----------

There is a standard way to prove that two terms which reduce to the same term are equal. Can you find it yourself? This place might be helfpul: https://leanprover.github.io/theorem_proving_in_lean/quantifiers_and_equality.html#equality. Can you finish off the following proof?

.. code-block:: lean

    -- BEGIN

    def two_plus_zero_equals_zero_plus_two : 2 + 0 = 0 + 2 := sorry

    -- END

    def two_plus_zero_equals_zero_plus_two_solution : 2 + 0 = 0 + 2 := rfl

Exercise
----------

Can you state the goal of the tutorial (to prove that a + b = b + a) using lean? You can call it `addition_is_commutative`. You won't be able to prove it just now, so use `sorry` in place of the function definition.

.. code-block:: lean

    -- Your definition of `addition_is_commutative` comes here.

The `have` and the `show` keywords
------------------------------------

There's only one thing stopping you from writing long and complicated proofs (like the one of `addition_is_commutative`). While technically you know everything you might need to write them, as soon as you try to do it, you'll realise that your proofs are becoming very long and ugly.

Consider the following example:

.. code-block:: lean

    variables a b c d e : nat

    variable proof_b_eq_a : b = a 
    variable proof_c_eq_b : c = b
    variable proof_c_eq_d : c = d
    variable proof_d_eq_e : d = e

    #check ((eq.trans (eq.symm proof_b_eq_a) (eq.symm proof_c_eq_b)))

    def proof_a_eq_e : a = e := eq.trans (eq.trans (eq.trans (eq.symm proof_b_eq_a) (eq.symm proof_c_eq_b)) proof_c_eq_d) proof_d_eq_e

This example is not only horrible to read, it was also horrible to type. I had to cheat with `#check` a couple of times before I got it right.

But how about this:

.. code-block:: lean

    variables a b c d e : nat
    variable proof_b_eq_a : b = a 
    variable proof_c_eq_b : c = b
    variable proof_c_eq_d : c = d
    variable proof_d_eq_e : d = e

    def proof_a_eq_e : a = e :=
        have proof_a_c : a = c, from eq.trans (eq.symm proof_b_eq_a) (eq.symm proof_c_eq_b),
        have proof_a_d : a = d, from eq.trans proof_a_c proof_c_eq_d,
        show a = e, from eq.trans proof_a_d proof_d_eq_e

Hopefully this should be nice and easy. Just to check you have a correct understanding of `show` and `have`, can you break down the `proof_a_c` into two smaller steps: `proof_a_b`, `proof_a_c`.

.. code-block:: lean
    
    variables a b c d e : nat
    variable proof_b_eq_a : b = a 
    variable proof_c_eq_b : c = b
    variable proof_c_eq_d : c = d
    variable proof_d_eq_e : d = e

    def proof_a_eq_e : a = e :=
        -- modify the line below
        have proof_a_c : a = c, from eq.trans (eq.symm proof_b_eq_a) (eq.symm proof_c_eq_b),
        have proof_a_d : a = d, from eq.trans proof_a_c proof_c_eq_d,
        show a = e, from eq.trans proof_a_d proof_d_eq_e

Exercise
---------

We need a couple of auxiliary theorems (often called lemmas), before we can show that `a + b = b + a`. I'm going to prove the first one. Don't worry if you don't understand it, recursing on `eq` is beyond the scope of this tutorial. Your job is to just tell me **what** it is that I've proven (Hint: look at the type signature). 

.. code-block:: lean

    def succ_over_equality {a b : nat} (H : a = b) : (nat.succ a) = (nat.succ b) :=
        eq.rec_on H (eq.refl (nat.succ a))

Exercise
----------

Remember the task where you were supposed to discover `eq.refl`? Could you use it to prove the following lemma?

.. code-block:: lean
    
    -- BEGIN
    def add_zero_right (a : nat) : a + 0 = a :=
        sorry
    -- END
    def add_zero_right_solution (a : nat) : a + 0 = a :=
        rfl

Exercise
----------

It's actually much harder to prove the "left" version of the lemma. But I'll get you started.

.. code-block:: lean
    
    -- BEGIN
    def add_zero_left (a : nat) : 0 + a = a :=
        nat.rec_on a rfl (λ prev_a : nat, λ IH : 0 + prev_a = prev_a,
            sorry)
    -- END

    def add_zero_left_solution (a : nat) : 0 + a = a :=
        nat.rec_on a rfl (λ prev_a : nat, λ IH : 0 + prev_a = prev_a,
            have X : nat.succ (0 + prev_a) = (nat.succ prev_a), from succ_over_equality IH,
            have Y : nat.succ (0 + prev_a) = (0 + (nat.succ prev_a)), from rfl,
            eq.trans (eq.symm Y) X)


Exercise
-----------

Can you prove that `0 + a = a + 0`, using your code for `add_zero_left` and `add_zero_right`?

.. code-block:: lean
    
    -- BEGIN
    def addition_is_commutative_base_case_solution (a b : nat): (a + 0) = (0 + a) := sorry
    -- END

    def addition_is_commutative_base_case_solution (a b : nat): (a + 0) = (0 + a) := eq.trans (add_zero_right a) (eq.symm (add_zero_left a))


Exercise
-----------

Can you prove the following two lemmas? The first one is very easy.

.. code-block:: lean

    -- BEGIN
    def successor_over_sum (a : nat) (b : nat) : a + nat.succ b = nat.succ (a + b) := sorry
    -- END

    def successor_over_sum_solution (a : nat) (b : nat) : a + nat.succ b = nat.succ (a + b) := rfl

For the second, you will have to use a recursor.

.. code-block:: lean

    -- BEGIN
    def successor_over_sum2 (a : nat) (b : nat) : (nat.succ a) + b = nat.succ (a + b) :=
        sorry
    -- END

    def successor_over_sum2_solution (a : nat) (b : nat) : (nat.succ a) + b = nat.succ (a + b) :=
    nat.rec_on b rfl (λ prev_b, λ IH : nat.succ a + prev_b = nat.succ (a + prev_b),
        show nat.succ a + nat.succ prev_b = nat.succ (a + nat.succ prev_b), from
        have X : nat.succ (nat.succ a + prev_b) = nat.succ (nat.succ (a + prev_b)), from succ_over_equality IH,
        X)

Exercise
-----------

And eventually, our final goal!

.. code-block:: lean

    -- BEGIN
    def addition_is_commutative (a b : nat) a + b = b + a := sorry
    -- END

    def addition_is_commutative_solution (a b : nat) : a + b = b + a :=
        nat.rec_on b
            (addition_is_commutative_base_case a b)
            (λ prev_a, λ H : a + prev_a = prev_a + a,
            show a + nat.succ prev_a = nat.succ prev_a + a, from
            have X : a + nat.succ prev_a = nat.succ (a + prev_a), from successor_over_sum a prev_a,
            have Y : (nat.succ prev_a) + a = nat.succ (prev_a + a), from successor_over_sum2 prev_a a,
            have Z : nat.succ (a + prev_a) = nat.succ (prev_a + a), from succ_over_equality H,
            (eq.trans (eq.trans X Z) (eq.symm Y)))