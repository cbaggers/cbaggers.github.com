---
layout: post
title: TaleSpire Dev Log 340
description:
date: 2022-04-29 17:47:02
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo folks!

We've been chugging away on fixes and features as usual. With a long weekend coming up, we've been working hard to make sure things are as stable as possible for those days.

HeroForge made minis from packs available to TaleSpire yesterday. This rollout was not perfect, and we uncovered some details that had slipped by us. We've been chasing down issues that resulted from that, and once we push out a small patch later today, we should have workarounds for the most egregious bugs.

Next week I will be looking to improve the speed of fetching the HeroForge data. Currently, it's pretty slow for large collections.

While fixing the above and the other TaleSpire bugs, I've also started work on "move-rulers." These will be a feature you can control locally, and the first version looks like this:

![move rulers wip](/assets/videos/moveRulers1.gif)

This should be a nice, lightweight complement to tactical rulers.

Not much work remains for the move-rulers feature. I need to add some UI for toggling it on or off and to clean up a few edge cases.

Until next time,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
