---
layout: post
title: TaleSpire Dev Log 222
description:
date: 2020-09-07 02:02:20
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

Ree has been working full steam on the flying mechanic, and it's looking pretty damn nice. You should get your hands on it very-soon™ if all goes well.

Work on assets has been going well too. We have some good stuff we hope to show there soon-ish also.

Before the weekend, I tried to get more dice working. However, the D12 was spinning relentlessly, so I just put that down for a bit. I feel like we are past the big hurdles with that part of the physics integration, so it should be a case of just hunting down those issues.

I also started on porting the animated fire to the new system. This requires some relatively simple changes to the batcher but also means we need to recreate some scripts in spaghet. Here is the noodly goodness as it stands now.

![noodles](/assets/images/fireScriptGraph.png)

This will definitely be cleaner when we support collapsing subgraphs into function-nodes, but it's not too bad even now. We are sampling a few curves in order to:

- pick the active mesh
- rotate the object which holds the light
- adjust the light's intensity

For the curious, this compiles down to a short sequence of operations.

![ops](/assets/images/fireScriptOps.png)

I still need to switch from local time to global time and add a random-number node to add some timing variation between each fire instance. I believe I still need to do a little work to handle some cross-frame state, but this hopefully won't throw any spanners in the works.

Tomorrow I'll pick up where I left off and try to get this lot running in TaleSpire. Except for lights, which are their own can of worms.

Seeya then.
