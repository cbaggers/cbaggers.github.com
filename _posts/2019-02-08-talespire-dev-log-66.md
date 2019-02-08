---
layout: post
title: TaleSpire Dev Log 66
description:
date: 2019-02-08 08:25:53
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks. A quick one today. With both some life business and serialization fixes out of the way it is finally time to get back to tickets. People have been amazing at filing them so the fact I haven't been triaging these for a week sucks. I'm gonna get through reproducing as many as I can today so I can then focus on fixing next week.

@Dwarfs has been killing it making tonnes of progress updating all of the tiles to the new sizes and fleshing out the sets to include everything in the test tile set we made available in the last update. These will all count as new sets as, because of dimensions changes we can't just swap then out with the originals without breaking your existing boards. Of course that will be coming with the floor changes but until then we want to keep everything working.

@Ree has made a pile of progress on props. As they will become their own class of object they can now have their own behaviors. This includes being able to be placed in way more place, orienting to the surface they are placed on and automatically 'dodging' out of creatures way if the creature steps in the same tile as some props. He's also started on the floor UI as by nailing that we can then start on the deeper engine changes.

All go but mostly behind the scenes. We can't wait to show you more though so thanks for joining us on this journey.

Peace

p.s One thing we haven't made time for yet is tweaking the emails so they are less likely to end up in spam. We will be doing this for the next invite push though. When we do that we will re-email all codes to all of you who are already meant to have access.
