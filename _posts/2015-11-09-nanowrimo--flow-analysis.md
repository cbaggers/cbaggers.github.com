---
layout: post
title: nanowrimo & flow analysis
description:
date: 2015-11-09 00:20:13
category:
tags: ['lisp', 'cepl', 'varjo', 'nanowrimo']
---

So more progress today. It's too late to explain this well but Ιm gonna try dashing it out anyway.

So rendering involves matrices everywhere, a matrix describes a transform, and we often define spaces in terms of a transform and a parent space. The odd thing however is that the spaces involved arent stored with the transform so the context has to be maintained in the programmers head.

This isnt entirely accurate as on the cpu side we may have a matrix stack or a scene graph that does maintain this. But we dont have that context on the gpu (and dont want pay the overhead of it anyway).

The question of whether this could be done in a nice way was what prompted by blogpost yesterday.

I started by assuming Ι could have spaces on the gpu but that Ι wanted to be able to upload on the transforms that were needed. This meant knowing which 'spaces' were used in which functions as `(transform space-x space-y vec4)` needs a different matrix from `(transform space-y space-x vec4)`. This very quickly made me realize that I need some kind of flow analysis in varjo; I need to know where all the values can flow through the system.

Today I got the basics of this working and am going to finish it off over the next few days. I will then use this as a way to statically check the validity of transforms in shaders, which should make more of the shaders intention more transparent. Finally as these primitives will only exist at compile time, we get no performance cost at runtime (as by then its just matrices) woo!

right time for bed,
Seeya
