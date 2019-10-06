---
layout: post
title: Cutting Tiles Take 2
description:
date: 2019-10-06 19:44:47
category:
tags: ['Bouncyrock', 'TaleSpire']
---

This weekend Ree and I also took another look at the tile cutaway effect. This one is super important not just to reveal the part of the building you are looking into, but also to cut down walls that would get in the way of controlling your creatures.

> **Please note:** All the following screenshots are not from TaleSpire or even Unity, they are from some test code I was using to play with cutaway effects. It'll look much nicer in TaleSpire :p

We had prototyped this effect before. Here is a sphere we have chopped the top of using the technique.

![c0](/assets/images/cutaway0.png)

The basic approach was that we discard all the fragments that are above the cutaway and then texture the backfaces as if they were positioned at the flat cutaway plane.

However, we saw an issue when the meshes of the models were intersecting (we also saw this in some of the assets on the Unity store for drawing cutaways)

![c1](/assets/images/cutaway1.png)

So what can we do? Well if we look at this with the power of MS Paint this is what we have.

![c2](/assets/images/cutaway2.png)

So let's draw some lines from an imaginary camera point to a few points across the cutaway spheres. Then let's make a score for each line if the line crosses a backface you add 1 point, for a frontface you subtract 1.

![c3](/assets/images/cutaway3.png)

Neat, notice how if the score is above zero then it means you are seeing through the cutaway. Also note that because it's addition and subtraction, we can do it in any order.

We can perform this check for every pixel really easily. Let's render all the backfaces first and additively blend all the fragments. Then let's render all the front faces into the same buffer subtractively blending.

Then we have an image which is the mask for the cutaway area.

![c4](/assets/images/cutaway4.png)

At this point, we have effectively solved the original problem. We can now render the tiles (throwing away the stuff above the cutaway) and then render the cutaway using the mask we have made.

> Note: you could definitely do some nice stuff with stencil buffers to speed all this up.

And there it is, a simple cutaway handling intersections.

![c5](/assets/videos/cutaway0.gif)

Now this may be the version we use in TaleSpire or it may not, we still need to work on it some more and see how it performs once it's in. But either way, it's neat :)

Alrighty, that's all for now.

Peace
