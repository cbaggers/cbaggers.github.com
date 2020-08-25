---
layout: post
title: TaleSpire Dev Log 217
description:
date: 2020-08-25 03:40:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

After a lovely couple of days out of town, I've got back to business, and it's been a busy one.

I spent the first half the day on dynamic colliders. For now, these are the colliders that are attached to tiles (and props) and are moved by scripts. This was a somewhat painful process as I ended up bouncing back and forth between TaleWeaver and TaleSpire as I needed to make some tweaks to the code that dealt with the object hierarchies.

![dynamic collider 0](/assets/videos/dynamicCollider0.gif)

![dynamic collider 1](/assets/videos/dynamicCollider1.gif)

With that behaving, I stubbed out the code for computing the motion data required by the physics engine. Finally, I tweaked the per-frame code so that the physics engine knows when it doesn't need to be rebuilding all the internal data for static objects. This should, in normal cases, give decent performance.

For the rest of the day, I switched to slabs. I really wanted to be able to paste large objects from the community and see them appear immediately. Annoyingly they have managed to avoid working yet. The majority of the work is done. I'm just chasing down smaller bugs and mistakes, but it's late, and I need to sleep.

I expect to write up a bit more on this once it's working.

Seeya tomorrow.

p.s. I'll show a pic from the weekend to those curious when I get them from my partner. I'm using a dumbphone currently, and photos are not it's strong suit :D
