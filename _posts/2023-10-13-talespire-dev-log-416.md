---
layout: post
title: TaleSpire Dev Log 416
description:
date: 2023-10-11 02:35:24
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

It felt good to get the slab-browser beta out, and while I do have things to fix before it is ready for prime time, I couldn't help but take a swing at a different feature. One we have all wanted for a while. Board folders.

![janky unfinished folder with a board in it](/assets/videos/boardFolder0.gif)

Today was mostly spent writing server code for this feature. I tested some of it but wanted to get some UI in before trying the rest. To that end, I've been hacking the basics together.

For the first version, changing a board's folder will be done via a dropdown. Really, I'd prefer to make it possible to just drag boards into folders, but I'm not familiar with how to do that yet, and I don't want to spend too much time on this as I NEED to get moving with the creature modding support. So, the first version will be functional, if a bit basic. I'm also not adding arbitrary nesting of folders for now.

Jumping back to the creature modding, I read more about what kinds of access Unity gives us to mesh data. I'm satisfied that I can make something to cover all the bases very quickly. It's also tempting to look into Zeux's excellent [mesh optimizer](https://github.com/zeux/meshoptimizer).

That's enough for today. 

See you in the next one.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
