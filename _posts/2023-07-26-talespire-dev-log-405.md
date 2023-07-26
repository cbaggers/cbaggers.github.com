---
layout: post
title: TaleSpire Dev Log 405
description:
date: 2023-07-26 11:51:12
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

Coding days like yesterday are great. Issues are clear and don't get weird when prodded, and fixing them is smooth.

I fixed up a bug around the positioning of bookmark names, font issues on Mac, some bugs on scene transition, and a mistake in our Mac build process.

Today my focus is on the Mac builds produced by our build server. The first issue was that I was putting the assets in the wrong place, which was an easy fix. It also let me dip my toes into writing Jenkins files which was... well, I won't say nice, but it's good for me to do!

Now I'm looking into a Mac bug where it seems like the root camera is null. Oddly, I've not had this in any of my Mac testing so far. It's likely that I've only been deploying dev builds though so this may be specific to release builds. I'm gonna check that first. Hopefully, this is simple.

Alright, I'm too excited to keep writing. I gotta get back to this.

Seeya!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
