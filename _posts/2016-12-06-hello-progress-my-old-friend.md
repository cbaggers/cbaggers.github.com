---
layout: post
title: Hello progress my old friend
description:
date: 2016-12-06 15:52:25
category:
tags: ['lisp', 'varjo', 'types']
commentIssueId:
---

Ah this week was so much better, my brain and I were on the same team.

I made good progress in my compiler with first class functions. The way I implemented it is roughly as follows:

I make a class to represent compile-time values

    (defclass compile-time-value (v-type)
      (ctv))

It inherits from `v-type` as that is the class of my compiler's types.

It has one slot called `ctv` that is going to store what the compile things the actual value is during compilation.

IIRC this associating of a value with a type is called 'dependent types'. However I'm going to avoid that name as I don't know nearly enough about that stuff to associate myself with it. I'm just going to call this compile-time-values or `ctv`s.

Next we need a type for functions.

    (defclass function-spec (compile-time-values)
      (arg-spec
       return-spec))

Here we make a type that has a list of types for the arguments (`arg-spec`) and a list of types for the returns (`return-spec`). Return is a list as lisp supports multiple return values. Being a `ctv` the compiler can now associate values with this type.

Note we don't have a name here as this is just the type of a function, not any particular one. In my compiler I have a class called `v-function` that describes a particular function. So there is a `v-function` for `sin` for example.

In lisp to get a function object we use the `#'` syntax. So `#'print` will give you the function named `print`. `#'thing` expands to `(function thing)` so in my compiler I defined a 'special form' called `function` that does the following:

1. look up the `v-function` object for that name
2. make an instance of `function-spec` with the result of step `1` as the `ctv`
3. use the result of step `2` as the type of this form.

Nice! this means the specific function is now associated with this type and will be propagated around.

    (let ((our-sin #'sin))
      (funcall our-sin 10))

Later our compiler will get to that `funcall` expression. It will look at the type of `our-sin` and see the `ctv` associated with it. It will then transform `(funcall our-sin 10)` to `(sin 10)` and compile that instead.

## Functions that take compile time values as arguments

We do a very simple hack when it comes to this. If we have something like this:

    ;; this takes a func from int to int and call it with the provided int
    (defun some-func-caller ((some-func (function (:int) :int))
                             (some-val :int))
      (funcall some-func some-val))

And we call it in the following code:

    (labels ((our-local-func ((x :int))
               (* x 2)))
      (let ((our-val 20))
        (some-func-caller #'our-local-func our-val)))

Then the compiler will swap out the `(some-func-caller #'our-local-func our-val)` call with a local version of the function with the compile time argument hardcoded

    (labels ((our-local-func ((x :int))
               (* x 2)))
      (let ((our-val 20))
        (let ((some-func #'our-local-func))
          (labels ((some-func-caller ((some-val :int))
                     (funcall some-func some-val)))
            (some-func-caller our-val)))))

The `some-func` var is in scope for the local function `some-func-caller` so the transform we mentioned earlier will just work. The rest is just a local function transform and the compiler already knew how to do that.

Things get more complicated with closures and I havent finished that. I can now pass closures to functions but I cannot return them from functions yet. I know how I could do it but it feels hacky and so I'm waiting for more inspiration before I try that part again.


## Primed for types

With all this compiler work my brain was obviously in the right place to start things about static typing in general. Being able to define your own type-system for lisp is something I have wanted for ages, but as support for this isn't built into the spec I've been trying to work out what the 'best approach™' is.

quick notes for those interested. Lisp has an expressive type system and a bunch of features to make serious optimizations possible. However it doesnt have something to let me define my own type system and use it to check my lisp code.

The problem boils down to macroexpansion. If you want to typecheck your code you want to expand all those macros so you are just working with vars, functions & special-forms (dont worry about these). However there isn't a 'macroexpand-all' function in the spec[0]. There is a function for macroexpanding a macro form once, however this does not take care of things like the fact that you can define local, lexically scoped macros. This means there is an 'expansion environment' that is passed down during the expansion process and manipulating this is not covered by the spec.

There is however a tiny piece of fucking voodoo code that was written by one of the lisp compiler guys. It allows you to define a locally scope variable that is available at compile time within the scope of the macro. With this i can create and object that will act as my 'expansion environment' and let me have what I need.

Anyhoo, the other day I case up with a scheme for defining blocks of code that will be statically checked and how I will do macroexpansion. It's not perfect, but it's predicable and that will do for me.

I am going to make a library who's current working title is `checkmate`. It will provide these static-blocks and within those you will be able to emit `fact`s about the expressions in your code. For function calls it will then call a `check-facts` method with the arguments for the function and all the `fact`s it has on them. You can overload this method and provide your own checking logic.

The `fact`s are just object of type `fact` and can contain anything you like. And because you implement `check-facts` you can define any logic you like there.

This should give me a library which makes it easier to define a type system. I can subclass `fact` and call that `type` and inside `check-facts` I implement whatever type checking logic I like.

A while back I ported an implementation of the [Hidley (Damas) Milner checking algorithm](https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system) to lisp so my goal is to make something where I plug this in and get classic ML style type checking on lisp code.

Wish me luck!

## Next?

I'm not sure, my next year is going to contain a lot of study so I hope I can keep on top of these projects as well. The last few weeks have certainly reminded me to trust my instincts on what I should be working on, and it's good to feel 'out of the woods' on that issue.

Peace


[0] Although a bunch of implementation do provide one. I made a [library to wrap those](https://github.com/cbaggers/trivial-macroexpand-all) so technically I do have something that could work
