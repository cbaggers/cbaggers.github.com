---
layout: post
title: TaleSpire Dev Log 341
description:
date: 2022-06-03 00:54:39
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

I've now packed up my computer, ready for the move, but I did get some stuff done before then.

I've made the first pass at including groups in the HeroForge browser.

![wip](/assets/videos/hf-folder-wip.gif)

I've merged the polymorph UI branch into my dev branch and am currently working on bugs. Currently, I'm updating the code to push scale along with the morph data so that players get the correct result even if the asset hasn't finished loading yet.

The scale work required back-end updates to make unique-creatures handle this correctly. That is done and is looking good so far in testing.

I think that's the interesting stuff for now. I should be set up for work by Monday, so I'll see you then.

Ciao

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

