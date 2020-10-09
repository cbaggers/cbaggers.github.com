---
layout: post
title: TaleSpire Dev Log 234
description:
date: 2020-10-09 02:30:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

Once again, today was mostly spent on physics. Having solved the dice 'popcorning,' we talked about yesterday, I was left with an issue that dice would slide around as if they were on ice. It looked like this:

![ice dice](/assets/videos/iceDice.gif)

I had assumed this was material related and, sure enough, I found several places where I had not been setting the materials properly when generating colliders. However, even with those fixed, the problem still remained.

This led me into a whole bunch of tests and tweaks which weren't getting me very far. However, after some of the previously mentioned material fixes, I saw that things worked correctly on one of my test scenes. That scene was only using GameObjects with driven by BouncePhysics wrapped around Unity.Physics. The sliding must have been related to tile colliders, which are managed by the board and written into the physics engine separately. However, I had already set a material for all these colliders.

At some point, I finally noticed that the material I was using for the tiles (`Unity.Physics.Material.Default`) had a FrictionCombinePolicy than in the material I was using for other objects. For BouncePhysics, I was using a material converted from the default material found in Unity's classic physics engine. The result of that conversion used 'arithmetic mean' for friction combination, whereas the `Unity.Physics.Material.Default` used geometric mean. I switched the tiles to use the converted classic material, and the sliding stopped.

This is good, of course, but what a silly mistake. I had just assumed the two would play well together.

Anyhow for completeness, here are the dice. They won't behave like this when they ship. This is just to show the difference from the gif above.

![ice dice](/assets/videos/iceDiceFixed.gif)

With that done, I knew the next task was to rewrite line-of-sight, but that seemed like a pretty overwhelming way to end the day, and so I decided to play with something much more straightforward.

I've really wanted to experiment with adding a custom URL scheme for TaleSpire. This would mean opening a link that looked like `talespire://<something>` would open TaleSpire and do something useful. My first thought was that you could get a URL for a specific point in a particular board in your campaign. You could then share that with your party or even use it in external tools (World Anvil comes to mind), so you could link points in your documents directly to the TaleSpire maps that represent them. Also, if we support links out of TaleSpire, we could get a very basic but rather compelling way to move in both directions, which isn't tied to any specific tool or system.

To add a custom URL scheme to Windows, I needed to write some registry keys. Steam [seems to support this](https://partner.steamgames.com/doc/sdk/installscripts), but when I tested the registry options of the install script, I saw only partially made registry subkey trees. This was disappointing, but they also support executing other processes as part of setup. I assumed they would need to run as administrator, so I made a little c# script to add the required registry entries. This worked great. Here it is in action:

![custom url scheme](/assets/videos/customUrl.gif)

> Note: I have chopped out a bit of the middle of this video as loading takes much longer in a developer build, and it's boring to watch

I'm not going to do any more work on this for now, as there are many more important things to do, but this was a nice change of pace for a few hours.

That's all for now.

Peace.
