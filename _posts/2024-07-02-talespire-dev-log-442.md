---
layout: post
title: TaleSpire Dev Log 442
description:
date: 2024-07-02 18:54:15
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again folks,

In [yesterday's dev-log](https://bouncyrock.com/news/articles/talespire-dev-log-441), I talked about us working with [TitanCraft](https://titancraft.com/) to help you bring your creations into TaleSpire. 

The first thing they would like to do is allow their users to export their miniatures in the TaleSpire creature format. To that end, I've been working on a command-line tool they can use in their export pipeline.

Today, I fixed a bug in the handling of thumbnails, and suddenly, we had TitanCraft creations in TaleSpire.

![Some miniatures from TitanCraft inside TaleSpire](/assets/images/titanCraft0.png)

These are looking very much at home! [0]

The rest of the day was spent on tweaks and fixes as we tried more minis.

Tomorrow, I'll be looking at scale. TaleSpire has some limits on scaling due to our current movement code not being enjoyable once things get really big. As with our HeroForge support, we will need to massage the input to work well with our system. It's something we've done before, though, so it shouldn't be too hard.

That's all for now,

Peace

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*


*[0] Please note that these visuals are not final, we will likely tweak the shader before this feature ships.*
