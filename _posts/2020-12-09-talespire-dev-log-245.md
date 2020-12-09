---
layout: post
title: TaleSpire Dev Log 245
description:
date: 2020-12-09 03:27:25
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks. I've been a bit quiet recently for a couple of reasons, but I doubt they are that intriguing, so do feel free to skip this next paragraph.

The first reason is easy to explain, I'm moving house, which has been stealing my time. The second reason is more personal. I go through slow swings between needing to produce and needing to absorb content. Recently I switched over to the 'absorb' phase, but stupidly I didn't notice, which meant I was getting very frustrated with my productivity. I don't know why this blindsided me as it's happened a fair few times before, but that's for me to muse over, I guess. The upshot of this was I didn't want to hang out much as I was not happy with what I was producing. However, this last week, I've been coming back out of that funk, and work has been going very well.


Three days ago, I finally got the new cubemap capture system working. This had a lot of false starts as I explored implementing it with CommandBuffers. We needed a custom approach for this because we handle all the culling and rendering ourselves. At first, I thought we could use a replacement shader and the that BatchRendererGroups would render to the camera, but of course, we had set those up in ShadowsOnly mode, so that didn't work. A while back, Ree and I experimented with shadow-only shaders, which could be overridden with replacement shaders; however, the performance hit was significant.

All this meant I needed to write a culling compute shader to fill per-cube-face batches and then to dispatch them. It took a couple of tries to get something I was happy with. What I have should be adequate for rendering the limited regions we need for the character vision.

With that working, I merged my old fog-of-war (FoW) experiments and got them working with the new system. It's not networked synced yet, but that is on the todo list soon.

Over the last couple of days, I've been rewriting the line-of-sight (LoS) system to use the cubemap capture system results. The cubemaps now only store the distance to the nearest occluder, and creatures are not occluders. This means you can now see a gnome behind a giant, for example. It also means creatures don't block FoW reveal, which was a bit annoying in my experience.

The way our LoS works is to render all creatures to a cubemap using a shader that discards all the fragments. This sounds pointless, but what we do instead is to compute the direction from the camera to the fragment and use that direction to look up the distance to the nearest occluder from the cubemap made by the capture system. We can then see if the creature fragment is closer to the camera than the occluder and if so, we record that creature's id in a buffer. We use a compute shader to collate that buffer's contents, and we read it back to the CPU-side asynchronously. What we end up with is a buffer of visible creature ids.

The cool thing with the new system is that we have separated the LoS step from the view cubemap capture. This lets us recompute the LoS multiple times without having to update the view. This happens a lot when the GM or other players are moving their creatures, but yours is stationary.

As a 512x512 cubemap is not an insignificant amount of GPU-memory, I experimented with different schemes of when to free the cubemaps. There are a few tradeoffs:

- Capturing the view is relatively expensive, so you want to do it as infrequently as possible. Ideally, only when the board changes or when the creature, whose view it is, moves.

- If a GM right clicks to check a specific creature's LoS, it's highly likely they will do it again. So we should keep the data around

- However, it's also likely that they will check the LoS of a few different creatures. So we have a risk of making too many cubemaps

The approach I have now will hand out the cubemaps but, assuming they don't become invalid for other reasons, they will expire after some time. Also, if we have more than sixteen cubemaps alive, the manager will mark them for disposal. The amount of time given is less for creatures who are not members of a party[0].

There are a few other details, but that was the meat of it :)

I am now writing a new mesher for the FoW. The Minecraft style mesher we have been using was fine, but it almost does too good a job at reducing the polycount. To experiment with the visuals, we want a finer mesh. As marching-cubes is easy to implement, I'm going to use that for now[1].

I should have that done tomorrow.

Seeya then!


[0] Parties are a concept that will be getting a lot of attention before the Early Access release. LoS and FoW are both party-wide, so you will see what other party members see. I'll talk more about this as we implement it.

[1] Surface-nets would probably be another good option.
