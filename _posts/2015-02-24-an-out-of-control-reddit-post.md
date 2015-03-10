---
layout: post
title: An out of control reddit post
description:
date: 2015-02-24 09:32:48
category:
tags: ['lisp', 'cepl']
---

I tried to write a short reply to someone on extending a language with macros...I failed...it got long

This was the thread: http://www.reddit.com/r/lisp/comments/2wy18r/is_this_what_people_mean_when_they_say_lisp/

--------------------------------------------------

[note: this assumes you're a lisp newbie, sorry if this is patronising]
Is it what 'THEY' mean when they say Lisp allows you to invent your own language. No, not really. But it is nice to be able to use symbols normally deemed off limits right? But syntax is part of language so what you are doing is in the sphere of language. Let's have a look at a macro:

    (defmacro fn^ (name args &body body)
      `(defun ,name ,args
         ,@body))

Ok so first let's see what it does and then how it does it.

A macro is a function that runs at a different time, weird right?! It the above kind is run *before* your program is compiled. the arguments will be source code and the return value will be new code to go in place of the old code. So this:

    (fn^ some-func (x) (* x 10))

-becomes-

    (defun some-func (x) (* x 10))

See what happened there? The macro was give the following arguments:
name -> some-func
args -> (x)
body -> ((* x 10))

if not you may want to read up on 'backquote syntax', it is (aproximately) a very tidy way of splicing lists and data together.

Ok so now we have functions and this kind of macro (yes there are other kinds) we can image that we redefine every common contruct in common lisp.

lets say: let becomes val<-, make-hash-table becomes hsh^,

(f^ fnc (%)
    (val<- ((x (hsh^)) (y :dflt))
           (<> % x)))

Well it's clearly still lisp but it's very unfamilar (and ugly! :D). While this may affect language design it doesnt feel like a new language. Let's do something else with these macros

    (defmacro val<- (&rest form)
      (let* ((body (last form))
             (bindings (butlast form))
             (b (loop for i below (1- (length bindings)) by 2 collect
                     (list (elt bindings i) (elt bindings (1+ i))))))
        `(let ,b
           ,@body)))

so now

    (val<- ((x (hsh^)) (y :dflt))
           (<> % x))

could be writen as

    (val<- x (hsh^) y :dflt
           (<> % x))

OK! so now this is different. This feels more like changing the language as we messing with how the code behaves.
The issue is...now we are messing with language we not only have the normal problems a programmer deals with but also with those of a language designer. These are issues of feel and experience as well as functionality (it's very like api design but with much deeper implications), the answers are often subjective and based on what the language is designed to solve. A big one for me is 'Does the programmer have to think more to acheive something using this'.

In my own project I use macros to make defining glsl shaders feel like defining regular lisp functions.

    (defshader some-name ((x :int))
      (+ x 10))

And translate the lisp code into glsl for the programmer.

We have only scratched the surface of what we can do in this little blogpost. There are also other kinds of macros, like reader macros for example. Reader macros are function that run before the macros we have seen already. They don't recieve the code as lists of symbols, they recieve the actual characters from the code you wrote. They can allow for cool and often crazy stuff, all the of literal syntax you see around lisp is made usign reader macros: ' ` , #() . etc are all made possible using reader macros, and you can extend that in any way you can concieve.

I hope this was of some use. Sorry it turned into a whole post rather than a comment!

p.s.
Also check out:
http://enthusiasm.cozy.org/archives/2013/07/optima  <- macros give CL pattern matching
https://gist.github.com/chaitanyagupta/9324402 <- Info on reader macros
Look symbol-macrolet <- this type of macro swaps out a symbol for another whole form
