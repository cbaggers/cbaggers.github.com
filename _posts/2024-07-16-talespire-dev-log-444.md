---
layout: post
title: TaleSpire Dev Log 444
description:
date: 2024-07-16 19:57:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I'm back! Having ten days to get a proper mental reset was very nice. I've barely been online (except to download things to read), so I'm definitely out of the loop, but I'll get caught up to speed this week.

To give myself something fun to jump back into, I looked at a performance problem that has been on the list for a while.

### The problem

Picking in TaleSpire is pixel-perfect. This is critical for moving in crowded spaces. We don't pick based on physics colliders as a good collider doesn't necessarily match the shape of the object exactly (if you are curious, do check out [this dev log](https://bouncyrock.com/news/articles/talespire-dev-log-254)).

Instead, we render pickable things from the scene in a specific way and read back the thing the mouse is over the very next frame. That "very next frame" bit is important as we don't want things to feel sluggish. However, reading back data so soon is generally not how the GPU would prefer to work. If you try to read data back from somewhere the GPU was using, it wants to wait until it's done everything it was doing, and this causes a huge delay. Luckily, MJP's post [over here](https://www.gamedev.net/forums/topic/691724-gpu-write-cpu-read/5354495/) explained a feature in DirectX that we can use to read data back the next frame without stalling the rendering.
We've used this for ages, and it's been great, but there are cases where it has not worked.

My laptop is a Thinkpad with an integrated GPU (Intel UHD Graphics 620), and it runs terribly on an **empty board** in TaleSpire. I got out the profiler and saw this:

![Profiler graph showing fetching of picking result taking 22ms](/assets/images/laptopPerfIssue0.png)

Jumpin' Jesus, what is going on with `PixelPicker.FetchSingleResult`?

### The theory

If I recall correctly, Unity will try to overlap the rendering of frames (this is a good thing), so my theory is that something is touching the buffer that holds the picking results too soon, causing a stall.

Mitigating this could be really simple. We can just add a second buffer and switch between them each frame. That way, we don't get any accidental prodding of the resources.

### The result, and what next

Let's not beat around the bush.

![Profiler graph showing the picking delay has now gone](/assets/images/laptopPerfIssue1.png)

Yeah, that's much better. The time to fetch the result is insignificant now (from 22ms down to 0.003ms or something in that area).

Of course, the frame rate is still far too low. There are a lot of things that need to be done to make it run well. But a win is a win, and this needed to be fixed so we could see the other issues. I'm happy with this.

We will be shipping this fix in the next couple of days.

So what next? I'm not exactly sure. I've got to get back to some partners now I'm back, and I need to get caught up with community happenings. However, I will do some more profiling and start thinking about what I want to tackle next on the performance front.

I'll also look into the next big feature I'll be starting work on. But more on that another day.

Until next time.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
