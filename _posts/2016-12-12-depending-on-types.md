---
layout: post
title: Depending on Types
description:
date: 2016-12-12 20:38:21
category:
tags: ['lisp', 'cepl']
---

Working on the compiler in the previous weeks got me in a mode where I was thinking about types again.

I'm pretty set on the idea of making static type checking for lisp, however I'm interested in not only being able to validate my code, but also to play with random types systems and maybe make my own. I need an approach to doing this kind of compile-time programming.

Macros give a super simple api, they are just a function that gets called when the code is being compiled. Say we make a macro that multiply some numbers at compile time (a bad usage but that doesnt matter for the example)

    (defmacro multiply-at-compile-time (&rest numbers)
      (assert (every #'numberp numbers)) ;; check all the arguments were number literals
      (apply #'+ numbers)) ;; multiply the numbers

And now we have a function that calculates the numbers of seconds in a given number of days:

    (defun seconds-in-days (number-of-days)
      (* number-of-days (multiply-at-compile-time 60 60 24)))

When we compile this the macros need to be expanded, so for each form the compiler will look and see if is a list where the first element is the name of a macro. If it is then it calls the `macro-function` with the code in the argument positions. So it'll go something like this:

- Looks at: `(defun seconds-in-days etc etc)`
- is `defun` the name of a macro? Yes, call macro-function `defun` and replace `(defun seconds-in-days etc etc)` with the result
- the code is now:

```
    (setf (function seconds-in-days) ;; NOTE: Not the actual expansion from my compiler, just an example
          (lambda (number-of-days)
            (* number-of-days (multiply-at-compile-time 60 60 24))))
```

- is `setf` the name of a macro? (let's say no for the sake of this example) No? ok continue
- Looks at: `(function seconds-in-days)`
- is `function` the name of a macro? No? ok continue
- this repeats until we reach `(multiply-at-compile-time 60 60 24)`
- is `multiply-at-compile-time` the name of a macro? YES, call the macro-function `multiply-at-compile-time` with the arguments `60`, `60` and `24` and replace `(multiply-at-compile-time 60 60 24)` with the result.

The final code is now:

    (setf (function seconds-in-days)
          (lambda (number-of-days)
            (* number-of-days 86400)))

Technically we have avoided multiplying `60`, `60` and `24`.. once again, this of course this is a TERRIBLE use of macros as these kinds of optimizations are things that all decent compilers do anyway.

The point here though is that we implement a function and then there is a mechanism in the language that knows how to use it. This makes it endlessly flexible.

So if I'm going to make a mechanism for hooking in types then I want a similar interface, implement something and have it be called.

Now I know nothing about how 'real' type-systems are put together so I bugged a dude at work who knows this stuff well. Olle, is ace, he's currently writing his own dependently-typed language for low level programming and so clearly knows his shit. When I mentioned I wanted this to be fairly general he recommended I look at 'bidirectional type checking'.

A quick google later I had a pile of pdfs, but one clearly stood out at being a best place for me to start and that is [this one](http://davidchristiansen.dk/tutorials/bidirectional.pdf). It's a very gentle intro to the subject and with a few visits to youtube & wikipedia I was able to get through it.

One take away from it is that we can drive the process with a couple of functions `infer-type` & `check-type`. `infer-type` takes a form (code) and an `environment` (explained below) and works out what the type of the form is. `check-type` takes a form, an environment an expected type, it then infers the type of the form and ensures it is 'equal' to the required type. The environment mentioned above is the object that stores the mapping between names and types, so if you define an `int` variable named `jam` then that relationship is stored in the `environment`

Unless I've massively misunderstood this paper this sounds like the start of a pretty simple api. Let's fill in the gaps.

First we need to be able to define our own checker. Lets make up some syntax for that:

    (defchecker simple-checker
      :fact-type simple-type)

It will define a class for the environment and a class for our types. We could then define a couple of methods:


    (defmethod infer (code (env simple-checker))
      ...)

    (defmethod check (code (env simple-checker))
      ...)

I specialize the methods on the type of the environment, who's name matches the name of the checker we defined.

Let's now say we want to compile this code:

    (let* ((x 10)
           (y 20))
      (+ x y))

First, we expand the code (I have made a expander that get's it in a form that is useful for my checking)

    (let ((x 10))
      (let ((y 20))
        (funcall #'+ x y)))

My system the walks the code trying to find out facts (types) about the code and replacing each form with the form `(the <some-fact> <the form>)`. The system knows how to handle some of the fundamental lisp forms like `let`, `funcall`, `if`, etc but for the rest the `infer` & `check` methods are going to be called. For example `infer` is going to be called for `10` and `20` but the system will handle adding the bindings from `x` & the result from `infer` to the `environment`.

The result could look like this:

    (the #<simple-type int>
         (let ((x (the #<simple-type int> 10)))
           (the #<simple-type int>
                (let ((y (the #<simple-type int> 20.0)))
                  (the #<simple-type int>
                       (funcall (the #<simple-type func-int-int> #'+)
                                (the #<simple-type int> x)
                                (the #<simple-type int> y)))))))

where each of these `#<simple-type int>` is a instance of our `simple-type` class holding some info of the type it represents.

This type annotated code will then be the input to a function that turns it into valid common lisp code. The simplest version of this would simply remove the `(the #<fact> ..)` stuff from the code but a more interesting version would convert the type objects into lisp type names. So something like this:

    (the (signed-byte 32)
         (let ((x (the (signed-byte 32) 10)))
           (the (signed-byte 32)
                (let ((y (the (signed-byte 32) 20.0)))
                  (the (signed-byte 32)
                       (+ (the (signed-byte 32) x) (the (signed-byte 32) y)))))))

This is valid lisp, if you tell lisp to optimize this code it will produce very tight machine code. `[0]`

Usually lisp doesnt need anywhere near this number of type annotations to optimize code but having more doesn't hurt :p

The result of this could be that we have a dynamic language where we can take chunks and statically type it, with checkers of our own devising, gaining all the benefits in checking and optimization that we would from any other static language.



.. That is of course if it works.. I could be talking out my arse here! :D


I need to do some more reading before diving back into this and I really should do some work on my PBR stuff again. So I will leave this project until next year. I'm just happy to have finally made a start on it.

Seeya



[0] Yet again, this is a trivial example but the idea extends to complex code. The advantage of having a readable post vastly outweighs being technically accurate
