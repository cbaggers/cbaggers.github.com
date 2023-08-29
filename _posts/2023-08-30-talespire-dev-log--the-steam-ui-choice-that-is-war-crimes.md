---
layout: post
title: TaleSpire Dev Log 411 - The Steam UI choice that is war crimes
description:
date: 2023-08-29 23:24:43
category:
tags: ['Bouncyrock', 'TaleSpire']
---

> This post is about releasing for Mac

We were ready dammit. We had done all the stuff. The build was signed and tested. We had been open about goals and limitations, and we were counting down the minutes to release time.

We. Were. Ready.

Take a look at this:

![](steamUiExpectation.png)

Now, obviously, we all know from the title that something here is wrong. But take a second, clear your mind, and pretend that the world isn't a vortex of discount madness. What should this UI do?

To give you a moment and to give some distance before the answer, here are two distractions.

![](/assets/videos/slabInLibrary.gif)

Hey, look! Slabs in the library. We've been meaning to do this for a while. Content-packs in TaleSpire will be able to contain slabs. That will let us ship bigger building blocks for speedy building.

![](/assets/videos/roomDragTest.gif)

Distraction the 2nd. This is a test I've been wanting to do for a bit. It doesn't feel as good as it should, as it feels like it should have smarts to handle intersecting rooms. However, it was a neat test, and it might find its way into the Symbiote API.

ALRIGHT. So you've decided what a sane system would do. Now let's see what we saw 20 minutes before we shipping the Mac release:

![](/assets/videos/steamUiWat.gif)

Yeah... YEAH.

Twenty minutes before release, we find out that we can't choose not to ship on Intel Macs, even though none of the Steamworks documentation I've found says that is a restriction, and the UI definitely doesn't suggest it.

As you folks know, we chose not to ship on Intel Macs as we don't have the resources, as a team, to make the game run well on those machines and support any idiosyncrasies that come up.

Suddenly, we are missing our deadline because of... assuming Steamworks checkboxes work as checkboxes.

If only someone had invented some kind of UI gizmo for this very situation. Maybe even inspired by those old buttons on radios.

![](assets/images/SteamUiSane.png)

Ah, a man can dream.

Back in reality, we were in trouble. We either had to cancel or try and get a valid build ready for intel that day. Naturally, we had to at least attempt the second option.

Telling Unity to build for Intel Macs is simple enough, but when I tried to run it on my 2015 Macbook, it quickly reminded me that we have native library dependencies, too.

Some of those libraries are our own, so we rebuilt those as universal binaries for those. An easy fix. But for some, we didn't have the binaries required.

For one, we were able to find the Intel version on Nuget, and I tried to tell Unity which one was for what architecture. Sadly, I couldn't get that to work. I'm 100% sure it was my fault, but we were short on time, and I was stressed. Almost as a joke, I googled how to combine two dylibs into a universal dylib, and bloody hell, [there is a way](https://ss64.com/osx/lipo.html). [Lipo](https://ss64.com/osx/lipo.html) is a program that lets you do exactly that!

By simply running `lipo libminiz-intel.dylib libminiz-as.dylib -output libminiz-universal.dylib -create`, we suddenly had what we needed. I slapped that into the build and got an intel-compatible build.

Now, we still cannot support these platforms, so we whipped up some UI to inform the user of that fact. We saw Unity's [SystemInfo.processorType](https://docs.unity3d.com/ScriptReference/SystemInfo-processorType.html) property, and the docs even showed how to detect ARM. Perfect. We slapped that in, but it didn't work as, even though Apple's M processor range is ARM architecture, `processorType` doesn't return `"ARM"` for them. On any typical day, this would be a momentary annoyance, but when the day has already gone so sideways, it's just an extra kick in the knackers.

Compounding the stress is that a complete clean release build and signing takes a couple of hours, so little issues can really compound.

In the end, as you know, we shipped on Mac a few minutes before midnight in Norway. So we had met our public deadline, but a stiff drink was in order after all that nonsense.

To end, I want to say this to the ether:

> Steam peeps, please invest in radio buttons. I no longer know how to trust, but others could be spared.

And to the rest of you, have a good one folks :P

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
