---
layout: post
title: TaleSpire Dev Daily 40
description:
date: 2018-11-23 20:12:59
category:
tags: ['BouncyRock', 'TaleSpire']
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/TjR_sWK0sLw" frameborder="0"></iframe>

As promised the video of hammering the networked undo/redo. It's pretty stable in the video but I do have one bug in that system that I'm going to fix tomorrow (looks like a simple mistake in when updating the history stack, nothing too worrying).

Today has been focused on asset deleting and making sure that all the subsystems that have to hold references to board assets (like tiles) release them if the asset is deleted. An couple of hours was spent on refactoring the multi-select tool in my ongoing (entirely wholesome) love affair with state machines, its a good deal more robust now and the code to release delete assets is much faster. That latter part is important as any time you have a selection someone else might delete some or all of the tiles you have selected.

There is still a lot more I can get out of this speed-wise but it's not wise to spend more time there given that it's not yet an issue and there is so much to get done for the alpha.

On that subject, work continues. I'm up to 128 tickets on github :p but having gone through the codebase I feel the number of engine related tickets are not going to grow violently more before we release. Lots for the UI though :)

Right. I'm off to chill and watch 'an american werewolf in london', such a classic!

Peace.
