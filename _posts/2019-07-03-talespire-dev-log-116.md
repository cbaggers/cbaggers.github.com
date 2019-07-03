---
layout: post
title: TaleSpire Dev Log 116
description:
date: 2019-07-03 23:33:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, yesterday and today I have spent getting familiar with Unity's Job Systems and new ECS.

I've written toy ECS' before and am currently working on a little optimizing compiler for querying table data so a lot of things were very familiar. The bulk of the video and prose content for Unity's new systems is focused on the fact that the new thing is 'different, but don't worry it's fast and not that hard' etc, etc. As I'm already sold on the premise this is nice but of limited here. Beyond that, you better be comfortable reading other people's code :p

The best resource so far has been the [ECS Samples](https://github.com/Unity-Technologies/EntityComponentSystemSamples). It's a small of samples evolving a piece of code from a `foreach` on the main thread, through different flavors of jobified tasks. One of the biggest advantages of the code is just seeing what is still in use. If you glanced at the [cheatsheet](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/Documentation%7E/cheatsheet.md) you'd be missing certain things (like sync points).

The reference docs are passable but there are plenty of things where the description of a method is the name. When I hit walls in C# I often read Mono's source to get an idea of what to expect, but naturally many of the things in the new ECS are either proprietary or are in c++ code I can't reach easily so those missing doc-strings become a pain point.

Given that, in TaleSpire, we load most assets dynamically I'd like to avoid having to use the Entity conversion routines on load of each asset, I'm fine with running them in TaleSpire and serializing *something* useful, but I'm not sure how we prefabs play with ECS entities at this stage. I'd be very surprised if Unity's mega-city demo was using conversion routines so I'm sure there is still more to understand here. Also the lack of documentation around the `Hybrid.Renderer` is a pain in the proverbials, I'd really like to look into zone culling in TaleSpire to potentially improve rendering performance but information seems scarce here. I'm sure it's all in the mega-city demo but that doesn't feel like the best introductory material to be using.

Regardless, between the samples and [these docs](https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/ecs_entities.html), I'm making decent enough progress. We'll definitely be using this in TaleSpire and I'll have this under my belt well before the campaign ends.

Peace
