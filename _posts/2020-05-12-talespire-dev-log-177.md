---
layout: post
title: TaleSpire Dev Log 177
description:
date: 2020-05-12 02:23:10
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I've been a little quiet today, but I've been around. I've mainly been testing and re-checking the code that handles board format upgrades. I didn't push out an update today as, during testing, I saw some cases where GM-blocks showed up incorrectly. I'm pretty sure these are pre-existing bugs, but it's better to delay the release and fix them, rather than take the risk.

Tomorrow I'll carry on testing the board format upgrading and then put out the release. Initially, the release was to add fog control to atmospheres but it now also includes:

- A fix which means that when you return to boards, it (finally) remembers where your camera was and where it was facing
- A fix for one instance of an error where, on rejoining a board that failed to load, you got a 'failed to correctly assign internal id' error.

And of course the above gm-block fixes.

After this, I'm aiming to focus on updating the asset format used by TaleSpire. I've been talking about doing this for about a year, but other things were always more important. The big goals are to improve load times and to make the asset data accessible to the job system. This will be huge for me as there is far too much complexity and inefficiency in the asset code due to constantly having to repack data in structures amenable to the job system.

Of course, some community projects have started consuming some of this data and, even though use of that data is not supported, I'd rather not break those. What I think we'll do is make a portion of the data available as a separate json file. TaleSpire itself won't use that file, but it'll be handy for others. I'll keep you posted on these changes.

Have a great evening folks,

Peace.
