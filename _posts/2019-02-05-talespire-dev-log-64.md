---
layout: post
title: TaleSpire Dev Log 64
description:
date: 2019-02-05 22:43:30
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today I finished testing the fixes for serializing board-assets with state (doors, chests etc). There will be one side-effect when this goes out and that is all door/chests etc will revert to their default state.

This side effect is a trade-off. We are fundamentally changing how that code worked and so to handle this change we would need to do a of work and testing to port the current state over. This would be the **only** choice once we have released but whilst we are at this point in the alpha we want to focus on getting more content out there and that is blocked until this fix it out.

So yup that's all for today. I've lost some work time due to sorting out some health stuff and tomorrow will be most taken up by that too. However in the evening I will be prototyping the start of the tile cutaway shader in my lisp stream so that'll be fun.

Until tomorrow,
Peace :)
