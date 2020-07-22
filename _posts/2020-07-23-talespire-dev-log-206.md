---
layout: post
title: TaleSpire Dev Log 206
description:
date: 2020-07-22 23:50:29
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks. I've now ported enough of my old Spaghet experiments to the new node graph that I can compile code again.

Here is a useless script:

![spaghet0](/assets/images/spaghetCompiled0.png)

On the left, we have some nodes (the node visuals are still WIP), and on the right Visual Studio's debugger showing the result from the compiler. I've indicated the debug representation as it's a bit easier to see the generated operations. The actual result is just a byte array, so I haven't bothered to expand it (spoiler, it's full of bytes).

I've also added some code to check for type errors and to report loops in the graph. This feels like the bare minimum requirements to be able to code in a sane way.

![spaghet1](/assets/images/spaghetLoopError.png)

![spaghet2](/assets/images/spaghetTypeErrors.png)

Tomorrow I'll look into the output nodes. First, I'll add ones for setting an asset's transform and another for driving an animation. Both will let you drag the assets in questions straight from the Unity hierarchy so that it fits the standard Unity workflow.

That's all for now. Seeya tomorrow.
