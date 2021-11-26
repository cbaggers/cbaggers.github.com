---
layout: post
title: TaleSpire Dev Log 303
description:
date: 2021-11-26 14:38:54
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey everyone,

I've continued my HeroForge work and got to the point where spawning HeroForge minis works. Which is great. In this video, you will see both the asset-pack files appearing after conversion and the creature being spawned.

So that I didn't get distracted by UI, I just knocked up an inspector in Unity to list and spawn the linked minis.

![hf asset loading](/assets/videos/hf_wip.gif)

The next problem is that this code has a data race. It only works if the HeroForge mini information comes back from the server before the board loads. We could try delaying everything, but this is a symptom of a larger issue, how to handle missing creatures.

Modding and HeroForge integration both expose this problem. Up until now, asset packs have had an index, which we can load fast and gives us creature info, and asset-bundles which hold the mesh, textures, etc., and load slower. We were okay with asset-bundles loading relatively slowly as we made systems that could work with only the data from the index. However, we can no longer rely on the index being available, or at least not in a timely fashion.

To this end, we have to take our code one step further. Instead of just handling the delayed arrival of information from bundles, now we need to handle even more basic information being missing. Things like default-scale, name of the kind of creature, head-position, and many more.

Head position is actually a critical one as, without it, we can't do line-of-sight checks or fog-of-war updates. We have to be very careful here to minimize the chances of differing results on different clients. I have some ideas, but I'll save them for another day.

Today I am doing the big refactor to the creature management, spawning, and vision code. Hopefully this goes smoothly.

Until next time,

Peace
