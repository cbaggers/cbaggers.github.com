---
layout: post
title: TaleSpire Dev Log 392
description:
date: 2023-06-14 09:18:33
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Whoops :D

I released a little more than I expected to yesterday. Let's talk about that.

I aimed to release some UI we could use during maintenance to make the process nicer for players. However, I forgot to disable a few unfinished things before shipping, so you folks got a little peek.

> Big thanks to Ree, who cleaned up my mistakes as I was on the road when you alerted us to this issue.

The first thing that you might have seen is this:

![community mod browser](/assets/images/communityMods0.jpg)

This is the first version of the 'community mod browser.' It's where you'll be able to find community-made slabs, symbiotes, etc, hosted on mod repositories. It's currently only pulling from our mod.io repo, but that will be easy enough to expand once the core is complete.

As some of you saw, you can click on a slab to bring it immediately into the game, making the flow of building with community creations even smoother.

Aside from some more testing and adding separate categories for symbiotes and boards, this part is relatively close to shipping. The reason we haven't is due to the next thing we accidentally shipped.

![ui for uploading slabs from inside TaleSpire](/assets/videos/slabShareUI.gif)

This is the unfinished UI for sharing slabs from inside TaleSpire. Very soon, it will be as easy as selecting your creation and hitting a few buttons to share your slabs with the world. We are starting with uploading to mod.io but then want to expand to uploading to other mod repositories

That said, it's clear why this hasn't shipped yet. The camera controls are borked, the UI is unfinished, and there is no way to set up your mod.io account.

There is *one* part that works, however. You'll notice that, when entering the upload UI, TaleSpire isolates just the selection. This is very handy for seeing what is selected, which can be tricky in busy scenes and can help avoid sharing more than you mean to.

This "isolation mode" is a helpful tool in its own right, so we have made it available in the building tools menu.

![issolation mode](/assets/videos/issolationMode1.mp4)

Also, this feature already works despite being an accidental release, so we've left it in. At least that way, you folks get something useful out of my screw-ups!

Today I'm back squashing bugs in Symbiotes. I have an annoying exception occurring when switching to and from dev-mode.

Catch ya in the next dev log.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
