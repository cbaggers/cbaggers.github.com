---
layout: post
title: TaleSpire Dev Log 295
description:
date: 2021-10-27 09:41:11
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

This morning I'm continuing my work on the HeroForge integration.

My current tasks revolve around loading the assets. For Dimension20, we had knocked together an importer which did the job but has two issues that don't make it suitable for use inside TaleSpire:

- It didn't need to worry about blocking the main thread
- It used some functionality from [UnityEditor](https://docs.unity3d.com/ScriptReference/UnityEditor.html), which is not available at runtime.

This means we need to get coding :)

The first target to replace was [EditorUtility.CompressTexture](https://docs.unity3d.com/ScriptReference/EditorUtility.CompressTexture.html), which we use to compress the textures to DXT5/DXT1. You might at first think that you could just replace it with [Texture2D.Compress](https://docs.unity3d.com/ScriptReference/Texture2D.Compress.html), but it doesn't have the same quality settings and (much worse) is not async.

So I went looking elsewhere. Luckily for us, the fantastic [stb single file library project](https://github.com/nothings/stb) has [just what the doctor ordered](https://github.com/nothings/stb/blob/master/stb_dxt.h). stb_dxt is small, fast, and easy to read; however, it is in C, and while I could just include a dll, it looked like it would be easy to port to Burst's HPC#.

So that's what I got up to the other day. It was a straightforward task[0], and now I just need to write the code that drives the compression process[1].

Today my focus is on converting the format we get from HeroForge into something suitable for TaleSpire. We need to extract what we need, apply compression and store everything in a format suitable for fast async loading[2].

That's it from me for now. I'll be back with more as it develops.

Peace.




[0] I'll probably release that code once I can confirm it's all working.

[1] DXT compresses 4x4 pixel clusters, so stb_dxts' API takes one cluster and appends the results to a buffer. The code to provide the clusters (with correct padding) is not part of stb_dxt.

[2] A small amount of work still needs to be done on the main thread as creatures use Unity's GameObjects. However, most of it can be done without blocking.
