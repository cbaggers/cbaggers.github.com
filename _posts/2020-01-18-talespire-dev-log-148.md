---
layout: post
title: TaleSpire Dev Log 148
description:
date: 2020-01-18 13:05:45
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I've been having a great time coding down at @Ree's place, but it has meant I've neglected these updates. Let's fix that.

We met up so we could collaborate on merging our feature branches together. Up until now, I've been focused on performance, and the data layer and @Ree focused on tooling. The goal isn't to complete the merge, but instead to do any tasks that are easier when we are in the same room.

I've successfully dumped all of my code into the project, got it all building, test passing, etc. The managers for various systems are created now, but the building tools are still using the old code for now.

One massive source of complexity in the old version stemmed from our use of Photon, the 3rd party networking layer (which is great btw). By default, when a person leaves the game Photon automatically clears up all the networked objects they created[0]. This is no good for creatures (which were networked objects) as the other players still need to see them, so we disabled auto-delete of networked objects. This naturally meant we had to handle cleanup, which includes things like dice.

With the new system, we won't ever have the whole board loaded at once, and this means some creatures won't be loaded either. This also means we don't want Photon to be helpful and make sure all networked objects are spawned as soon as someone joins the board. To deal with this, we removed the Photon network component from the creatures and gave a networked 'handle' to each player. This is attached to a creature when you select it and syncronizes the transform while the creature is held. On the other end, if the creature is loaded, it seems to behave exactly the same sa it did previously. If not, then no worries, once the creature is spawned, it will have the latest position will be sent anyway. 

After a misguided prototype, I teamed up with @Ree to work out the handle api, and then everything came together very quickly.

This change actually lets us turn back on auto-delete in Photon and delete a bunch of cleanup code. Removing code is the best.

> NOTE: The handle approach may seem super obvious, but previously there had been reasons that it wasn't a good fit. However, those reasons are out of date now thankfully :)

On the subject of creatures, @Ree has been working on a new creature controller, which is much better suited for moving around these increasingly vertical scenes. It's based on the excellent [Kinematic Character Controller](https://assetstore.unity.com/packages/tools/physics/kinematic-character-controller-99131) asset for Unity. It's an impressively stable base to build upon [1]. Our creature control still looks the same (we haven't switched to slidy tank controls or anything :p), but this lets us handle moving through scenes more reliably than before. You still have the ability to lift the piece and throw it over walls when needed.

With the simplification of the networking code, I looked into upgrading to the latest version of Photon. I got partway through the migration before I hit an undocumented change. The [StreamBuffer](https://doc.photonengine.com/en-us/pun/current/reference/serialization-in-photon#streambuffer_method) class used to be a subclass of `System.IO.Stream` and in the latest version, it isn't. We have a bunch of code that relies on it being a stream, and so all that will need to be rewritten. This was already underway, but we need to keep the codebase working while all that gets finished and hooked up. For now, that means that I have to give up and do it later. Lost a few hours to this but ah well.

Another thing I have been doing during this merge is ripping out code. Old fog-of-war, line-of-sight, board format upgrading, all that and more got ripped out. There is no point fighting things if they aren't required to keep tooling development progressing. It also is making it easier to work with or refactor code that used to have to interact with those removed systems.

There has been plenty more than this going on. @Ree is changing how the board grid works; I've been working on build speed and assemblies... and so on and so on. 

In all this is going very well. We are plowing through tricky stuff and are ready to get back to working separately again, albeit this time with much more frequent merging as we approach Beta. 

I'm taking Sunday as a rest day, but I'll be back on Monday with more dev news. 

Ciao.


[0] strictly, it's the ones they own, which can be different if ownership has been transferred.
[1] also *amazingly* cheap for what it is.
