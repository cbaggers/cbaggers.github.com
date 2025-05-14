---
layout: post
title: TaleSpire Dev Log 456
description:
date: 2024-09-12 08:49:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Allo folks,

Yesterday, I exported the Norwegian translations into the new format. This went well. I'm now looking at the developer's experience of using the system.

I hope to obviate the need for a central registry of text entries that the developer has to manage[0]. TaleSpire needs to be able to export a translation template for a given language, and that file will have to include all the things that need translation. When we are in the Unity Editor we can iterate through all the objects in the game looking for translatable elements, but that isn't something we can do once it's shipped. That is a place where a registry helps, so I will try building this structure when we build the game.

The only fiddly part is that (unlike Godot) Unity has objects in both scenes and prefabs and working with scenes is more awkward than working with prefabs. Hopefully, it doesn't prove to be too much of a pain.

Community creations will build their own translation tables when saved from TaleWeaverLite. That will be much easier as we control the whole format and process there.

More news coming soon.

Ciao.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] This desire came from our experience using the previous system.
