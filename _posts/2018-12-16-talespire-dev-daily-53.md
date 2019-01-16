---
layout: post
title: TaleSpire Dev Daily 53
description:
date: 2018-12-16 01:41:28
category:
tags: ['BouncyRock', 'TaleSpire']
---

I've enjoyed today.

We (the TaleSpire team) had a quick meeting to look at what was feasible in the days remaining. We are still going to try the closed alpha out on the 31st so it's all hands (all 6 hands :D) in order to make that. It's gonna be tight but it could work. Minimal viable product is the name of the game.

Campaign history is in a very basic working state and so I jumped back to looking at unique creatures. The UI had a few requirements I hadn't met yet so I did a bunch of work getting that in shape. I finished that about half an hour ago. In short the list of data describing the unique creatures in the campaign can be used to spawn the previews we use when placing creatures. When the preview is placed we spawn the actual creature object at that spot passing in the 'unique creature info' this let's us populate all the correct values (name, hp, etc) and tell the backend that the creature has moved boards.

UI is getting tighter as Jonny is hammering at it night an day. We are not going to have the final look by the 31st but we will be iterating on it pretty hard (along with everything else) in the new year.

Tomorrow my primary two tickets are getting the Steam signup/login finalized for the alpha and fix the 'sync on quit' feature. If neither of those cause any drama then I'll probably look into a rather cute fog of war issue. Currently there is a case where if the obstruction object is on the boundary of a zone it may not show, which is terribly confusing when it's a locked door :p

Goodnight folks!
