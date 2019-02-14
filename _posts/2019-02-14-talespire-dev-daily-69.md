---
layout: post
title: TaleSpire Dev Daily 69
description:
date: 2019-02-14 23:17:05
category:
tags: ['BouncyRock', 'TaleSpire']
---

It's that time again, today was spent:

- Finishing off the fix for [If GM permissions are rescinded while any GM-only UI is visible, they stay on-screen](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/66)
- Case where game spuriously entered cutscene mode when GM rights were removed from a player
- Initiative mode wasn't updating the edit UI for remote GMs when local GM changed the line-up
- Reordering of lineup was not handled correctly in the edit UI
- Fix `UIList`'s `Clear` method which was causing order issues when new elements were added.

Tomorrow I think I'm going to look at that fog of war issue causing corners not to appear. The first part is visualizing the points that are sampled for fog of war info and then we'll see where the bug takes us.

Peace.
