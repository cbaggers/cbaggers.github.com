---
layout: post
title: TaleSpire Dev Log 357
description:
date: 2022-10-11 11:29:04
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Yesterday my goal was testing the group movement with multiple clients and, as mac support is progressing, I decided to use that for the testing.

To do this I updated our native plugin to support the lasso and broke out the profiler to check that the implementation was going to be viable on Apple Silicon. Luckily enough it ran perfectly, but as I looked at the profiler I got curious about the somewhat poor performance on medium sized boards. This led me into a day of prodding, profiling, and remembering how much I hate dealing with XCode.

The TLDR is that the M1 iMac currently struggles to render the shere amount of stuff we regularly put in our boards.

That of course won't be the end of the story. I bumbled my way through getting an XCode build of TaleSpire that allowed for GPU profiling and the captures contains reams of data giving clues as to where we can speed things up.

We are going to need to do a lot of experiements but the work we do there will probably benefit lower-end PCs too.

There is more I can ramble about, but I'll get to that when I'm working on mac support again so I'll spare you that for today.

Alright folks, now I should get back to testing.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
