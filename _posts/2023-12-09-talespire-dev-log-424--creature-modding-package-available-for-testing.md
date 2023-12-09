---
layout: post
title: TaleSpire Dev Log 424 - TaleWeaverLite: The good, the meh, and the future
description:
date: 2023-12-07 00:15:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

In preparation for creature modding feature shipping, we are making the tool available so you can try it out, and we can fix any egregious bugs you find.

You can find a guide on how to get the tool and how to use it over here:

[https://talespire.com/taleweaverlite-guide](https://talespire.com/taleweaverlite-guide)

> NOTE: You cannot load the mods into TaleSpire today. This is simply a build of the tooling used to make the mods.

## What is this (and what is it not)

TaleSpire is going to have a couple of ways for you to add new creatures to the game:

- In-game creature creator: A way of building creatures out of a palette of configurable parts
- Mods: mod-files are composed of pre-made assets outside of TaleSpire

TaleWeaverLite is for the second approach. It is a very minimal tool for composing pre-made assets into a creature mod that is ready to be used by TaleSpire.

It is not a modeling or posing tool. It expects content in specific formats and with specific layouts.

The expected usage is that the mesh and textures are prepared in other tools, such as Blender, and using TaleWeaverLite is the final step in making a mod.

## The good

Once you are producing meshes and textures in the correct format, it's trivial to turn them into a mod. Simply drag them into the Unity project, set the fields in the creature panel, and save.

## The meh

We've been using Unity for our asset tools for a long time. It handled processing assets for us and gave us a UI we could use to make tools.

This means that to use TaleWeaverLite, you need to install the free version of Unity.

We have mixed feelings about this, as packaging this stuff for public use has not been as easy as we had hoped. Your feedback will help us decide whether to stick with this in the long term or to start moving to a more standalone tool.

## The future

We are busy working on the backend changes needed to support the new creatures. Once we have that done, we'll wrap up the needed changes in TaleSpire and get ready to ship. We are pushing hard for this and hope we can get this in your hands very soon.

By getting TaleWeaverLite out now, there might even be some new creature mods ready to ship the moment that TaleSpire gets the feature!

Until then, thanks for stopping by.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
