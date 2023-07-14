---
layout: post
title: TaleSpire Dev Log 402
description:
date: 2023-07-14 16:45:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

As the macOS build is rapidly approaching being shippable, I decided to spend the day looking back into how Steam handles multiplatform releases.

I set up the requisite depots and, after a bit of poking around, got the build script how I'd like it. That let me do a manual push, and so now we have:

![TaleSpire installing on mac](/assets/images/macSteamInstall.png)

Of course, this is not available for everyone yet. We still have a bunch of things to do before this can go public. Including but not limited to:

- Changing our keybinding handling so we can make sensible platform-specific defaults
- Tracking down an error that happens on exit
- Performing heavy testing and fixing any critical bugs that appear
- Fixing up Symbiote issues
- Adding in-game info to inform players about the limitations of the current Mac build
- Updating the info on Steam and prepping for release.

Due to not knowing what bugs we will find, we are not giving a release date yet. But if all goes well, it will be soon.

Big thanks to all the macOS folks who have been so patient with us. We hope it's worth the wait!


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
