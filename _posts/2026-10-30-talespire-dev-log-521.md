---
layout: post
title: TaleSpire Dev Log 521
description:
date: 2026-10-30 09:08:39
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya.

The last few days have been dotted across various projects. Fonts, group-spawn, notes, and inventories.

Work on the inventory has centered on backend and (rough) UI design. We understand what we want for the frontend and have nailed down the backend api. So we'll be moving ahead on that asap. I'll share screenshots as that progresses.

For notes, I've done enough testing to show we can take markdown text and produce some UI to display it, along with embedded content like images and tokens. As I only needed to prove to myself that it was feasible, it currently looks like hot garbage and isn't even part of the main TS codebase. I'm still stoked, however, as I'm not aware of anything that should slow me down when I get back to this feature.

Font work goes well. The current version needs final review, and then we should be able to merge it. It could be released fairly soon. Here's hoping!

And lastly, we have group spawn.

At first, I quickly tried retrofitting the current creature spawning tool for groups. But I felt it wasn't getting towards something good quickly enough. I had suspected this might happen, so switched to building the feature around the teleport tool instead.

The nice side effect of that is that we get group teleport.

![gif showing group teleport in action](/assets/images/groupTeleport0.gif)

I'll now take the layout work I did in the first experiment, combine it with the above, and add some UI so we can spawn an entire folder of unqiue-creatures in two clicks.

See you in the release notes!


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
