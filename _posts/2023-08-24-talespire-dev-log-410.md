---
layout: post
title: TaleSpire Dev Log 410
description:
date: 2023-08-23 23:18:24
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Well, we are nearly there. Depending on where you are, we are shipping Mac support today or tomorrow. I've just signed a release candidate build and have it primed on our pre-release Steam branch.

Here's a fun shot from our recent testing. TaleSpire running on Windows, Mac, and Linux, side-by-side and in the same board.

<video controls autoplay loop>
  <source src="/assets/videos/threePlatforms.webm" type="video/webm" width="100%"/>
</video>

> There simply must be a "three men walk into a pub" joke here somewhere, but I can't find it.

And here's a slightly clearer shot

![](/assets/images/threePlatforms.png)

In the last week, I've been working on:

- Platform-specific keybinding support
- Updating the cut volume to work with all tiles and props
- Tweaks to improve reconnection behavior
- Allowing Symbiotes to subscribe to creature selection changes.

These will all ship alongside the Mac release, so even pure Windows parties will get something out of it :)

That last one can potentially allow some very cool Symbiote integrations as creators can now react to the player switch between their characters in real-time. It's pretty speedy with group selections too.

<video controls autoplay loop>
  <source src="/assets/videos/symbSubSelection.mp4" type="video/mp4" width="100%"/>
</video>

Right, I best get ready and rested for the release.

Have fun folks!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
