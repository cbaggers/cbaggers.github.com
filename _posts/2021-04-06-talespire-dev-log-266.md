---
layout: post
title: TaleSpire Dev Log 266
description:
date: 2021-04-06 15:12:59
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks.

This dev-log is relatively niche but will be helpful to the few who are into it.

In the beta, all tiles had an associated `.boardAsset` JSON file which held information about that tile. Since then, we have replaced the multiple JSON files with one index file with a binary encoding. Although better for the game in every measurable way, the binary format makes it hard for the community-driven sites to get info on the tiles, props, and creatures in the game. To that end, we have added an `index.json` file which holds a useful subset of the binary file.

You can find this at `<path to the steam files for talespire>/Taleweaver/index.json`

You can find an overview of the layout here https://github.com/Bouncyrock/TaleWeaver-Community-AssetPack-Index-Format/blob/master/format.cs

It is written in a C#-like pseudo-code but should be enough to get curious folks started.

We also now pack the asset icons into atlases, so the JSON file includes the per asset information of where in the atlas the icon resides.

Have a great day!
