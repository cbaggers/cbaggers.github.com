---
layout: post
title: TaleSpire Dev Log 475
description:
date: 2025-05-27 19:52:50
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Howdy all,

Lot's in flight right now.

Chairmander has been working on the tags and part of that has been upgrading the creature format to support those. Yesterday I helped a little by updating the pack loading code in TaleSpire to work with the latest version of the format. I got to spend a little time helping get the vision limit feature closer to merging, and testing some backend fixes which will (when complete) allow some more features to progress.

However the main thing I've been spending my time on the last couple of days has been the particle system. Like many other particle systems the behaviour is scripted via a noodle graph[0]. We've had issues in the past with libraries for making these graphs so over a year ago, Ree made one for us. I've finally been able to get stuck in and it's been ace.

I've just finished making the serialization code and hooking it into Unity's save prompting system.

Next up I'll be working out how I want the type and error checking to work, and then I'll sketch out the basics of the compiler. From there I can hand all this over to Alex so he can add it to his work on the particle system.

If all goes well I should be back on the dice networking rewrite late next week. Early in the week I may be involved in a little hackaton, but I'm not sure what on yet.

All fun stuff, looking forward to telling you more.

Peace

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] Or node graph, whichever you prefer
