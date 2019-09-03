---
layout: post
title: TaleSpire Dev Log 126
description:
date: 2019-09-03 10:31:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Current dev work is focused on the new building systems.

My goal is to have the basics of the data model worked out locally by the end of the week. Most of this right now is focused on how best to handle undo/redo so that it's fast but also simple to synchronize. I had previously designed had some schemes for this but some tools we'd like to use are [currently still in preview](https://issuetracker.unity3d.com/issues/dots-ecs-hybrid-renderer-materialpropertyblock-instanced-data-does-not-get-correctly-updated) and some would require significant engineering to work around (In this case driving rendering ourselves would require a lot of work on the lighting system) and we don't have time for that.

@Ree has been getting back into prototyping the bit that players interact with, the tools and game feel of building. Progress is being made in both areas but nothing worth showing yet.

Thanks for dropping by! More updates soon.
