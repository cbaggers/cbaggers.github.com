---
layout: post
title: TaleSpire Dev Log 506
description:
date: 2026-04-21 16:28:29
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

It was lovely to see a bunch of you on the bimonthly banter last week. Since then I've been keeping busy.

Monday was spent on research. I had a terrifying number of tabs queued up, and it was time to work through them! The reading was not focused on a specific feature, but instead is about giving me new tools to apply when challenges appear. I won't link everything I was working through, but here are three I enjoyed:

- [Ray Tracing of Signed Distance Function Grids](https://jcgt.org/published/0011/03/06/paper.pdf) It was really neat to see how to use trilinear interpolation to get the exact ray intersection.

- [H-PLOC: Hierarchical Parallel Locally-Ordered Clustering for Bounding Volume Hierarchy Construction](https://gpuopen.com/download/HPLOC.pdf) We have a lot of things to draw so techniques for quickly building volume hierarchies are always of interest. Also I'm not as familiar with more complex compute strategies so this exposed me to a little of that.

- [Filtering, Convolutions, and Quaternions](https://theorangeduck.com/page/filtering-convolutions-quaternions) This is such a gem. Incredibly well written, and a goldmine of information. Unlike many of my friends, I don't have an audio background, and so didn't get exposure to signal processing until much more recently. This article was just what I needed to start a whole bunch of puzzle pieces falling into place in my head.

So a very fruitful day, all in.

Today I'm back on physics. I'm slowly updating our frontend to the engine, trying not to break anything in the process :) If all goes well this will give a good performance boost to the physics code.

That's all from me today. See you in the next one.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
