---
layout: post
title: TaleSpire Dev Log 252
description:
date: 2021-01-06 01:05:49
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks! Yesterday I got back from working over at Ree's place, which was, as always, super productive.

Between us, we have:
- props merged in
- copy/paste working correctly with combinations of props/tiles together
- Fix parts of the UI to configure keybindings
- More work on the internationalization integration
- Progress on the tool hint UI and video tutorial system
- Updated all shaders to play nice with the new batching system
- Fixed some bugs in the math behind animations and layout of static colliders
- Rewritten the cut-volume rendering
- Fixed a bug that was stopping us spin up staging servers
- Generated low-poly meshes for use in shadows, line-of-sight, and fog-of-war

That last one is especially fun. Ree did a lot of excellent work in TaleWeaver hooking up Unity's LOD mesh generator (IIRC, it was AutoLod). The goal is to allow creators to generate or provide the low poly mesh used for occlusion. This allows TaleSpire to reduce the number of polygons processed when updating line-of-sight, fog-of-war, or when rendering shadows.

Most of the above still need work, but they are in a good place. For example, the cut shader has a visual glitch where the cut region's position lags behind the tile position during camera movement. However, that shouldn't be too hard to wrangle into working.

Today I moved the rest of my dev setup to the new house. Lady luck decided it was going to well and put a crack in my windscreen, so I'm gonna have to get that replaced asap.

The next bug for me to look at is probably one resulting in lights being positioned incorrectly.

Anyhoo that's the lot for tonight.

Seeya!
