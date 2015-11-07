---
layout: post
title: Controlling the compiler is AWESOME
description:
date: 2015-11-07 20:06:42
category:
tags: ['lisp', 'cepl', 'varjo', 'nanowrimo']
---

I really should have gotten used to this idea, with the amount of macro stuff Ι've been doing with lisp, but being able to mess around with compiler is just fun.

In the process of my nanowrimo project on making 'spaces' for cepl Ι've been looking into how these will be handled on the gpu. I want to be able to reason about these more explicitly than just transforming some vector with some matrix, but at runtime Ι need to keep things as simple and with as little indirection as possible.

Whilst on the cpu side I'm living in a (mostly) dynamically typed language, on the gpu I'm fully static. This opens up some possibilities for using/abusing my type system to check these things for me. I am able to add types that exist purely so the typechecker can validate what I'm doing and that will just compile down to a regular glsl vector4.

So one example I could try is:

    (let ((pos (p! some-vec world-space)))
      (transform pos view-space))

Where p! is a function that returns a value with type 'position-world-space'. I can then use the metadata produced by the compiler to work out what matrices (for the various spaces) need to be uploaded. I can also ensure you don't do something potentially meaningless (like adding two vectors in different spaces) without first indication that that was what you intended.

    (let ((pos-1 (p! some-vec world-space))
          (pos-2 (p! some-vec clip-space)))
      (+ pos-1 pos-2))

This is not very interesting though as it's not normally the mistake you make. Something more interesting would be when you are in a fragment shader (which is implicitly screen space) and you want to do some post-proc lighting based in clip-space. You could use a simple block like this:

    (in-space clip-space
      ..your code..)

And you could be sure that any 'position' that was used inside would be transformed to the correct space prior to the calculations being carried out. Any time the spaces match it's a no-op so no conversion code is emitted.


I'm thinking that a chunk to this can be implemented with the compiler the way it is, but for the bits that can't it means I have to add and expose some new compiler features, which is cool.

So that's why I'm excited by compilers today :D

Back to work!
