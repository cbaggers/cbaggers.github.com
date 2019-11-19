---
layout: post
title: TaleSpire Dev Log 129
description:
date: 2019-11-19 01:07:45
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I've continued working on scripting. My main goal has been to work out the most efficient way to handle the allocation and setup of the private data that each instance of a script is allowed to store data into.

There can be large numbers of tiles made in a single action and so making sure I'm no performing unnecessary copying of data has been important. On the flip side many creation events are of only a single tile so whatever we have needs to make sense at that scale too.

As I progressed with this I kept feeling that the [native collections](https://docs.unity3d.com/Packages/com.unity.collections@0.0/api/Unity.Collections.html) I had available, whilst great, were not ideal for this task so I decided to look into how to write my own. I really wanted something chunked like NativeChunkedArray but with

- faster indexing
- no deallocation of backing chunks unless explicitly requested
- An api more focused on being backing store for the data rather than being focused on working on an element by element basis

The [official documentation](https://docs.unity3d.com/ScriptReference/Unity.Collections.LowLevel.Unsafe.NativeContainerAttribute.html) is both limited and also kind of out of date so I'd recommend starting with [this fantastic article series](https://jacksondunstan.com/articles/4963) by Jackson Dunstan. In fact if you are considering doing any work with Unity's new DOTS systems, trawl that site, it's a goldmine.

Pulling apart his examples and getting to grips with Unity's safety system took the rest of the evening, but it was definitely worth it. I've now got the base of a much more focused chunked store that I can ensure provides exactly what the scripting system needs.

That's all for tonight.

Peace
