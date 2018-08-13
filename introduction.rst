.. _introduction:

Introduction
============

Why lean is *different*
-----------------------

Programmers often brag about their favourite programming language X: "In X you can do it so easily". Maybe you can do it *easily* in X. But truth be told that, perhaps with a little bit more typing, you can also do it in Y, Z, W, and all other programming languages there are. Actually, this is called "Turing completeness", but let's not go into this rabbit hole just now.

`Lean <https://leanprover.github.io/about/>`_ really **is** different. You can use it to do things, which are basically impossible to do in an "ordinary" programming language. Specifically:

1. You can encode mathematical proofs as computer programs.
2. You can supply functions in your computer programs with impeccable type signatures. If your type signature says that your function is sorting a list, it is sorting a list, period (or you've found a bug in lean!).

Somewhat incredibly, Lean's first capability is actually achieved through a clever "thought experiment" on top of the second capability. The make it work, we will have to **agree** that mathematical proofs are special kinds of type signatures. The proofs will be the functions implementing those type signatures.

Before we get started, we need a motivating example. Here it goes: in this tutorial we're going to prove, using lean, that if :math:`a` and :math:`b` are natural numbers, then :math:`a + b = b + a`. Why is this a big deal? As you're going to realise, we're going to be working at a very low level of detail, lower than any mathematics you've ever done before. People often refer to it as "mathematical foundations", but all we will have to worry about is types and functions.

Let's get started.

