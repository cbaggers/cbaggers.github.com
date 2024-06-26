---
layout: post
title: TaleSpire Dev Log 226
description:
date: 2020-09-16 13:27:14
category:
tags: ['Bouncyrock', 'TaleSpire']
---

A quick tip when dealing with [BatchRendererGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.html). The instance limit per batch is 1023, but the UNITY_INSTANCED_ARRAY_SIZE limitation still applies[0], and so on desktops, this will likely cut your max batch size down to 500. This matters if you rely on instance-ids. I saw some very odd stuff when a single batch went over 511 instances; what was happening was that Unity was splitting the batch in two, one for the first 511 instances and one for the last one. This meant the instance-id for the 512th instance was 0 rather than 511, and this naturally resulted in incorrect behavior when using it to index into a buffer.

That was an afternoon of pain explained after a quick trip to the frame profiler, I was so concerned that my batching code was wrong that I didn't consider the instance-ids not being what I expected. I can't wait to know more of Unity's internals as these issues are such a pain and seem avoidable.


Another node that I added last week is the random node. It can produce random floats, int, and vectors of each. It uses a simple stateless random function I've used in shaders before, which means it would be terrible for security, but is perfectly fine for visuals. The node has an option called 'Update Mode' that might be worth mentioning.

![random node](/assets/images/randomNode.png)

By default, if you give the node the same input (the seed), you are going to get the same result on the output. You will probably feed time into the input to get different results each frame. However, if you are using global time, then the output will be the same for all instances of this script running on all assets this frame. That might not be what you are going for! This is where the 'Update Mode' comes into play. You can set it to global, which gives the behavior described above, or you can pick one of two 'local' options, which both modify the seed in a way that will give different results. The 'Local' option means that it mixes in the tile's unique id, so now the result will be different from other tiles running the same script. Note, though, that each asset within a tile can have a script, and maybe you don't want those assets to get the same result from the same node. To handle this case, you have 'Asset Local', which also mixes the asset's index into the seed along with the tile's id.

Alright, back to work.
Seeya!

[0] Unity's use of constant-buffers for its object-to/from-world matrices and constant-buffers have a max guaranteed size of 65k. See [section 1.4 over here](https://catlikecoding.com/unity/tutorials/rendering/part-19/) for some interesting details.
