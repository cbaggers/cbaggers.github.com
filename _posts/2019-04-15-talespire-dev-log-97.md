---
layout: post
title: TaleSpire Dev Log 97
description:
date: 2019-04-15 23:02:45
category:
tags: ['BouncyRock', 'TaleSpire']
---

Good [time adjusted] evening to you all,

Today I've been keeping busy with bug fixes and some small workarounds. These changes will be out in the next update (hopefully wednesday or thurday)

- We now show the name of the selected creature along with the player name in the GM UI
- We clear the history panel when switching boards
- Fixed a case where, if you controlled many creatures and clicked on their image in player mode it might focus the wrong creature
- Fixed an ugly exception that would occur if you disconnected after having used the turn based mode
- Fixed a case where the first GM joined after other non-gm players and the gm would be put in cutscene mode
- Tweaked the number of tiles updating per frame. We still get ugliness in large, multi-floor boards but this makes it a tiny bit more playable whilst we find a better solution.

We also pushed out the next 900 invites, so thats 1900 in two days which feels nice.

Now to get fixing so we can get another batch out!

Peace

![creature name](/assets/videos/nameBadge0.gif)

p.s. I'm not going to get much done tomorrow as the machine I use for streaming and testing has died so I need to sort that out first.
