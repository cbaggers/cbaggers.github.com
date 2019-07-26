---
layout: post
title: TaleSpire Dev Log 120
description:
date: 2019-07-26 08:48:45
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, behind the scenes I've been working on the next version of the undo/redo system along with new data layouts to make batching (for rendering) faster.

Undo/redo is one of the areas we've had serious lag before when working with large slabs of tiles. When looking at the code it was understandable why, however, there are interesting complexities to the system when you have multiple people adding and deleting tiles concurrently.

For the last two days, I've been working on a new design for the system. When doing we also have to make sure that the data layout we choose for this doesn't make it slow to perform frequent tasks like rendering.

This is one of the bits I love with this job, sitting down with a pen and paper and just wrestling with a design.

I think I have something now but I'm not going to put it here yet as it needs some further validation. What I'll do next is code a small prototype so I can see the properties of the system more clearly.

More details on that in the coming days :)

Peace.
