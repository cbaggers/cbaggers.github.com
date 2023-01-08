---
layout: post
title: TaleSpire Dev Log 368
description:
date: 2023-01-08 22:55:25
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

In the last dev stream, we stated that we would start our foray into mods by making an official slab repository (along with an API for user-run sites to hook in too). This gives us some experience using mod.io's API. As of a few days ago, I had this bit working:

![showing a mod in the browser and inside TaleSpire](https://imgur.com/RHv1FPt)

Since then, I've worked on the manager that handles downloading and caching slabs. It's coming along well.

While I expect UX for creature, tile, and prop mods to be similar to our HeroForge integration[0], I think slabs should behave slightly differently. Rather than needing to link/subscribe to a slab to use it, I want you to be able to simply search, then click the icon to spawn the slab. If you want easy access, you can bookmark it to save the slab to your library for future use.

The way things are looking, I should have the manager working mid-week, at which point I'll switch to working on the in-game slab uploader. Woop!

Right, that's all from me for now. 

Ciao.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*


[0] And also share a lot of the same code
