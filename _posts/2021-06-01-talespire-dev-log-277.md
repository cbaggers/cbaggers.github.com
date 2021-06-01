---
layout: post
title: TaleSpire Dev Log 277
description:
date: 2021-06-01 22:00:22
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

For the last few days, I've had an urge to work on bugs, so I put down the creature improvements and dipped back into the issue tracker.

The first bug I ended up fixing was one I spotted while working on other things. It turned out that long error logs were not being truncated when uploaded to the server, which caused the transfer to fail. This was a nice quick one to fix.

Next up, I was curious about a long-time bug where static lights would flicker briefly when placing new static lights[0]. The code was fiddly as the way I was iterating through the assets was good for performance but not for readability. In the end, it came down to some mistakes where I was writing the GPU data for the lights[1].

The last thing I did yesterday was to fix a bug in hide-volumes where, if you make them too small in any dimension, they became unclickable. This was simply that the physics engine doesn't handle boxes where the size in any given dimension is zero. I've modified the tool so that you can't make hide volumes that small anymore.

Today I focused on [one specific bug](https://github.com/Bouncyrock/TaleSpire-Beta-Public-Issue-Tracker/issues/964). As the excellent report shows, there are cases where copying large slabs caused the game to crash. A board was provided, which, along with the instructions, made it trivial to reproduce the issue. The problem came down to an oversight I made while trying to reuse code.

The batching for slabs is heavily based on the code for batching a single zone. This worked great, but I had missed the fact that the batches allowed a max of 13000 instances per kind of object. This was far more than is needed per zone, but for certain slabs it's not hard to go over that limit (50x6x50 single grass tiles, for example). To handle this, I wrote a new struct where the internal array did not have this limit at the cost of some additional allocations[2].

All these fixes are now merged, and so they should ship in the next update.

Until next time,
Peace.


[0] Static lights are ones that do not animate. The crystals are good examples of this.
[1] I tried to expand on the explanation here, but it got too unwieldy to explain without lots of surrounding code
