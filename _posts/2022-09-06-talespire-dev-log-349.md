---
layout: post
title: TaleSpire Dev Log 349
description:
date: 2022-09-06 21:28:58
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

With AOEs in Beta, and Ree working on the visuals, I have turned to the next feature we want to ship: group-movement.

A good while back, Ree got this feature pretty much to the finish line. We were just missing a selection method that felt good. For our tests, we used the same rectangle section tool from the building tools.

<iframe class="video" src="https://www.youtube.com/embed/KLNrGoxkX8I?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

As much as you lovely folks tried to tell us the selection would be fine, we are still very stubborn about the feel of TaleSpire, and so you've had to wait. More recently, however, we think we've figured out how to make a pixel-perfect lasso tool that updates in real-time. So we are currently working to find out if it's correct. The general idea is that we can render the on-screen creatures to the picking buffer, reusing the depth buffer from the scene render. By setting the depth test set to "equals" and turning depth-write off, we early out of all hidden fragments (which helps with performance).

I've tested the rendering portion, and it's doing what I expect, so I've turned to the lasso itself. We need to make a mesh that is like this…

![lasso](/assets/videos/lasso0.gif)

…but filled in. This means triangulating an arbitrary polygon. Luckily there are techniques for this, such as [ear clipping (the linked video is ace)](https://www.youtube.com/watch?v=QAdfkylpYwc) but many only work on ["simple" polygons](https://en.wikipedia.org/wiki/Simple_polygon). Simple, in this case, means no "holes" and no self-intersection. There are algorithms out there that can handle these complex cases, but after drawing a lot of squiggles in my notebook, I *think* Ι can make something more efficient[0]. That is my goal for the next few days. After that, it should be mostly copying and modifying existing code to get the results back from the GPU.

That's all from me for now. I'll be back when there is more to show :)

Ciao.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] In the game, we are building the polygon incrementally over many frames and don't have to handle "holes." So I think a decomposition to simple polygons can also be spread over many frames. After that, the ear clipping approach can be applied.
