---
layout: post
title: TaleSpire Dev Log 185
description:
date: 2020-05-27 23:41:49
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Today has been spent testing and fixing the backend changes for creature scaling and hooking the creature scale feature into the data model.

The scaling is feeling great, Ree's done great work, and I'm very excited to see this ship. There are still bugs to be fixed, so we'll keep working on those. 

The server side has now been patched, and so everything is ready to go.

The first thing I'll be doing tomorrow is adding some code to detect mismatched versions of the client. This will stop a person with an older version of the client connecting to a session hosted by someone with a newer version (and vice versa). This is important as the board sync format can change and are not compatible between different versions of the client [0].

That's the lot for today

Ciao


[0] Please note that this format is not the same as the board persistence format for which there are format upgrade methods written.
