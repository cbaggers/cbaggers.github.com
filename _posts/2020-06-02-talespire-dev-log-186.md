---
layout: post
title: TaleSpire Dev Log 186
description:
date: 2020-06-02 00:58:13
category:
tags: ['Bouncyrock', 'TaleSpire']
---

TLDR: The first prototype of 3D fog of war started working about an hour ago. Check it out:

<video controls src="/assets/videos/fow0.mp4" type="video/mp4" width="620"></video>

> BIG OL' CAVEAT: This is not the final mesh or shader used on the fog. This just shows that the raw transforms *can* work.

A lot of the leg work over the last days has been managing the pools of cubemaps, buffers and such used in the fog of war system so that we never allocate more often than we absolutely have to. We also have been making sure that no step blocks for longer than necessary and that we have something we can easily tune.

So here is the very rough rundown:

- when you place a creature that you control, the scene (for a 30 unit radius) is rendered into a cubemap holding creature ids and distances from the observer.
- the ids are used by the line of sight system to accurately determine what creatures are visible to the creature you just placed.
- the cubemap (and some other data) is kicked over to the fog-of-war system that works out every cell (a 1x1x1 unit volume) visible for the observer.
- It packs this data into a buffer that is then applied to the zone's fog-mask.
- The updated fog mask is handed over to the mesher which generates a relatively low poly mesh for the fog and sends gives it to the zone to display

Multiple of these can be done at once. The line-of-sight and fog-of-war updates that rely on processing the data in the cubemap are all on the GPU, and any other step that does any kind of data processing is done in jobs dispatched over multiple cores. I've not profiled this yet, but it's feeling ok, and we have the tools to make this quick.

Right now, I'm just stoked this is starting to work. Tomorrow I'll be bug-fixing, and after that, I'll start work on the network sync for these updates. I'll write a dev-log on that problem as it's a fun one too.

We are also getting closer to the creature scaling update. We found a bug in the keyboard movement and we need to get fixed before we ship. We'll keep you posted on that.

For now, I'm gonna go poke this system some more :)

Seeya!

p.s. Here is a pic of a fog mesh. You can see that the mesher does an ok job of cutting down the number of polys from the worst case.

![pew](/assets/videos/fow0.png)
