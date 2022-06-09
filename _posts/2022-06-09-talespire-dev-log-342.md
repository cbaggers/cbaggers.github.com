---
layout: post
title: TaleSpire Dev Log 342
description:
date: 2022-06-09 11:00:11
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Last night I pushed a server update containing code needed for polymorph, HeroForge pack grouping, and tracking photon errors. Aside from one hiccup, it all went well.

The last few days have been a mix. My Sunday and Monday were mainly working on moving house stuff. I've got a little cabin thing I'm using as an office, so I dug a trench and laid conduit to get internet out there. That is working great, so I'll need to work out my streaming setup again so we can do some catchups.

I've just merged a pile of polymorph work into master. It's slowly getting more stable, although I still have some pressing bugs to fix before we can push it out.

I've got to head out now to get a vaccine for tick-borne encephalitis, as the area I'm in is pretty forest'y. When I get back, I'll be working on HeroForge. I need to focus on the biggest pain points there so we can get it out of beta status. I'd like to get file hash validation done, and then I'll see how far I can get on texture rescaling today[0].

The next week or so will still be a bit hectic for me (due to life stuff), but everything is going in a great direction.

See you around.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] Currently, our assets file sizes are way bigger than they need to be due to working around a Unity bug where their texture quality setting breaks DXT textures. I'm hoping I can rescale the textures so that sizes at each mip level will be a multiple of four (required for DXT) regardless of the texture scaling their quality setting uses.
