---
layout: post
title: TaleSpire Dev Log 178
description:
date: 2020-05-13 15:38:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

After shipping the update yesterday, I reviewed all the usages of the data from the `boardAsset` files in TaleSpire. There were clear patterns in the usage that will allow us to pick better data layouts and divide the data that is needed in jobs from the data, which isn't. This description is pretty vague, but basically, it's good news :)

I'm going to play a little with non-burst-compiled jobs in Unity as they are going to be useful in the future when I need to do simple processing on non-blittable types, but don't want to have to introduce a sync point.

I've also started looking back into a bug, which forced me to hold off from fog-of-war before the beta release. I had a case where rendering to a cubemap would result in the top and bottom faces being flipped. I didn't report it at the time (which was dumb on my part), so I spent the first part of today reproducing the issue and submitting the issue to Unity for review.

You can find the repro that shows the issue [here](https://github.com/Bouncyrock/potential-cubemap-descriptor-bug). Luckily it seems this issue does not occur when using a different `RenderTexture` constructor as reported by this lovely person [here](http://answers.unity.com/answers/1709964/view.html). With this knowledge, I can dig back into this and see if I can make more progress on fog-of-war again.

I can also give an update that the last bug we reported to Unity has been successfully reproduced internally, so odds are good that we will see progress on that in the future.

That's all for now. I'll probably make a more detailed post on the fog of war implementation once I have some more news.

Peace
