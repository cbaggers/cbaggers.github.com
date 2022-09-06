---
layout: post
title: TaleSpire Dev Log 348
description:
date: 2022-08-31 19:43:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Work on AOE[0] feature has been going well. In fact, if all goes to plan, we should have a Beta in your hands by the weekend.

<iframe class="video" src="https://www.youtube.com/embed/FcWRFhAe5MU" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

The goal for the Beta is to test the basic functionality. What we have is as follows:

- While using a ruler, a GM can press a key[1] to turn that ruler into an AOE
- Players and GMs can use the Tab key to view the names of the AOEs
- GMs can right-click on the handles of an AOE or bring up a menu that lets them delete or edit the AOE
- AOEs are saved in boards.
- They are also included in published boards.

For now, the visuals of the AOE are exactly the same as the rulers. This will probably change before the feature leaves Beta.

Yesterday was spent making a fast way to place and update the names above the AOEs. The result is a slightly generalized version of the code I use for bookmarks and creatures. Now that I have this, I'd like to go back and try replacing the creature and bookmark implementation with this new version, but that's a task for another day.

As you can see, the names in the above videos are just dummy values. Today I'll be adding the code for setting and changing names. I expect that to be a quick job so getting the Beta out by Saturday seems feasible.

I guess we'll know soon enough!

Until next time,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] AOE = area of effect
[1] The keybinding is configurable in settings
