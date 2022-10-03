---
layout: post
title: TaleSpire Dev Log 354
description:
date: 2022-10-03 21:21:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks! I've been quiet this last week, so let's remedy that now.

## Group Movement

The hard work for the lasso is now done! By this, Ι mean that we can draw out a lasso and get pixel-perfect info of what creatures are underneath it back from the GPU.

<iframe class="video" src="https://www.youtube.com/embed/UGhJw-Ym_DM?feature=oembed" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

> The text at the bottom of the video shows the IDs of creatures as they get selected. I was trying to show the pixel precision, but it's not sure clear. The red lasso graphic is not final (which is probably obvious)

I am now resurrecting Ree's group-movement code and wiring up the lasso selection. If all goes well, Ι hope to have something to show by the end of tomorrow. 

After that, the focus is on cleanup, testing, and a proper shader for the lasso itself.

At that point, we'll get this into a Beta. While in Beta, I'll be focussing on bugs and performance. 

## macOS support

This is looking good. Ree has worked out the shader problem, but some legwork is still needed to fix it. 

I'm pretty confident we'll have macOS support in Beta within the next couple of months.

## Other stuff and things

Based on this dev-log, you'd be totally justified in thinking not much is going on. While the opposite is the case, it would, unfortunately, be unfair to natter about it just now, so Ι won't. I am really hoping to be more forthcoming in the next dev stream.

As for the dev-stream, we can't set a date yet as Ree is currently off sick. So no news on that yet either :|

Not the most satisfying way to wrap up a dev log Ι know. That's why I haven't written until Ι could at least show the lasso progress.

Can't wait to share the rest with you,

Have a good one.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

