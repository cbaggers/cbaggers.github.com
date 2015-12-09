---
layout: post
title: Flow analyzer complete enough
description:
date: 2015-11-23 10:12:42
category:
tags: ['lisp', 'cepl', 'nanowrimo', 'varjo']
commentIssueId:
---

So this week I got the flow analyzer working WOO! There are a few cases it's missing but the hard stuff (iteration) is done and there's enough that I can start looking at how to use it

So a refresh:

- On the cpu side I have a tree of transforms representing different spaces (like a lightweight scenegraph)
- I want to use these in a sane way on the gpu
- I cant afford any runtime logic becuase then we are pissing performance up a wall.
- Matrices describe transforms between spaces. We are interested in the spaces yet we have no explicit way of handling them.
- If we could use spaces in our shaders and at compile time turn them into matrices (that are then uploaded as uniforms) then things would be much clearer.
- To be able to resolve this at compile time we need a flow analyzer...which we now have, yay!


So the next question was how to use this.

`position` is a point (vector) with a `space`. Position is a compile time construct which will be compiled to a regular vector

`space` will be the name of the type. Spaces will be passed up as uniforms

    (defun-g test ((vert :vec4) &uniform (world space))
        ...)

I was imagining a `transform` function that let you transform a position from one space to another:

    (transform pos world-space clip-space)

This is kinda dumb though as the position knows what space it is in. So maybe

    (transform pos clip-space)

Hmm, might be ok. What would be sweet though is a scope where everything inside is guaranteed to be in a certain space

    (in world-space
      (some code)
      (more code))

wait though, this makes `transform` redundant as

    (in clip-space pos)

Would transform the pos to clip-space, and it would return as it's the last thing in the scope. Sweet!

`in` is also nicer to write than transform so I think we have a winner.

Note on making positions: in cepl/varjo vectors are made with the `v!` function. As in `(v! 0 1 2)` returns a vector3. Position will have a similar interface but it will expect a space, either as an argument or taken from the current `in` scope. So `(p! 0 1 2 world-space)` gives you a position3 in world space. `(in world-space (p! 0 1 2))` does the same (I may kill the first version so `in` is the only way to do it)

So assuming this worked how does cepl use it?

My current thinking is that cepl will iterate over the function calls looking for calls involving spaces and positions. It then works out the matrices needed for each calls, de-duplicates the result and adds them as uniforms. Intermediate calculations need to be added to the source code (which I havent worked out the details for yet) and then the new lisp shader code (now without any spaces or positions) is recompiled. The result of this second pass the the shader uploaded to the gpu.

I'm off to america this week so I have no idea if I will get any of this done. We shall see :)
