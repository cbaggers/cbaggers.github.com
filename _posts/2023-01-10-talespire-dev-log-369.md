---
layout: post
title: TaleSpire Dev Log 369
description:
date: 2023-01-10 00:30:07
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo again!

I was on the road most of today, but I have a little progress to show. Here is the first slab coming from mod.io directly into the game:

![slab being spawned from community mod library](/assets/videos/slabIo_0.gif)

As mentioned in the last post, unlike chonkier mods like creatures and tiles, we want you to be able to spawn slabs without subscribing to them first. However, I don't like the idea of waiting for the web request each time, so naturally, we're doing some caching.

It works like this. When you first use a slab, it downloads it and saves it as a temporary file (that is deleted on exit). Once again though, loading a file does take a little bit of time, so we also keep the ten most recently used slabs strings in memory.

So when spawning a slab, TaleSpire can use the in-memory cache, fall back to the local file, or just download it.

In the gif above, you can *just* about see the timing difference between the first and second spawn of the slab.

While the code is still currently full of todos and stuff to clean up, I am going to switch to upload now as I want to start filtering by tags, and those are most easily added via the upload API. I'll start tomorrow and see how quickly I can hack something in.

Until then, have a good one folks!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
