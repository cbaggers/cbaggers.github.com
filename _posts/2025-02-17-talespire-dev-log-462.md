---
layout: post
title: TaleSpire Dev Log 462
description:
date: 2025-02-17 09:12:03
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Howdy folks. My posting is erratic, but I am still working away. Recently, my focus has been on localization and more 3rd-party mini support.

### Localization

I've been on a translation kick again in the last few days. I've been fixing bugs and going through the main menu and settings panel, slowly making all the text elements translatable. This is tedious work, but it's got to be done. 

One tip I picked up from a GDC talk (I can't recall which one[0]) was to make a "translation," which is just the English text backwards. This helps as, even if you aren't skilled in other languages, you can see what UI elements have not been translated at a glance. It's easy enough to read the text, too, so you can keep testing the game.

My next task here is to make dropdown menus localizable. Let's hope that is as easy as it sounds.

Once we have something I'm happier with, I'll enable the translation tools in the settings panel, at which point some of the adventurous of you can kick the proverbial tires.

### Additional third-party mini support

You have all made the community creation support a huge success. While we at Bouncyrock still have a lot of work to do (for example, tagging, improved search, and tile/prop support), we are delighted and amazed with how much you have all built. Naturally, not everyone is a 3d artist, and there are folks online who have created great options for making and exporting miniatures. Eldritch Foundry and TitanCraft are two who have reached out to see if we could make the process of bringing creations to TaleSpire as smooth as it is for the community and HeroForge.

As usual, we are not taking any money or kickbacks from EF or TC for doing this, and we won't be taking a cut of their sales [1]. We just want to make sure the community is strong, and part of that means giving you all options. 

We still have our creature creator on our roadmap, but we are a tiny team, so it takes us a long time to get big stuff done. Luckily, we aren't going anywhere, so we'll keep building, and the rest will work itself out.

I've been using this feature as an opportunity to do the work to merge the 3rd party providers into the community creations panel. Currently, HeroForge is in a separate tab, but the community browser has always been meant to support many providers. Having HF separate has been grand, but it will get a little unwieldy once we have three mini-providers. This work is also helping with the future community-run-repository support as it's forcing me to fix a lot of places where I had hardcoded things for Mod.io support[3]. 

### More stuff

Here is a grab-bag of other stuff that I've been involved in.

We had a report of a weird performance bug where player mode was running slower than GM mode. That kind of bug is catnip to me, so I dove into fixing that. When that was done, I used Unity's [PlayerLoop](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/LowLevel.PlayerLoopSystem.html) API to add a new callback later in the frame that they usually let us run code. The goal here was to run some tasks while waiting for rendering to complete. This worked, but it didnt give the performance boost I was hoping for[2]. However, it was fun for an hour's work and something I will definitely be revisiting when I return to performance work. I'd love for TaleSpire to get faster as we add more features, rather than how it too often feels in much of software we all use day to day. 

Chairmander has been working on tagging, so I've been in meetings with him as we hammer out details.

Among the many things Borodust has been working on, he made a nicer view of our internal error tracker, and with it, I found a load of bugs in TaleSpire that would be difficult for users to report due to how they manifest. I fixed about 8 bugs that afternoon. That was great fun.

The creature-vision-limit feature has been parked for a few weeks as it needs some play testing, and we've all been busy. The play sessions are to determine what else we want to do with the UI for the feature. The sooner we get that done, the better. I'm dying to get that in your hands. Today, I'm probably going to be wiring up some excellent work by Ree, which makes the range limit hide the floor plain in addition to hiding tiles/props/creatures.

Well, that's what's popping into my mind right now. I'm sure I'm forgetting a slew of other things, but that's what dev log 463 is for!

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] It might have been [this one](https://www.youtube.com/watch?v=t6Wwe1_NnOc)
[1] The same is true with HeroForge
[2] Unity's frame interleaving was already doing a semi-decent job and so we didn't get a benefit from just pushing things around.
[3] This happened as I was rushing to try and get the package managed shipped before the heat death of the universe :D
