---
layout: post
title: Bug Month Patch 6
description:
date: 2024-05-23 20:23:29
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today, we tackle a bug that has thwarted the pasting of certain kinds of slabs.

When a slab that combined thin tiles and props was copied, this bug made it impossible to place the slab back on the grid correctly. It would be misaligned.

The following shows the difference before and after this patch.

![gif showing difference of slab pasting before and after fix](/assets/videos/alignmentFix0.gif)

Along with this, we fixed a case where a malformed slab could put tiles in unsupported rotations. This also allows for some data optimizations later on.

That's all for this today. We will be back on Monday with Seats and more fixes.

*BUILD-ID: 14497681 - Download Size: Win / Linux 2.9 MB / Mac OS 6.7 MB*

Ps. We are not going to make a habit of releasing on the weekend. This fix was going to ship on Thursday, but our main build server decided to freak out and mess up the end of our week.
Remember, computers can hear you, and as soon as you announce a deadline they will do their damnedest to mess that up :p
