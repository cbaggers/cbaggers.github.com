---
layout: post
title: NanoWrimo1 - This month's project
description:
date: 2015-11-02 21:25:04
category:
tags: ['lisp', 'cepl', "nanowrimo"]
---

Right, in this post I want to go over what I'm going to be making this month, but first a summary of day 0.

Rewriting [backscatter](https://www.shadertoy.com/view/ltlXDr) in cepl turned out to be a *really* good way to find bugs in the compiler, it was depressingly good at finding them actually. The changes have made the struct handling in varjo much better and made the function dependency code a lot better in cepl.

Whilst the compiler is now producing good glsl, the pipeline crashes hard as soon as you use it :p. Oddly enough commenting out a couple of the uniforms being passed to the pipeline stops it crashing, so I'm assuming I have either screwed up the uniform uploading, or it didn't work before in examples like this.

Either way it is not the primary focus for this month so I am forcing myself to leave it as it is for now and concentrate on nanowrimo.

On that note let's talk about that :)

The project for this month is to add a repl friendly, gc'able, optimizable way of managing hierarchies of transforms in cepl. If this sounds like a scene-graph then that's no surprise, but I'm consciously trying to avoid putting game objects in the graph directly. By this Ι mean Ι don't want to have some 'game-object' type that you have to subclass and that then goes in the graph.

What I would prefer is some means of defining spaces and then having objects subscribe to them. This decouples objects from the means of organizing them.

In this system it is normally wise to have some kind of caching system. This pays off when there are multiple spaces that share a parent. For example if you are calculating the position of each finger on a hand you dont need to recompute the position of the hand each time. Another property that melds well with this is that in many cases you can compute the transforms lazily. You dont need to calculate a parent transform unless the child needs it (or it is needed itself).

Slam this all together and you get a kind of event system. One that caches the latest event rather than propagating them and can be resolved by pulling.

So far so good, but there is one was case I'm interested in. It may be necessary to walk many of the spaces. To that end it would be good to have a flush event that would push all the pending events through the system.

The finale: Input events are just events which always flush. This means it may be possible to merge these event systems to produce a unified interface. One which would allow subscribing to any event. This is basically some odd rx hybrid (with less state safety). Care will have to be taken around the boundaries between different style nodes
but more safe systems can be built on top of this (or it can be ignored and just used to feed some higher level system)

This may end up not working out but it's the experiment for the month and we shall see how it goes :)

Time to go watch a bunch of talks on streams/frp/etc to steal ideas. Wooo!
