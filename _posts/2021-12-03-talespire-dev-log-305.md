---
layout: post
title: TaleSpire Dev Log 305
description:
date: 2021-12-03 11:42:55
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Morning all,

Since last time I've worked on the polymorph feature, which is going quite nicely.

![](/assets/videos/polymorph1.gif)

We still need UX, better transitions, etc. But the direction feels good.

Photon suffered what seems to be another attack. As the attacks affect specific regions, we pushed a patch to TaleSpire to redirect users to different regions. It is currently just a simple lookup table, but it was the fastest way to progress. The next step on that issue is making server changes to allow region remapping per campaign session.

A couple of weeks back, I did some performance work to improve the handling of large boards. I pushed a build of this to Steam for some internal testing. An issue was quickly found in hide-volumes, so I've fixed that and pushed another build so we can continue the testing. After that, we'll merge that branch and ship it to you folks.

I also found a bug that hide-volumes don't save when placed in a zone with no tiles or props. This should be an easy fix.

Continuing the subject of bug fixing, I am spending this next week fixing reported bugs. I have no strict criteria for which I'll be tackling. If I can replicate the problem, I'll try fixing it :)

That's all for now. I hope you have a good day.
