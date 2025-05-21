---
layout: post
title: TaleSpire Dev Log 474
description:
date: 2025-05-21 20:04:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

Here's a smattering of stuff I've been up to recently.

I'm getting ready to take [this fix to slab placement](https://bouncyrock.com/news/articles/slab-fix-beta-requesting-help-from-builders) out of Beta. It's had good testing from folks in the community, so I'm happy we'll finally be shipping it. I've got the build on our pre-production branch, and I'll do the final release testing tomorrow.

I've been helping out a bit with the WIP particle system. My role is writing the spec, stubbing out the API, and then setting Alex loose on it! Things are taking shape well there. Alex has been refactoring his particle experiments into more reusable code. He's also been researching what core operations are needed for us to build the rest of the standard library. We are slowly cornering the design, and it's a lot of fun. 

Yesterday afternoon, however, I simply couldn't get my brain to focus on particles, so I read about the network sync of physics objects. This meant I was in the perfect place to start a VERY overdue task: rewriting how our dice are synchronized.

The code that manages the dice has been around since before TaleSpire was called TaleSpire, and has been frankensteined over the years as we have added more features on top of it. Due to this, it has some very long-standing bugs. It is way past time it got some love. Until now, every die was its own synchronized entity in Photon, so it was often tricky to treat them as a group. In the new version, the "roll" is the synchronized object, and we handle the data packing ourselves. The result should be greater stability and hopefully less traffic used.

Until now, we have limited the number of dice in a single roll to 40. There have, however, been mods that hack around this. In the new version, the protocol will have a fixed limit of 64 dice per roll[0]. It will not be possible to change this without replacing the sync code. 

You can still have many hundreds of dice on the board, but each roll will have that hard limit. This limit also won't affect multi-part rolls (which will come with the improved dice macro system later).

> We are aware that it's possible, in games like 40K, to have some utterly mad rolls, but we think we can tackle mega rolls as their own feature in the future when it becomes an issue.

We do not foresee this being an issue for general play, but we didn't want it to seem like we were sneaking this change past you. So now you know!

I'll definitely write more about this as it gets closer to shipping. With the roll code rewrite, it's going to be even easier to add the [dice automation](https://bouncyrock.com/news/articles/talespire-dev-log-446) later this year.

That's all from me today.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] The UI may limit to less (e.g. 60) as a round number and will feel more natural to most folks.
