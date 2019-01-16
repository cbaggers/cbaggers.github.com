---
layout: post
title: TaleSpire Dev Daily 55
description:
date: 2018-12-17 23:26:33
category:
tags: ['TaleSpire', 'BouncyRock']
---

A fairly productive day today.

- Cleaned up client & backend code for Steam signup/signin. Feels good now
- return validated alias from backend on signin
- tweaks to how creature's memories of boards are handled for serializing
- refactored how and when unique creatures leave board and when they are name non-unique
- tweaks to summon creature to allow specifying the location it will be summoned to.

I also had been working on the fog of war to fix a bug where doors & walls flush against a zone boundary would not be shown if the character with on in the adjacent zone. I was pretty happy with how it was behaving earlier but I must have had some spurious data as now I'm seeing some very odd cases where tiles aren't appearing. Beats me how that happened but I'll need to fix that up in the morning.

I'm also off to Jonny's tomorrow so we can work a bit closer of a few bugs & features. Remote working is great but occasionally it really helps to jump on the train and yell at the same monitor :)

Peace
