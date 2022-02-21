---
layout: post
title: TaleSpire Dev Log 324
description:
date: 2022-02-21 14:06:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks!

The HeroForge work is rapidly solidifying. We have had some testers playing with it and finding plenty of bugs, but nothing looks too scary. We have been able to try out some more normal creations…

![examples_0](/assets/images/hf_0.png)
![examples_1](/assets/images/hf_1.png)

…and some that are a little more out there.

![examples_2](/assets/images/hf_2.png)


In recent days I've been working on:

- Support for multiple creatures on a single base
- A mistake in mipmap generation
- A Linux (via Proton) specific bug due to how I was calling some WinAPI functions for random data
- Issues with miniature placement
- A line-of-sight bug which arose because of the changes needed to the creature code.
- Better use of head/hand/etc positions from model, and sensible fallback values;

#### Handling scale

The eagle-eyed among you may have noticed some large creatures with surprisingly small bases. Until now, the base size has matched the creature size (0.5x0.5 -> 4x4). After some discussions with HeroForge, we decided that the scale of the base relative to the creature should match the HeroForge editor as closely as possible.

One problem that immediately arose was that you can't just use the base size as the default scale. Otherwise, this chap

![big with small base](/assets/images/hf_6.png)

who is a 1x1 creature can be scaled up to 4x4, which gives you this

![far too big](/assets/images/hf_5.png)

Which is unplayable[0].

This meant decoupling base size and creature size. We use the total size of the mesh to pick a sensible default size and let the base be rescaled to keep the proportions required. This keeps things looking good while also being playable[1]

![decoupled](/assets/images/hf_3.png)


#### Today

So far today, I've hooked up the emissive map, which is where HeroForge bakes its lights[2].

![emissive](/assets/images/hf_4.png)
*Note: As you can see there is still work to do on the strength of the emissive*

It seems that the emissive map is included even if it is empty, so I am updating the asset conversion process to check for this. We can then exclude blank maps, saving some GPU memory in the process.

I am then back on bugs and loose ends. I have a lot of 'todos' in the code where I need to handle failure cases and decide how to relay them to the user. This will be fine, but it takes a little time. All of these cases will be relevant for the creature modding too.

I won't finish all the fixes today naturally, but it's all looking very positive. Getting a mini from HeroForge into TaleSpire takes about ten seconds, depending on download time (mine is pretty slow).

Until next time!


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*


[0] I'm trying not to be hyperbolic here. I tried playing with it, and it was utterly useless at those scales.

[1] Even today, scale can be tricky in TaleSpire. I definitely don't want to claim that our approach with HeroForge is ideal, but this feels in line with the current experience of the built-in assets.

[2] Except for the "dark magic" lights. We need to work out how/if we can support those.
