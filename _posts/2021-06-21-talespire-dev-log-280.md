---
layout: post
title: TaleSpire Dev Log 280
description:
date: 2021-06-21 21:52:07
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

We are currently working on the last things before we'll be ready to ship the 'props on bases' feature. This is a super exciting one, and I expect it to be used and abused in very interesting ways.

Last night, however, I took a little detour to look at a bug that has been around since the chimera build. Lights that turn off when you get close to them.

Our lights attempt to be the same as the standard Unity lights, except without using Unity's GameObjects for performance reasons. This means that, like Unity's lights, ours use a different material depending on whether the camera is inside or outside the light's area of influence[0].

The lights seemed to be turning off because we were not always updating the material at the right time. Let's get into the weeds a little.

To avoid using GameObjects, we use [CommandBuffers](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html) to render the light meshes at the right point during the frame. For dynamic lights[1] we have to rebuild the queue each frame[2] as the light's position or properties are being changed, but for static lights, it's different. Static lights, by definition, aren't changing, so we have a separate CommandBuffer for them which we only update when the board is modified.

> An important detail here is that you can't just update an element of the CommandBuffer. It has to be cleared and rebuilt

This approach has a very dumb mistake in it, though. When the camera moves, the material the light is using might need to be changed. I'm not sure why I didn't notice that while writing the system, though I expect I was rushing for Early Access and forgot. Regardless, this needed fixing.

Internally the TaleSpire board is split into zones, and we apply operations across these zones in parallel. Each zone communicates with the light-manager to enqueue its lights into the CommandBuffers of lights to be rendered[3]. What I tried to do was have one CommandBuffer for static-lights per zone and then only update them dependent on the camera position[4].

This worked, but I noticed something annoying when I looked at the profiler. The update time for the static lights was fine, but the rendering took a significant hit.

This is my test scene. It's 4096 static lights. I've not added anything else so as not to confuse things.

![lights](/assets/images/lightPerfScene.png)

Here is what we have for light rendering before the fix (so with one CommandBuffer for static lights):

![old version](/assets/images/lightPerf0.png)

And here is the same scene, same camera angle, etc, but with one CommandBuffer per zone:

![new version](/assets/images/lightPerf1.png)

Doesn't that suck? Even though the amount of work to do is the same, the overhead from many CommandBuffers made things take significantly longer[5].

I still hope to ship the fix for the lighting bug this week, but I'm going to have to look at it after we ship the 'props on bases' feature, as it's clear I've got more experiments to do.

There is still plenty to try. What I'll probably start with is switching to three CommandBuffers. One for dynamic lights, one for static lights from zones that have had to update recently, and a final one for static lights from zones that haven't been updated in a while. This way, we minimize the overhead from CommandBuffers while also minimizing the number of lights being rewritten to the CommandBuffer each frame.

Alright, that's all for now. Can't wait to be back with more.

This is gonna be a fun week.

Seeya!


[0] More or less. This is close enough to be able for this discussion.
[1] Dynamic lights are lights being moved or animated by scripts on the tile/prop
[2] Yup, there are places we can optimize here, but this log skims over that detail as we are focused on static lights
[3] This isn't the exact architecture, so don't sweat these details. We just want to talk about the issues.
[4] Of course, this is really about the camera position in relation to the area influenced by any of the lights in the zone.
[5] I'd recommend not caring too much about the wall-clock time in this case. Of course, 2 ms matters, but this is also running on a fast CPU. What really stings to me is that it was significantly faster before.
