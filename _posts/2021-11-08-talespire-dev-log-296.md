---
layout: post
title: TaleSpire Dev Log 296
description:
date: 2021-11-07 15:31:37
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Blimey, what a week.

I've been looking into converting heroforge models into a format we can consume. For our experiments, we were using some APIs that were either only available in the editor or not async and thus would stall the main thread for too long.

We get the assets in GLTF format, so we used [GLTFUtility](https://github.com/Siccity/GLTFUtility) to do the initial conversion. We then needed to:

- pack the meshes together (except the base)
- resize some textures
- pack some texture together
- DXT compress the textures
- Save them in a new format which we can load quickly and asynchronously

The mesh part took a while as I was very unfamiliar with that part of Unity. Handling all possible vertex layouts would be a real pain, so we just rely on the models from HeroForge having a specific structure. This is a safe assumption to start with. Writing some jobs to pack the data into the correct format was simple enough, and then it was on to textures.

We are packing the metallic/gloss map with the occlusion map using a shader. We also use this step to bring the size of these textures down to 1024x1024. To ensure the readback didn't block, I switched the code to use [AsyncGPUReadback](https://docs.unity3d.com/ScriptReference/Rendering.AsyncGPUReadback.html).

This did get me wondering, though. The GLTFUtility spends a bunch of time, after loading the data, spawning Unity objects for Meshes and Textures. Worse, because then use [Texture.LoadImage](https://docs.unity3d.com/ScriptReference/ImageConversion.LoadImage.htlm) it has to upload the data to the GPU too, which is totally unnecessary for the color and bump maps as we save those almost unchanged.

So I started attempting to modify the library to avoid this and make it more amenable to working with the job system. 

Images in the GLTF format are (when embedded in the binary) stored as PNGs in ARGB32 format. LoadImage previously handled that for us, so I added [StbImageSharp](https://github.com/StbSharp/StbImageSharp), tweaked it so as not to use objects, and wired that in instead. 

Unfortunately, the further I went, the more little details made it tricky to convert. Even after de-object-orienting enough of the code and making decent progress, I was faced with removing functionality or more extensive rewrites. I was very aware of the time it was taking and the sunken cost fallacy and didn't want to lose more time than I had to. I also noticed that some features in GLTF were not yet supported, and integrating future work would be tricky. 

As I was weighing up options, I found [GLTFast](https://github.com/atteneder/glTFast), another library that supports 100% of the GLTF specification purports to focus on speed. I had to rejig the whole process anyway, so it was an ok time to swap out the library. 

In the last log, I talked about porting stb_dxt. stb_dxt performs compressions of a 4x4 block on pixels, but you have to write the code to process a whole image (adding padding as required). I wrote a couple of different implementations, one that collected the 4x4 blocks one at a time, and one that collected a full row of blocks before submitting them all. The potential benefit of the latter is that we can read the source data linearly. Even though it looked like I was feeding the data correctly, I was getting incorrect results. After a lot of head-scratching, I swapped out my port of stb_dxt for [StbDxtSharp](https://github.com/StbSharp/StbDxtSharp) and was able to get some sensible results. This is unfortunate, but I had already reached Friday and didn't want to waste more time. If we are interested, we can look into this another day.

Over the weekend, I did end up prodding this code some more. I was curious about generating mipmaps as the textures included didn't have any. Even though the standard implementation is just a simple box filter, it's not something I've written myself before, so I did :) 

A bit of profiling of the asset loading shows mixed results. Reading the data from the disk takes many milliseconds, but we'll make that async so that won't matter. The odd thing is how long calls to [Texture2D.GetRawTextureData](https://docs.unity3d.com/ScriptReference/Texture2D.GetRawTextureData.html) are taking. I'm hoping it's just due to being called right after creating the texture. I'll try giving it a frame or two and see what it looks like then. The rest of the code is fast and amenable to being run in a job, so it should mean even less work on the main thread.

The processing code is going to need more testing. GLTFast is definitely the part that takes the longest. Once again, the uploading of textures to the GPU seems to be the biggest cost and is something we don't *need* it to do… unless, of course, we want to do mipmap generation on the GPU. It's all a bit of a toss-up and is probably something we'll just leave until the rest of the HeroForge integration code is hooked up.

So there it is. A week of false starts, frustrations, and progress. 

Have a good one folks!
