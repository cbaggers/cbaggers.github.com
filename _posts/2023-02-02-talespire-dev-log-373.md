---
layout: post
title: TaleSpire Dev Log 373
description:
date: 2023-02-02 20:41:36
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks! 

Last week was spent on the backend. A lot is going on, but I want to save that update for when the governmental paperwork for our potential new hire is complete. It's taking ages. 

Work has continued on Symbiote, our modding feature allowing you to dock webview-based mods on the right-hand side of TaleSpire. Luis and I have been testing the web-view library under Linux (via Proton) to ensure we can provide the same experience across Windows, Linux, and Mac. So far, we've got it working if we disable sandboxing, which isn't ideal but might be how we ship the beta. 

Continuing with Linux, we have progressed in getting talespire:// URLs to work out of the box. Until now, we've been relying on a great script from community members, but in some cases, it requires user input [0]. This was because some execution flags[1] involved are system dependent.

Luckily for us, after a bunch of digging, Luis realized they were in the environment variables! So now we are pretty sure we have what we need to make this work. Hopefully, more on this soon.

From Linux to Mac we also have some good news. Unity has replied to Ree, saying that the bug he submitted (and that is blocking us from shipping mac support) has been fixed and is rumbling through whatever internal process is needed before it is released. When the fix eventually ships, we will have to upgrade our Unity version, which hopefully won't be too painful [2].

I'm mostly done with a feature that allows you to only show the tiles/props you are currently selecting. This can be handy for precise selections, especially when working out what to upload to a slab repository. You can see it in action here:

![a mode which isolates what you see to the active selection](/assets/videos/isolationMode0.gif)

You may also have spotted this icon:

![the publish slab button](/assets/images/publishSlabButton.png)

This is the "publish slab" button. Clicking it will take you to a mode where you can frame the screenshot for your slab and provide the required information. That mode will also be used for editing the info of the slabs you've already uploaded. It's looking rough right now, but we have plenty of time to improve it. For now, I want to focus on shipping this stuff as soon as possible.

![publish mode](/assets/images/slabPublishWip.png)

I'm running a bit low on caffeine, so other things will have to wait for another dev log. 

Have a good one folks.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] Also because it is run by Proton, so needs to be a Windows program.
[1] ESync or FSync
[2] There is a good chance that it will also fix the long-standing bug in the input system that stops us binding `e` to actions. That one is so weird!
