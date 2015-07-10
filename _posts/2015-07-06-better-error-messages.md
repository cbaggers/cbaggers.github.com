---
layout: post
title: Better Error Messages
description:
date: 2015-07-06 11:11:57
category:
tags: ['lisp', 'cepl']
---

Been a bit quiet recently but yesterday I spent the day working on conditionals in cepl.

There are simply too many cases where the error is cryptic, or fails way later in the process that it should have.

Defpipeline should now through more sensible errors now and varjo will try and suggest types if you use one it doesn't know. e.g.

    VARJO> (defshader test ((x sampler2d))
             (texture x (v! 0 0)))

gives

    Varjo: Varjo: Could not find the correct type for type-spec SAMPLER2D

    Perhaps you meant one of these types?:
    :usampler-2d
    :sampler-3d
    :sampler-2d-shadow
    :sampler-2d-rect
    :sampler-2d-ms
    :sampler-2d-array
    :sampler-2d
    v-sampler-2d
    :sampler-1d
    :isampler-2d
    :sampler
       [Condition of type UNKNOWN-TYPE-SPEC]

    Restarts:
     0: [RETRY] Retry SLIME REPL evaluation request.
     1: [*ABORT] Return to SLIME's top level.
     2: [ABORT] abort thread (#<THREAD "repl-thread" RUNNING {1005078003}>)

I have finished cleaning up type errors for textures and will be focusing on c-arrays & fbos after this.

That should hopefully stop the cases where Ι assume that cepl is buggy rather than checking my own typos :D
