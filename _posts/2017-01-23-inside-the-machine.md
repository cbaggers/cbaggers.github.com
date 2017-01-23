---
layout: post
title: Inside the machine
description:
date: 2017-01-23 10:42:23
category:
tags: ['lisp', 'varjo']
commentIssueId:
---

I've been back in the compiler this week and it's just been constant refactoring :p

I kicked of the week with tweaks & bugfixes to the first-class function code. I then started on adding some compile-time metadata to Varjo.

The idea is super basic: We want to associate some data with the values flowing through the shader.

We already have `flow-id`s, which represent the values in the shader, so we could just add a hashtable that maps these ids to the data. Simple, and it seems to work!

At the very least it will work enough to make some progress on the idea anyhoo. You are able to add, but never remove metadata. Metadata can also be combined, for example:

    (let ((x (v! 1 0 0 0))  ;; ← lets assume that both these have metadata
          (y (v! 0 1 0 0))) ;; ← with a key called 'foo'
      (if something
          x
          y))

What should be the metadata for the result of the if? Clearly this depends on what the metadata is for. So in the case of conditionals like `if` or `switch` we call a method called `combine-metadata` defined for that metadata type, and it is it's job to decide the metadata for the new result.

This mechanism is very simple but should be enough to get some useful results.

So with this in place I could dive back into re-implementing my `space` analysis feature! Progress.. excitement... Ah fuck it, I really need symbol-macros.

Symbol macros are pretty simple:

    (let ((x some-object))
      (symbol-macrolet ((foo (slot-value x 'foo))) ;; You specify a symbol 'foo' and what it will expand into
        (print foo)   ;; the compiler then replace every instance of 'foo' in your code with the expansion
        (setf foo 20) ;; before it compiles to code
        (print foo)))

So what actually ends up getting compiled is:

    (let ((x some-object))
      (print (slot-value x 'foo))
      (setf (slot-value x 'foo) 20)
      (print (slot-value x 'foo)))

So that should be simple to implement.

*SHOULD BE*

Alas when I made the macroexpander[0] for Varjo I was super lazy. I didn't implement any lexically-scope macros and just expanded everything in a prepass. This really hurt as I suddenly needed to go change how all this was written and mix the expansion into the compile pass.

This refactor took all of Sunday and it was one of those ones where you are changing so much fundamental stuff that you can't compile for an hour or so at a time. As someone who compiles many times a minute this just left me feeling very unsettled. However when it was done the little test suite I have made me feel fairly confident with the result.

Another nice thing about all this is that my macros will now be able to take the compilation-environment as an argument just like in real common lisp. This means I can provide some of the environment introspection features that are described in [CLTL2](https://www.cs.cmu.edu/Groups/AI/html/cltl/clm/node227.html) but sadly didn't make it into the standard. This together with metadata make the macros extremely powerful.

### Next week

The first order of business is compiler-macros, which should be easy enough. Then I want to add support for `&rest` (vararg) arguments to the compiler. Then I will get back intothe `spaces` feature and see how far I can get now.

Peace

[0] back in 2013, holy shit!
