---
layout: post
title: TaleSprire Dev Log 403
description:
date: 2023-07-25 09:14:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

What the hell. Days go too fast. Alright, let's get caught up.

We are getting the Mac build ready. Testing got delayed by several days because the delivery of the m2 Mac mini took longer than quoted. That was disappointing, but eh, it happens. It was also a good opportunity to work on some bugs.

We'll have a patch out today containing a group of little fixes and a fix for group selection. The group selection issue was very embarrassing as it was much worse than I expected. It was broken in a couple of ways. First off, the stencil buffer wasn't being cleared, which meant every lasso shape drawn would effectively be combined with every lasso shape drawn that session.
Next, the depth buffer was empty, which resulted in group selections selecting creatures through walls.
Both of these things got in when we switched out where we were rendering to allow independent scene and UI resolutions.

I went a little crazy looking at the empty depth buffer bug, as it really looked like it shouldn't have been the case. We were rendering into a RenderTexture with a correctly set up depth buffer, and it was attached to the camera's `targetTexture` property (which is, in our case, how Unity wants you to render into these targets).

Ree saved me by making a minimal test case and finding out that if we ran the commands [AfterFinalPass](https://docs.unity3d.com/ScriptReference/Rendering.CameraEvent.html) instead of [AfterEverything](https://docs.unity3d.com/ScriptReference/Rendering.CameraEvent.AfterEverything.html) the depth buffer was still populated. Unfortunately, this is before the image effects are run, which means things like depth of field affect the lasso.

![depth of field interacting poorly with lasso](/assets/images/dofGs.jpg)

It's not game-breaking, and that's not the usual angle to be commanding larger groups from, so we aren't going to hold back the patch for this issue, but we will resolve it in a future patch.

Next up on the Mac front, I'll look into a weird bug affecting overlay text positioning and our Mac build pipeline.

Have a good one folks!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
