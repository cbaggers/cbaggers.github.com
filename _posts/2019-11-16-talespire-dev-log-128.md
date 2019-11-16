---
layout: post
title: TaleSpire Dev Log 128
description:
date: 2019-11-16 16:56:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

![/assets/images/spaghet0.png]

Here's a wip shot from something that will appear in a proper update post in the future. On Thursday I wrote a compiler and vm for a little noodle-graph scripting language I'm calling Spaghet.

It runs using no GC on multiple cores and will easily scale to the crazy number of interactive tiles you folks like to use. The graph above (whilst being complete nonsense code wise) can run 10,000 times in less than a millisecond [0]

Naturally the look of the graph, and the nodes available will be improved too, I just needed something I could get working quickly and would give me something real I could test.

I'm working on the code that hooks this into the rest of the board and sync systems next week.

WOOP!

[0] This number is very conservative. There are also tons of things we can easily do as soon as we want perfomance improvements.
