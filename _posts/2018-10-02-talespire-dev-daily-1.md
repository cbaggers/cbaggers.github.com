---
layout: post
title: TaleSpire Dev Daily 1
description:
date: 2018-10-02 17:02:29
category:
tags: ['TaleSpire', 'BouncyRock']
---

Alright, another day down.

Today was primarily spent adding the undo/redo system which is looking pretty good now. A few bugs to iron out but nothing that looks terrifying.

As you'd probably imagine it's a simple list of history actions and an index of where in the history you currently are. Each event has `Undo` and `Redo` methods and some serialized state for the thing that was being added/removed/etc. This a dirt simple approach and can get pretty unwieldy for systems with many more kinds of actions but for our little thing it's just fine.

During making this I noticed that the board synchronization code was being too conservative about when it could send the board so now it gets sent much sooner after loading from disk. The basic run down goes something like this:

Tiles load their assets (visuals and scripts) asynchronously so it can be a relatively long time (a few frames) after the tile is created before the Lua scripts inside are running. Because of this, when deserializing we cache the state intended for this asset until the Lua scripts are fully set up.

When loading levels we obviously want to send the full state to the other players so we waited for all the assets to be full loaded before sending the level to other players. This was pointless as a give tiles can only be in 3 states regarding initialization.

- it's fully set up and running
- it's been initialized, has some cached state, and is still loading
- it's been initialized, has no cached state, and is still loading (like when you first place it)

In all of these cases we can serialize as, even if it hasn't finished loading, we have the pending state or we know it's going to have default state.

We also could just sync the level data from the local file and append the additional setup data (character network ids etc), and we may go that way, however for now the runtime cost of load-then-sync is so low currently that it's not worth it.

Tomorrow I will be refactoring dragging out tiles. Simple stuff but it needs some attention so it plays nicer with sync and undo.

Seeya tomorrow!

<iframe width="560" height="315" src="https://www.youtube.com/embed/S7jK9wxwq9A" frameborder="0"></iframe>

-----------------------------

> Boring Caveats
>
> These are just my daily notes as I work on TaleSpire. It's so cool that people are interested in the game and it's way more fun to share this stuff that just keep it away, however nothing said here should be taken as any kind of promise that a thing will exist in a given release or work in the way stated. Stuff changes constantly and I'm wrong about at least 1000 things a day so whatever I'm stoked about today may well be tomorrows nightmare.
>
> So yeah, that's that. Back to the code :)
