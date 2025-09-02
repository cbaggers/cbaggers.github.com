---
layout: post
title: TaleSpire Dev Log 481
description:
date: 2025-09-02 17:27:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Howdy all,

Today, I've been back working on our GPU particle system, which will eventually be used for our weather effects.

Our artists will need to be able to script the particles' behavior, so we've gone down the very tried-and-true route of having them make these scripts via a node graph.

![](/assets/images/particleGraphWip.png)
*WIP version of the node graph. Not final*

A while back I wrote a good chunk of the compiler and, although I've got more to add, it's already producing decent compute shaders. Since then we've been working on making the user exerience for the artists nicer.

The current task I have for this is adding support for allowing the artists to make their own nodes (essentially functions), which themselves are defined as node graphs.

Hopefully, this'll only take a few days. After that, I think I'll be back on dice. We'll see how it goes.

Until next time folks.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
