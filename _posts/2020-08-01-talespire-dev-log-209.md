---
layout: post
title: TaleSpire Dev Log 209
description:
date: 2020-08-01 00:21:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Phew, what a week. I've almost lost track of all the things that have been going on. Amongst it all, more code is getting written.

Ree has been able to get through enough other tasks that he's got some time to start experiments with props behaviors. Props are one of the most significant missing pieces from the game right now, and whatever behavior we settle on, we are going to have to live with for a while, so it's good to take our time with this and find something we like.

I've been back in TaleWeaver, looking at how board-assets work. Yesterday I wrote a tiny compiler for the state-machine scripts and added code for showing errors in the state-machine script graph.

Today I've written code to export the tile information in a new binary format. TaleWeaver now also makes atlases for the tile icons. This should allow TaleSpire to batch more draw calls in the UI, hopefully speeding that up a little bit.

The next stop for me is to take this new format, get it loading into TaleSpire, and rewrite the code that manages looking up tile information. I've also had great progress in the design of system that will run the realtime scripts, so everything is lining up to work well. Just got to keep hammering at it!

Hope this found you well.

Seeya
