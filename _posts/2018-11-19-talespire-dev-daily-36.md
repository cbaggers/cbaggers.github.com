---
layout: post
title: TaleSpire Dev Daily 36
description:
date: 2018-11-19 21:15:57
category:
tags: ['BouncyRock', 'TaleSpire']
---

After a couple of days of quiet I'm back. Saturday I was just tired and Sunday I wasn't working due to a headache (probably just a bit overloaded).

Today has gone well. I've just spent it working through small issues. Changes to information passed back at login, syncing on exit, fixes to tile visibility after a build action, and more stuff.

Tomorrow I'm either going to hammer on with more tickets from the pile (there are plenty) or I'll look at the synchronizing of the fog of war information. The serializing of that fog of war info is super simple but, as multiple players can 'own' the same creature, I need to have a think about who is in charge of calculating that stuff. Probably fine to just pick something dirt simple like 'the player with the lowest player id'. We can always revisit this.

Alrighty, time for a drink.
