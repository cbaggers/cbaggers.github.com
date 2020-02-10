---
layout: post
title: TaleSpire Dev Log 150
description:
date: 2020-02-10 18:16:38
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, yesterday I got the Spaghet that run our doors and chest hooked up and syncing correctly.

Spaghet is our little scripting language we made to deal with the question "if someone drags out a 30x30 slab of scripted tiles, how much resources are we using (both CPU & networking)".

Scripts come in two kinds:
* state-machines: Their progress is driven by user interaction and is synchronized across the network.
* realtime: Which run every frame and are unsynchronised

Each script gets a small chunk of private storage (currently 32bytes) and runs on top of unity's job system, which means we can run them concurrently across cores with no GC. We can also trivially pass the state of a script across the network and, with a little book-keeping, reapply it on the other side.

Currently, we are only using 1 script (the one that controls doors and chests), but we have a lot in place to be able to expand on this as we head through the beta. For now, I'm just happy to see it working.

Today after finishing some Spaghet work, I switched to looking again at copy-paste. Here I'm just trying to get it to the point that it is spawning the copied slab as expected without worrying about the feel of the tool. After that, we can look at this as more of a UX task.

Hmm, I think that's all for now.
Back with more tomorrow
