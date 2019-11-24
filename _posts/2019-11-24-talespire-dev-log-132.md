---
layout: post
title: TaleSpire Dev Log 132
description:
date: 2019-11-24 01:23:54
category:
tags: ['Bouncyrock', 'TaleSpire']
---

The last two days touched a few different things.

First off I was working on the resource list for the Spaghet scripts. This holds references to Unity objects in the asset like Colliders and Animators, and also the configuration of radial menus and GM requests.

In implementing this I re-learned a terrible rule of Unity's immediate mode UI. Don't try and make the code 'nice' you'll sink ages into chasing things that look like they would help only to hit some limitation of the system and have to throw it all away. DONT DO IT. I'm sure I was meant to have learned this lesson last time, but apparently, that wasn't enough.

Once that was good-enough-for-now™ I started looking into reimplementing the doors/chests/etc using the new scripting system. For now, I'm not worried about improving the graph, as I want to switch to Unity's node graph elements at some point, so I just wrote the ops by hand. In the process, I added a few more operations to the language: yield, goto, and an op to send a message to a resource from the resource list.

It's not done yet but after the resource list thing, I was a bit tired of scripts and put it to the side for another day.

On Friday I started off with a planning session where I just try and work out any small details I can across the tasks on my todo list.

One that stood out as needing some work was pathfinding so I've started work on that. The goal for the beta is as follows.

> When you pick up a creature you should be able to see all the places you could walk to within a given range (determined by the creature)

This means we are looking at a kind of flood-fill where we 'walk' something for a certain distance, but what do we walk?

Well, last year I walked an octree of the board itself (we did this for fog of war so the octree was available) and this was hard. This time I want a dedicated graph. I'll write more about this once I've got it working.

And that's the lot for now. It's been a good week and there will be more of these logs next week.

Until then, peace.
