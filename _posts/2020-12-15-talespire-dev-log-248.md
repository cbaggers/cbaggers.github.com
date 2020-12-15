---
layout: post
title: TaleSpire Dev Log 248
description:
date: 2020-12-15 01:13:22
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I've been working on the new light system.

The basics of this are that I am porting our [previous experiments allowing lights without gameobjects](https://bouncyrock.com/news/articles/talespire-dev-log-243) to our new branch and hooking it into the batching code. There are, of course, lots of details to work out when trying to make something shippable.

As usual, we need to think a little about performance. There are often many lights on screen, and each light in Unity takes a one draw call (when not casting shadows, which these don't). Each frame, we need to write each light into a CommandBuffer[0]. With Unity's approach to deferred lights, A light may be rendered with one of two shaders based on where the camera is inside the light's volume or not. Two matrices need to be provided, and the light color seems to need to be gamma corrected[1]. As CommandBuffers can only be updated on the main thread, I want to do as little of the calculation there as possible. Instead, we will calculate these values in jobs we have handling batching, and then the only work on the main thread is to read these arrays and make the calls to the CommandBuffer. This fits well as the batching jobs already calculate the lights' positions and have access to all the data needed to do the rest.

If all goes well, I hope to see some lights working tomorrow.

Seeya then.


[0] we need to use a command buffer as the light mesh has to be rendered at a specific point in the pipeline, specifically [CameraEvent.AfterLighting](https://docs.unity3d.com/ScriptReference/Rendering.CameraEvent.AfterLighting.html)

[1] This one was kind of interesting. When setting the intensity parameter of the light, the color passed to the shader by Unity changed. In my test, the original color was white, so the uploaded values were `float4(1, 1, 1, 1)`, where the `w` component was the intensity. However, when I changed the intensity to 1.234, the value was `float4(1.588157, 1.588157, 1.588157, 1.234)`. It's not uncommon for color values to be premultiplied before upload (see premultiplied alpha), so I just made a guess that it might be gamma correction. To do this, we raise to the power of 2.2. One (white) raised to the power of anything will be 1. So instead, we try multiplying by the intensity and raising that to the power of 2.2. Doing that gives us `1.58815670083..etc` bingo!
