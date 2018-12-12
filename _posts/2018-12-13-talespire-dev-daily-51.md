---
layout: post
title: TaleSpire Dev Daily 51
description:
date: 2018-12-12 23:52:24
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today we landed the new UI, we are now hooking it up and squashing bugs.

The tickets are rather specific but here are a few as the daily so far is a tad short :)

- Start hooking up the campaign invite panel
- Allow a lone Gm who is in player mode to perform actions that would normally require Gm approval without the request.
- Add hooks for providing strings to the session history panel (only the lua state-machine scripts are using it right now)
- Handle more cases where board loading can be interrupted
- Tweaks to the radial menu to make it feel nicer (such an insightful daily)
- Change under what circumstance we focus the camera on a creature (For some reason I had it set up to focus on the first time you selected any creature)
- More things that are so specific to the internals that it would be meaningless

Ok. That's enough that I'm not entirely wasting people's time by posting this.

Tomorrow we'll be working on more of the same. I also need to look at the code to react to a player's rights (e.g. if they can gm) being changed during the session.

Peace
