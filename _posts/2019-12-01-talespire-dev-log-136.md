---
layout: post
title: TaleSpire Dev Log 136
description:
date: 2019-12-01 21:30:57
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Phew, so that was the meat of the week's work. Let's wrap up the loose ends.

This is probably skippable as it's just a note of some other things that got changed.

## Moving to Unity.Mathematics

Unity added a new [math library](https://github.com/Unity-Technologies/Unity.Mathematics) recently and whilst having practical benefits, it also results in much nicer code to write and read so we have started using it for all new work.

I took an hour this week to move all the tile data code to use this except for Bounds as I want to make sure that the new behavior matches what I expect.

I also got to delete my Vector3Int32 class as Unity.Mathematics contains int3 and I renamed my Vector3Int16 to short3 and tweaked the API to match that of Unity.Mathematics so it all feels more consistent.

## Realtime changes

One future task that has been making me nervous is updating the realtime networked portion of TaleSpire.

I had a little breakthrough simplifying some of our code that got very spidery and complicated during the development of the alpha. I was going to write a little about that here but I'm gonna save it for another week as it would turn this into another long post.

## Start loading assets earlier

> I probably should have mentioned this in a previous post but oh well.

Unity loads [asset bundles](https://docs.unity3d.com/Manual/AssetBundlesIntro.html) asynchronously. As we don't want to try load every tile in TaleSpire at once, when you first place a tile we need to load it. This results in some frames passing before spawning continues.

This could be exacerbated by the fact that changes are queued as it won't know a tile is needed until it's time to spawn it.

For that reason, we make sure that as soon as the data representation has a new tile kind added, it notifies the asset loader so it can start loading the asset.

This will rarely matter but it may help when there is heavy load, and that's the time you need the most help.

## Merging this monster

I've also started kicking this code into a shape where it's suitable for merging into master. Jonny and I divided up the work so that, technically, neither of us have tasks that block the other. However, we still need to end up with one game and my current monster branch goes against everything I like when developing.

Ah well, soon enough it'll be in.

## Actually the end for realz

Well, that's the week. In that time I was also best man at the wedding of a dear friend so it's been a doozy.

It's probably time for a cup of tea.

Goodnight!
