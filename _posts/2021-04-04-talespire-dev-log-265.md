---
layout: post
title: TaleSpire Dev Log 265
description:
date: 2021-04-04 23:05:18
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

It's been a pretty heavy few days since the release. The beta went from feeling stable back to feeling very beta, and so we've been working away trying to get things back to stable as quickly as possible.

The upgrader and crashes have been number one on my plate. It is a gut-punch to see people not be able to play and, so I've been pushing hard to get those fixed. I introduced new bugs in the process, which really compounded that gross, 'pit of the stomach' feeling. No data was lost, but it sucks to worry people that they might lose their creations.

The bug was in the upgrader and is now fixed. The next update will re-introduce the downgrade button.

As before, the downgrader is only helpful for restoring certain things. As of today, it can fix:

- Missing hide volumes: in the case you upgraded and they were missing
- Boards which were empty after upgrade (potentially except for creatures)

The next patch will also fix the bug where, if a board crosses a certain number of lights, all the lights go dark.

Next, I'll be looking into some of the layout bugs resulting in particular objects (like doors) being at the wrong angles. I'll also be continuing work on any issues related to the upgrader.

I hope you are all keeping well,

Ciao.


p.s. Oh, and Happy Easter for those of you that celebrate it :)
