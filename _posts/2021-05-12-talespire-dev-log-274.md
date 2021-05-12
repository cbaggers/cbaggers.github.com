---
layout: post
title: TaleSpire Dev Log 274
description:
date: 2021-05-12 13:49:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks!

Progress is slow but steady for me this week. 

On Monday, I started adding support for multiple asset-packs to TaleSpire. This is required for future modding support. We knew this was coming, of course, and so a lot of the work had already been done. The main task was deciding how the asset-pack id would be stored in TaleWeaver, writing it into the correct places in the index, and updating TaleSpire to search for packs in a given directory and load them. 

The TaleSpire part will still be improved as we don't want to load all packs the game can find immediately. Each campaign might be using different packs, and loading unnecessary ones waste local and GPU memory.

Other than some board restores, my focus for this week is on the layout code in TaleSpire. The runtime data has a known issue that can make it very fragile to mistakes in asset-packs. The wrong change there can currently corrupt boards. It's been easy enough to tiptoe around it for now as it's only us providing assets, but this is unacceptable for modding, so it needs to be fixed. [0]

Naturally, this touches a whole mountain of code, so it's going to keep me busy for a bit, but it's gonna be a huge relief to get it done. 

That's all for me for today. 

Seeya around folks :)

[0] The changes should not have any visible impact to the game or to the board format. It's only changing how we juggle the data behind the scenes.
