---
layout: post
title: TaleSpire Dev Log 355
description:
date: 2022-10-04 21:40:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey everyone.

As hoped, a lot of things came together today, so I'm thrilled to be able to show you this:

<iframe class="video" src="https://www.youtube.com/embed/tP8fdK1G90E?feature=oembed" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

The lasso is finally working!

I've still got plenty to do, but I'm very confident about what remains.

First off, I need to clean up the implementation. Some things are still hacky and/or inconsistent with other tooling[0]. 

After that, the big priority will be performance. The current implementation is abysmally slow, but this was expected. I wrote it in an exploratory manner but made sure not to write code that would be hard to optimize later on[1]. If the code is slow in the ways I expect, then a combination of Burst, threading, and some spatial hashing will solve it. But we'll see once the code is profiled.

After that, the feature should be in the cleanup phase. Tweaks, bug fixes, and testing will be the order of the day[2]. Of course, the visuals for the lasso need making, but they should not impact any other part of this feature.

And that's the lot for today. Tomorrow is going to be another fun one for me, though not as visually dramatic :)

Have a great one folks!


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] I'm being a bit nebulous as explaining it would be clunky without code examples
[1] To me, this mostly means keeping to simple constructs and not writing "clever" code.
[2] I'll also be porting the c++ portions to macOS. But I think there is less than an hour of work to do there.
