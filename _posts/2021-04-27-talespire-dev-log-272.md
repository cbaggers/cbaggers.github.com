---
layout: post
title: TaleSpire Dev Log 272
description:
date: 2021-04-27 10:44:57
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi everyone,

I had somewhat of an extended weekend as I had forgotten I had some real-life things happening on Monday. All in all, it was lovely to decompress few days, and I'm stoked to get started again.

And we are in luck. A wonderful member of the community has managed to create a board that reproduces one of the nastiest bugs we have right now. For a long time, there has something that can trigger data corruption when cutting a board region and then undoing that cut. However, what has been confounding us is that it has not been possible to replicate the problem consistently. Several lovely folks have got us close, but there was always something that made it very trial and error.

We now have been given a published board with three-step instructions on how to break it.

Needless to say, this now makes this bug tractable. The start will be a lot of trial and error as I try and trim this board down to the smallest thing that still shows the bug. Then I'll dive into the data and see what is going on.

Board corruption bugs are the ones that upset me more than any others, so I can't wait to squash this one.

Hope you are all doing well!

Ciao

p.s. In writing this, I've already been able to find out that the issues are not related to cut specifically, but delete. I've also halved the size of the board that produces the issue. We are on our way :D
