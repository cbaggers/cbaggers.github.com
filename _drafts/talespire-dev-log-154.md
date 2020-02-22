---
layout: post
title: TaleSpire Dev log 154
description:
date: 2020-02-22 12:40:24
category:
tags: ['TaleSpire', 'Bouncyrock']
---

Heya folks, 

As planned I started off Wednesday working on a bug which meant that scripts were not getting unique ids. This manifested as you clicking to open one door but a chest in another room would open. Cute, but obviously broken :p

As I chased this down I became more and more annoyed with how centralized all the state for these state-machine was. Originally I had startd implementing the real-time scripts (ones that recompute their state every frame and arent syncronised over the network) and there I wan't the data to be packed together so I could easily iterate over large numbers of them. We didnt need the real-time scripts to be ready for the beta launch so I had switched my focus to the state-machine scripts (used for doors, chests, etc). I reused a lot of code from the real-time scripts and pare of what we inherritted was how their private data is stored. By storing this all together syncing a zone also meant syncing this store, which has worked but doesnt feel great. One result of this is that you end up having to download all the state for all the scripts in the whole board just to be able to have the scripts one zone work properly.

As the delay has given us a little extra time this became a great candidate for a refactor as it makes things better for the beta launch and also makes per-zone-sync (which is coming after beta launch) easier to implement. 

If this has raised your eyebrows that is fine. This is a slippery notion that a few of us were discussing on the discord the other day. It's actually possible for a delay to make a second delay more likely as you now don't *have to* ignore fundamental issues and just crank out something that works. You can suddenly address those issues, but in doing so you will innevitably run into new edge cases and bugs and those are now in your pile of things to do before release.

In this case I'm hoping it's worth it.

With the state for the state-machine scripts moved to the zones holding the tiles in question I turned to creatures. We have two kinds of creatures in TaleSpire, Normal and unique. 

When a creature is made unique it can be moved between boards in a campaign and it has a little extra data associated with it. Traditionally uniques were the only creatures stored in the database, non-unqiues were saved into the board.

The issue with non-uniques being saved in the board is a question of how much needs to be syncd. If their data is stored on the zone they are in then simple rotating your character means you have to sync that whole zone (which may contain a thousand other tiles). If you decide to store creatures together outside of zones then loading a single zone requires pulling all the data for all the creatures in the board, and there could be thousands of creatures in a whole board.

For now, we have instead moved non-unqiues to the database too. This lets us sync a single creature at a time and gives us the opportunity to pull filter what we pull on where they are in the world. 

Being in the db also gives us opportunities for tooling that lets GMs query info about creature across the whole board without having to pull it all first.

Lastly I finally spent the time to tweak the server and client so I can host the whole backend on my laptop and connect to it from Unity. This is awesome as I can iterate on both parts at the same time without having to push dev servers to aws and wait on that. This cost me about a day but, in my opinion, it's been totally worth it.

Right now I'm going to chill out for the weekend. Next week I'm planning to dedicate all my time to fixing the bug in board sync that delayed the beta.

Seeya all on Monday!
