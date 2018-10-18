---
layout: post
title: TaleSpire Dev Daily 15
description:
date: 2018-10-18 23:35:57
category:
tags: ['BouncyRock', 'TaleSpire']
---

Good-evening folks! Today was spent getting used to the profile, finding out 'deep-profile' is useless, finding out that some parts are pretty fragile and then making a few speedups.

Mainly it has just be reducing the amount of info that needs to be written into the octrees for each zone. This is kind of avoiding the real issues though so we will need to get back to actually reducing the time things take, but that is for another day. The main thing to tackle was rewriting the code that find which parts of a tile are solid and which aren't. This was previously hacked into TaleSpire itself and happened on load, but this is daft as it never changes so I have moved this to the tile modding tools.

I'm now going through all the existing assets and adding extra info to all of the floors and reexporting the assets with their new collision info. Tomorrow will basically be finishing that off and using this new data in TaleSpire, which should tale performance back to where it was. I will also serialize the octrees for file/load and syncing floors as it could be  a pretty quick win.

Seeya
