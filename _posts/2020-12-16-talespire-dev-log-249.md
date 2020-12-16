---
layout: post
title: TaleSpire Dev Log 249
description:
date: 2020-12-16 18:19:53
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good-evening folks,

I'm happy to report that the new lighting system for tiles and props is working! We now have finally moved away from GameObjects for the board representation.

As it looks identical to the current lights, I'm not posting a clip, but I'm still happy that that bit is done[0]. In time, we will want to revisit this code as there is still more room for performance improvements. However, there are bigger fish to fry for now.

After finishing that, I squashed a simple bug in the physics API where I was normalizing a zero-length vector (woops :P).

Next, I'll probably be looking at the data representation for props. I will be a bit distracted though, as I'm moving house and in the next few days, I need to get a lot done.

I think that's all I have to report for today.

Seeya around :)


[0] Technically I still haven't worked out Unity's approach to tell if a camera is inside the volume of a spot light. This means my implementation is a little incorrect, but it's not going to be an issue for a while.
