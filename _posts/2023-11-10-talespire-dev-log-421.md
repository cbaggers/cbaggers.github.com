---
layout: post
title: TaleSpire Dev Log 421
description:
date: 2023-11-10 12:07:23
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

After a few weeks of public activity, it feels weird to be quiet again, but things are progressing well.

My big task is restructuring our projects so we can ship the creature modding tools as DLLs.

This task involves some decoupling from Unity, so I've been removing the dependency on Unity.Entities. We didn't use the ECS they provided at all, but they had a zero-copy data format, which was pretty handy.

I wrote our own format the other day, so for the last couple of days, I have been updating all the code to use that. I'll admit that a lot of that time was me trying to find what I thought were bugs but turned out to be me screwing up.

My task for today is to work out how I will handle conditional compilation (e.g. ConditionalAttribute and #if) with this library. We'll see how that goes.

Catch ya soon.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
