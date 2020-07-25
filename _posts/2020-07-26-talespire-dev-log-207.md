---
layout: post
title: TaleSpire Dev Log 207
description:
date: 2020-07-26 00:52:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good evening all. For the last few days, I've been struggling with the graph UI and how I want to serialize the scripts. The TLDR is that I'm making progress again, but I'm about two days behind where I wanted to be.

One afternoon was spent trying to understand some odd behavior I was seeing when dragging assets onto nodes in the spaghet graph. I was able to make a decent reproduction of the issue, and the library author was incredibly quick at fixing it.

The graphs themselves are usually stored as [ScriptableObjects](https://docs.unity3d.com/Manual/class-ScriptableObject.html) in Unity. This is ideal for the state-machine scripts, which I want to share between tiles, however, it did not fit my design for the realtime-scripts, which are closely tied to the assets the manipulate. I struggled with the serialization logic here for about a day, and I'm not satisfied with it yet. Regardless I need to make progress with TaleSpire, so I need to crack on.

Today I've started work on the UI, which lets you pick the script for a tile and then assign a realtime-script to each state. It's looking something like this right now.

![setting up behaviors](/assets/images/twScriptSetup.png)

Basic, but enough to make progress.

Once I have this information serializing, I will finally start writing the new asset format. That will let me switch back to TaleSpire and rewrite the asset database. It should then be faster and compatible with the job system, which opens up some performance improvement possibilities in the future. However, we won't be chasing those particular performance improvements, as we need to write the new animation system first.

Back with more real soon.
