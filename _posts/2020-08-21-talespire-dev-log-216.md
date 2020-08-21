---
layout: post
title: TaleSpire Dev Log 216
description:
date: 2020-08-21 17:10:11
category:
tags: ['Bouncyrock', 'TaleSpire']
---

![chest](/assets/videos/chest.gif)

This looks like nothing. If you used TaleSpire, you've seen this a thousand times. For me however, this is great. The fact it looks the same means the following:

- Spaghet's realtime scripts are being controlled correctly by the state-machine scripts
- The custom animation system is working
- The new batching system is working
- All the work from the last week to replace the old physics engine has paid off. If you can pick a tile (which we are), then the raycasts are working.

This has been a slog. Any time you have multiple days without being able to start the game, it's mentally draining, and these last weeks have been tough. There are piles of issues still[0], but I think I *might* be through the worst part.

Have a great weekend everyone

[0] To name just one, mesh colliders aren't properly oriented


p.s. I'll be offline for the next few days as I'll be off recharging in the country somewhere :)
