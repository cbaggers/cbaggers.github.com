---
layout: post
title: TaleSpire Dev Log 436
description:
date: 2024-06-04 09:42:50
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks,

Yesterday was spent working towards a fix to prevent cases like the lost build session we spoke of in the last dev log. It’s coming along, but I’m going to give it another afternoon of thought as we can’t afford to get this wrong. It should go into testing later this week.

Yesterday, I also had a long tag-testing session with @chairmander. We picked some creatures and then tagged them using the new system so we could compare our results. As you might have seen in dev-log 434, he’s been working on a set of “blessed tags” and a sort of questionnaire that guides us towards some consistency in our tags across creatures[0].

The testing went well. We found some bugs in our prototype tagging tool, tweaked a couple of questions, and modified a few tag relationships that didn’t hold up under use.

Today, I’m handing this over to a contractor who has not been involved in any of this work. They will then do the same exercise as we did yesterday, and @chairmander and I will see how it works in practice. This is important as we’ve been looking at this problem for a while, so our biases and assumptions are very much baked into the system.

The contractor will then spend a couple of days tagging. We’ll get feedback, make necessary tweaks, and then unleash them on the rest of the creatures. In parallel, and as soon as we are happy enough with the blessed tag list[1], we will upload them to mod.io and start working on some UI for you folks to be able to use them.

Alright, I’d best be off. I need to get a little work done before the daily.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn’t reflect everything going on with the team*

[0] It is possible to add custom tags, but a core set offers consistency and also lets us upload them to sites with fixed tag lists like mod.io.

[1] This list wont be final, but will hopefully represent a good start.
