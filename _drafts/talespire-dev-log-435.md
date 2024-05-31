---
layout: post
title: TaleSpire Dev Log 435
description:
date: 2024-05-31 20:02:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good evening folks,

Today I was looking into a really ugly slowdown when opening the "community" section of the asset library. I cracked open the profiler and after poking around found that calls to `new Texture2D` were insanely slow, easily 1ms each. I think it might be that either Unity is initializing the contents of texture, or that it can only make a certain number per frame.

The next version of Unity above what we use does have an API call which allows making textures without initalizing them, but the one we use doesn't. I tried upgrading the project to the next version on the off chance everything would just work, but no. There are APIs that are critical to TaleSpire have changed and refactoring is not trivial.

So next I need to decide on a different approach. I could write some c++ for making the textures without initializing them, and call that from Unity. And/or I could spread the texture creation over a number of frames. The first option is trickier than I'd like as I don't think the conversion between Unity's GraphicsFormat and DX11's formats are documented anywhere (hopefully I'm wrong, please leave a comment if you know!)

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
