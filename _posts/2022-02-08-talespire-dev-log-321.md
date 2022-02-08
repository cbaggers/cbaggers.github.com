---
layout: post
title: TaleSpire Dev Log 321
description:
date: 2022-02-08 14:16:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

The weekend was very successful. Ree made significant progress with the UI for the HeroForge integrations, and I worked on the implementation.

I started by fixing an issue causing minis not to be positioned correctly relative to the bases. Next up was scaling. I surveyed our current minis to find reasonable thresholds for picking different 'default scale' values for the miniatures. I need to test more minis with this. It will not be perfect, but it should get us started.

Next, I wrote the client-side code that handles unlinking minis from a campaign. Technically it works, but I noticed potential race conditions between asset conversion and cache clearing. This is obviously unsuitable, so I'm writing a manager to mediate access to the cache. 

Usually, today would be a workday, but as I worked all Saturday and Sunday, I will take today as a break and get back into this tomorrow morning.

Have a good one folks!
