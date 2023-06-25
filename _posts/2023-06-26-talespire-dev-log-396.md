---
layout: post
title: TaleSpire Dev Log 396
description:
date: 2023-06-25 17:34:20
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Work has continued integrating mod.io (so we can download Symbiotes from within TaleSpire), albeit sporadically, as I've had a bunch of folks visiting for midsummer (no ritual deaths involved, in case you wondered :p). The first few days of this week are gonna be a bit hectic for me, so we are shooting for Symbiotes to come out of Beta sometime next week (as in between the 3rd and 7th). I am always hesitant to give dates, as life has a habit of kicking off when I do. But hey, it's been a while, and I'm ready to make that mistake again!

As you've probably seen, we've also been adding logs and gathering details on a bug that has given some folks sporadic difficulties logging in. We've narrowed it down to the request that fetches the game-environment details[0], but we are still working on minor fixes to address the cases that have been reported.

I hope you've had a good weekend.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] The game-env tells TaleSpire the address for the backend along with some extra stuff like maintenance times
