---
layout: post
title: TaleSpire Dev Log 314
description:
date: 2022-01-07 17:37:56
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks,

Work continues well on the HeroForge Integration.

In the last dev-log, I wrote about a specific issue related to perception updates. I was pretty confident with the solution I had come up with. But then I had a sleep and didn't like it anymore :D

So I reworked that code again, and I'm now much happier with the approach. The TLDR is that in cases where the creature index (some metadata) isn't yet available, we queue up the perception updates. There are, of course, details that made it easier said than done, but the result should serve us well.

Next, I started integrating the miniature metadata from HeroForge. We can pull metadata for each miniature as well as the asset file itself. This contains a bunch of info that we need, such as points of interest like head/s, chest/s, hand positions, etc.

I made a quick first pass that used this data, and it looked good. The first draft helped me spot places that were sub-par[0], so I'm currently rewriting it.

Technically this is all going well.

On the more human side: I needed to visit the police today to finalize my permanent residency. The Norway side of this has been flawless, but dealing with it put me back in the Brexit headspace, along with other thoughts I was happier leaving back in England. This distracted me today, and I decided to work Saturday instead and take the afternoon for some rest and games.

Hope you are having a good day,

Seeya!



*[0] I was only pulling and processing the metadata when pulling the mini itself. If I, instead, handled it before downloading the model data, the game could start using that data without waiting for the whole mini to download.*
