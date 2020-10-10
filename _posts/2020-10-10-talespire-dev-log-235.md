---
layout: post
title: TaleSpire Dev Log 235
description:
date: 2020-10-10 02:50:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again,

Today I started out thinking I'd be working on line-of-sight, but instead, I dug into some issues we've had with builds since the Unity version upgrade.

First, one was that the dummy-asset resource wasn't loading reliably on game start. This is very odd, and it feels like it's a Unity bug. As we are currently using the 2020.2 beta, this is entirely possible. For now, I've worked around it by simply exposing a SerializableField for the prefab, which, to be honest, is probably a better way to do it anyway.

Next, I noticed that even though we had supposedly moved away from our old JSON asset format, I hadn't converted the music asset files. This work began on the TaleWeaver side, and it was straightforward to get the music info into the new binary asset format. As I was upgrading the TaleSpire side to use this new data, I was able to do some cleanup, which totally separated the old asset-data classes from TaleSpire and so now TaleWeaver can own those. This is nice as both sides can evolve their formats in response to their own needs.

I then went on the hunt for an error that occurred when deserializing hide volumes. The TLDR was that, due to having moved to C# 8, a different method overload was being selected for a specific call. It took me a little while to spot, but it was a very easy fix. The C# change that caused this is actually the one I've been most excited for over the last year or two, [unmanaged-constructed-types](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8#unmanaged-constructed-types).

The code cleanup was really enjoyable, and, given that it was nearly the end of the day, I decided to do some more cleanup that had been waiting for the engine to get to this point. In this case, it was on the manager that handles asset loading and the manager that handles tiles and props. They were a bit too bound together, and their respected roles weren't always clear enough. The changes themselves aren't worth enumerating, but I'm happy with the result.

Tomorrow I might look at line-of-sight, or I might look at laying the groundwork for the new board format and the code to handle upgrading the current boards.

Seeya!
