---
layout: post
title: TaleSpire Dev Log 256
description:
date: 2021-01-22 00:55:46
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya all!

Today and tomorrow are somewhat taken up by what I'm hoping is the last of the moving business. However, I've had some time this evening to start digging into the per-zone sync.

I started off working out what we would need in a download manager. As I was doing this, I got distracted thinking about the compression and decompression of zones. Currently, we use GZipStream, which has been fine, but it would be way better if it played nicer with Unity's job system and if it could work without creating garbage. I also love that [AsyncRedManager.Read](https://docs.unity3d.com/ScriptReference/Unity.IO.LowLevel.Unsafe.AsyncReadManager.Read.html) gives a job handle as that would allow us to chain the load and decompress if we could work out how to decompress from a job.

I knew that Burst compiled jobs can call native code, so I searched for a simple zlib implementation written in C. I found a library called [miniz](https://github.com/richgel999/miniz), which looked ideal if you use the low-level functions as it doesn't allocate any memory when de/compressing. I then found a .Net wrapper called [net-miniz](https://github.com/jsm174/net-miniz), and after a read of its code, I forked it so we could make a version that works with Unity's job system.

It was pretty easy work, and we now have exactly what we wanted. I've not settled on an API yet, but I'll publish the fork when I've worked that out and tested it a bit.

So it's been a very successful evening. A mention I have more work to do at the old flat tomorrow, but I expect I'll get some time to start looking into database changes we'll need for per-zone-sync and eventual support for sharing boards :)

Ciao
