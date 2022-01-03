---
layout: post
title: TaleSpire Dev Log 312
description:
date: 2022-01-03 21:03:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi everyone,

The server upgrade today went well. Since then I've switched to our HeroForge branch of TaleSpire and was able to pull my first mini from production, which was very satisfying.

Now to do all the remaining stuff. After the butt-ugly merge I did yesterday, I have to test everything slowly to ensure I haven't broken anything. I took an hour or so to test basic creature behavior and started writing a manually testing script.

Another big issue I want to deal with early is that we can no longer be sure creature meta-data is loaded before joining a board. This means (amongst other things) that we don't know the head position and thus can't correctly calculate vision when a creature moves. At the very least, this means that we need to push the head position to other clients when a creature is spawned and when a board is synced.

That might be enough, but I need to test the theory first.

After that, it's UI, base conversion, stuff, and also things. But we'll get to all those in future logs :D

Have a great evening folks.

Ciao.
