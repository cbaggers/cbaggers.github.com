---
layout: post
title: TaleSpire Dev Log 149
description:
date: 2020-02-08 12:52:03
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Well, I've learned something about myself in this whole process. The closer I get to deadlines, the harder it is to convince myself to write updates when 'the answers' aren't yet available. I've been struggling with a bunch of bugs and issues in the last couple of weeks and each time I see that I really should be writing an update I see the unsolved problems and think "If I can just get a bit further I'll have something more concrete to talk about". I guess dev logs getting sporadic around releases is just a thing that's gonna happen for now. We shall see.

That said, hello again! A lot has been happening behind the scenes recently.

One big push on my side has been moving using ids rather than actual object references for creatures/players/etc across the whole project. So often in the future, the game will be receiving info on things that aren't loaded on your local client, and it needs to be able to handle that.  Luckily this only effects: building, sync, creatures, initiative mode, movement, gm request, and.. yeah well, everything. So, in short, I've been rewriting a significant portion of the codebase. It's gone well, but it's a lot of stuff to have in the air at once.

I've also finished implementing the sync of boards both to the server and between players. For now, it's the same method as we used in the alpha. We are saving the whole board to a single file we load all at once. Naturally this wont work for big boards but I need to do some server work before I can add per-zone sync. We could ship the beta with this and then upgrade as we go. This may happen. The most important thing, for now, is that we get something that will get us out the door and is in the right shape to be moved to the new approach.

In fact, so much of the work feels different this time round in that we know what we need to make, but it's a lot more cognitive weight :D. In the alpha, you make something, and then all the implications come pouring out; this time, you have to build and build with the main comfort being that, at least, you know it's all needed.

Automated testing has been a lifesaver too. I recently added an option to the tests so that halfway through the test, it will serialize the board, deserialize it as a new board and hook it up like a networked client. It then continues the rest of the random actions and compares the two boards afterward. With these random tests, I've been able to find piles of dumb mistakes.

Other stuff that has been worked on in the last few weeks:

- Rewrote the undo/redo stack and the code that moved asset data between the active and inactive set. It had gotten far too complicated and was impeding progress. The new system is coarser but simple and fast.

- Ree has been working on a control to allow you to move creatures between floors and across otherwise impassable obstacles easily.

- Wrote new managers for tracking board, campaign, and network state. Some of the work on campaign state is getting it ready for the coming backend changes, and a pile of it is making sure that our connection to the realtime network stack isn't so directly tied to whether we are considered in the board or not.

- (Finally) hooked up single tile delete!

- We want to support up to 16 clients building. Whilst writing the code that hands out those slots, I realized that we have a lot of what we need for spectator mode. We'll have to do a bunch of work on the UX side for this but it's more feasible now.

- The atmosphere system is under heavy modification by Ree. He's looking into how music is handled now. While we've been warned the first version will be pretty bare-bones, I'm personally excited for where this will go.

- We have now been able to jobify lots of tasks that only update shader state. This includes tile drop-in a highlighting. This can be much faster now.

- Moved the creature state to the new board representation. Attacks, stats, death, etc all go via the new system now.

- Rewrote cutscenes sync

- Disassembling class hierarchies for assets: Don't do OOP kids :p More seriously, I had tangled the implementation of creature & tile board assets in a way that made progress harder. I have undone that now and, while there is seemingly more duplicate code, it's way easier to manage and iterate on (and most of that code has subtleties that mean it shouldn't be merged anyway)

- Removed Lua

- Rewrote how Spaghet scripts index their private state

- Water prototypes! (see the bottom of this page)

And waaay more. Lots of bugs are being worked on in the mix too.


It's been a very productive time, but I'm still a good week behind where I wanted to be at this point. For context, the first 'proper' board load with the new system was 6 days ago, and the first networked play (with the new system) was yesterday. To be fair, it's, of course, the case that for those to work tons of other stuff has to be working, so this isn't *too* unreasonable. However it's still a little tight. The best thing to do it work hard and keep you all in the loop.

Under the next one of these,
Seeya


p.s. Here's a quick extra that we are showing off on the Kickstarter this week! More news on this as it happens
