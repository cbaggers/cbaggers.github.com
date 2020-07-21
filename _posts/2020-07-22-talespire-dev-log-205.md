---
layout: post
title: TaleSpire Dev Log 205
description:
date: 2020-07-21 23:58:15
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good evening folks,

It's been another decent couple of days convincing computers to do things.

Most of my time this week has been spent getting to grips with [NodeProcessorGraph](https://github.com/alelievr/NodeGraphProcessor), which so far I'm loving. The main developer has been super responsive, making me even happier about sticking with this library.

I didn't write up yesterday as it was one of those days that you know you need to learn a system very quickly and so you just slam your brain against it all day until you realize why things are the way they are :) A necessary endeavor, but not one that leaves you much in the mood for writing.

Today, however, I've been knocking together very simple nodes for the state-machine scripts that tiles can use. The closest thing I have to a visual is this:

![oo nodes](/assets/video/stateMachine0.gif)

This graph only expresses the state of the tile (or prop in future), and what actions transition between them. To do something visual, you will need the realtime-scripts.

A quick aside. In TaleSpire, tiles and props are things called `BoardAsset`s. `BoardAsset`s are collections of `Asset`s, which act as a single unit. The `BoardAsset` holds the information of each `Asset` required and it's local position/rotation/etc within the unit (along with a bunch of other `BoardAsset` specific data). The cool thing is that a `BoardAsset` may be made of `Asset`s from totally different asset-packs. This means you will be able to remix the `Asset`s that people like us provide.

By separating the two, we can share the state machine among many tiles easily and then, per-tile, choose the visual behavior that occurs during each state by mapping states to the realtime-scripts which live on the `Asset`s.

Codesharing of realtime-scripts will be done through making functions. These will be sharable as strings so that we can all do the same kind of thing that we do with slabs today. That's the idea at least :P I'll be writing that bit soon.

Tomorrow will be focussed on the realtime-scripts. I want to see if I can get the compiler for those working again in the next few days. When that is done, I'll see what the next logical step is. It'll probably be writing the state-machine compiler, which will be pretty straightforward.

Ok, enough rambling for now.

Seeya tomorrow!
