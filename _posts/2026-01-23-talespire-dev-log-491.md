---
layout: post
title: TaleSpire Dev Log 491
description:
date: 2026-01-23 12:56:50
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey all,

Decent progress yesterday .

* Fixed cases where curves connecting nodes were laid out incorrectly during certain inputs
* Dedicated types for various internal IDs
* Added the ability to drag/drop resources from the Unity project directly into the canvas.
* Improved debugger integration
* Users can now replace connections without removing the old ones first.
* Added a way to specify default values for connectors
* Got user-defined functions to compile correctly when used in other effects.

The drag/drop one is simple but it’s nice to be able to drop a texture into the graph and get a texture node immediately. The same is true when dropping in a user-defined function asset.

![drag-drop texture asset](/assets/videos/effects-graph-0.gif)
![drag-drop function asset](/assets/videos/effects-graph-1.gif)

The connection replacement, too, is a nice quality-of-life improvement. Previously, connectors could be turned into value slots with a right-click action. However, based on feedback, we’ve moved to showing the value slot whenever a connection is not made. It doesn’t always make sense to have a slot though, for instance, you wouldn’t want to type a texture!

![modifying connections and showing default value slots](/assets/videos/effects-graph-2.gif)

Today I’m going to focus on improving resource handling. In the graph, resource-nodes are nodes where the value comes from the CPU, rather than being computed on the GPU.

Seeya you then.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
