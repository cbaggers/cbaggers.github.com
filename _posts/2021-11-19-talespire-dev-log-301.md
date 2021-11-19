---
layout: post
title: TaleSpire Dev Log 301
description:
date: 2021-11-19 19:43:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I've continued work on HeroForge, but first, let's have a little multiplatform update.


### Mac

The 10Gb ethernet gear arrived and worked brilliantly out of the box. The reason we bought these is two-fold:

- Our m1 iMacs only have 8GB of RAM, and that makes for a poor dev environment [0]
- We already have a nice setup on our windows machines. Given that we can cross-compile, it makes sense to build for Mac from there.

The big problem with building on Windows, however, is getting the result to the Mac. Even though it's not big by modern standards, TaleSpire still takes a while to copy over a 1Gb network. In my experience so far, that delay is enough to hamper mental flow.

Re-enter the 10Gbe kit! I have now got the Windows box and the Mac directly connected via DAC, and I can copy an 8GB file between them faster than I can copy it from HDD to SSD on the same machine. [1]

I tested building directly to the Mac from Unity, and there was no drop in build time compared to building to the local one [2]. This test was the Windows build, however. So the next step was to start experimenting with builds for Apple Silicon.

The TLDR is there are still things to work out. For some reason, while building for Mac locally works, building directly to the remote folder fails silently[3]. Next, my build scripts are not producing the dll containing the Burst compiled code, whereas building from the Unity GUI does.

These aren't too worrying though. Soon enough, we'll have something that allows for fast iteration across these platforms. If it holds up under actual use, we'll order some more for the team members who need them,

Oh, and Unity's profiler works great across the link too!


### Linux

Steam support has been back in touch and answered my remaining queries. The key points are:

- Steam's store has no feature for us to treat running with Proton as the official Linux build (as I understand it)
- It is "not feasible" to bundle Proton with TaleSpire as a means to make a 'native' Linux build, which will show in the store as such.

What that means is that new Linux players will be buying the Windows version. Which is the same as it is now. Naturally, this works fine, but it's a little less official than we would like.

We will simply have to make sure our messaging makes it clear what we officially support. It looks like the store page makes that easy.


### HeroForge

Alright, back to the real work of the day.

The focus of today's work has been two classes.

- HeroForgeDownloadManager: Which is responsible for downloading minis, converting them for TaleSpire, and writing the resulting asset packs to disk.
- HeroForgeManager: Which communicates with HeroForge (via our servers) and handles information about which minis are attached to the campaign.

The complexity comes from the number of moving parts:

- Anyone can add or remove minis from their HeroForge account
- They give/revoke TaleSpire access to talk to their HeroForge account
- They can give/revoke specific campaigns access to specific minis
- This information needs to be available on all clients regardless of whether they are actively playing or arrive later.
- The assets will download at different rates for different people
- There are systems (like fog-of-war) that need specific information about creatures to be on all clients at the same time to ensure that all clients compute the same results

My work today has been focused on making sure the flow of information guarantees that the systems have what they need at the correct times. So that, for example, the FoW system can correctly update vision for a creature even if the asset itself has not yet been downloaded.

It's going well. I've got probably half a day more of wiring things up before I can start testing with real data and find all the places that are totally wrong :D [4]


### Wrapping up

I'm pretty tired now, so I'm calling it a night.

Have a great weekend folks!



*[0] Unity complains about low memory when trying to do something as simple as profile a dev build while the project is open in the editor. This clearly is unworkable. I don't blame Unity too much for this, to be honest. 8GB is just not enough for development tools and games running on the same machine.*

*[1] The network copy took about 12 seconds, IIRC.*

*[2] In fact, it was 8 seconds faster, but I'm assuming that was an anomaly.*

*[3] The build appears to succeed, but there is no file.*

*[4] This will happen. The interconnectedness of all of this has meant it's been a while since the code has been running. The compiler can only catch so much!*
