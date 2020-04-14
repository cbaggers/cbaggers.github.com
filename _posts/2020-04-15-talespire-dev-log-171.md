---
layout: post
title: TaleSpire Dev Log 171
description:
date: 2020-04-15 00:53:18
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today has gone slow and steady.

In the morning, we wrapped up the second beta patch and got that out. I spent a couple of hours around community conversations and then got down to work.

The goal is fixing the HardAssert errors that are plaguing people. To understand where those are coming from, we have to dive into some implementation details.

TaleSpire spawns a lot of objects, and we have to spawn them on short notice. We have no way to know at what point the player will just decide to drag out 10000 tiles and expect it to just work. Unity cannot spawn that many tiles in one frame[0], and so we have to spread the load over multiple frames.

The board is divided into 16x16x16 unit zones, and each operates largely independently of the others. This is important as boards have the potential to be huge and so we will want to only spawn the tiles in the area we are in. Also, because of multi-gm support, building could be happening in any other part of the board. This means TaleSpire must allow for zones to be in memory and fully editable without having to have spawned all the tiles in the zone. [1]

To that end, every zone may have a presentation. The presentation is the visual component of a zone and is what handles spawning tiles from the zone.

One issue with spreading an operation over many frames is that by the time it completes, another operation may have already arrived and have needed to edit the tiles you are spawning. Because of this, the presentation has a queue of changes to be applied. This means an operation makes its change to the data-model and then pushes the required change (and potentially new assets) into the presentation's queue. We then currently give four milliseconds per frame [2] for the presentations to apply as many ops as they can.

Doing this, we give ourselves the ability to keep a reasonable frame rate. We don't do a great job at this right now, but the pieces are in place for this to work [3].

Now let's throw in networking. We cannot send all the tile data for each operation across the network; it's not feasible. What we do instead (much like how RTS handle troop movements) is to send a description of what we need to be done (the operation) and let each client apply that change. As you can imagine, all clients have to agree on the order to apply these, or each will end up with a different end result. The details of that are beyond the scope of this post, but regardless we can talk about the issue that arises from this.

Agreeing on an order, and communicating the operations takes time, but games feel terrible unless actions are immediate. What we can (and do) do is to apply local changes immediately and then wait to see what order is agreed upon. If it turns out, we were already in the right order, then great! If not, we need to effectively revert our change, apply the changes that came before ours, and then reapply ours. Again making this fast in TaleSpire could be its own post, so I will give a TLDR by saying the way we do it involves applying the local changes eagerly to the presentation (and not the data) and then apply the change to the data-model when it arrives.

Finally, we can talk about the bugs :P

The HardAssert issues I'm currently working on are occurring when there is a mismatch on the presentation between the number of tiles the queued operation says needs to be spawned, and the number of tiles in the inbound queue. Clearly, there is something awry here, and so I am attacking one source of complexity in the code that enqueues operations for the presentation. The code in question was meant to act as an optimization in cases when a chunk of tiles is added and removed in the same frame (which can happen in undo/redo if you are super fast). However, given the rarity of that action, and the severity of the issues, I'm removing this in favor of something simpler for me to understand and debug.

Given all of the complexity I mentioned above (and plenty I left out), you can imagine that this has to be done rather carefully.

Today I managed to make the simplification, and so tomorrow I can start looking at the HardAssert errors themselves. I'll then be talking to people in the community who have offered to let me use their boards as test cases, and we'll see what we can learn from there.


Now that we have covered some of the background, I can also explain why we haven't just added local save files, as many have suggested as a temporary fix.

When code in Unity throws an exception, it does not halt the whole program as you might expect. Instead, it travels up the stack to the calling MonoBehaviour and destroys that instead. Now let us imagine that that MonoBehaviour was handling something like applying the messages which have had their order decided. In that case, we can see what will happen, our local changes will be eagerly applied, but when you save and reload, lots of stuff is missing. In fact, everything that was changed after the MonoBehaviour died will be missing as all you were seeing was the presentation of the assets.

Of course, there should be something that freaks out if a message goes missing like that, but clearly, that is not happening. It's very curious and very likely related (assuming this theory is correct)

We can also suppose that the cases where people see duplicate assets on reload could also be related. Imagine what happens if a delete message is lost. Locally we eagerly apply the change, but the delete message never arrives to change the data. So next time you reload, the tiles are back! In some cases, it's more subtle as tiles made afterward are still there. Going into that would make this post even longer, so I'll pass on that for now.

The takeaway from all of this is that all of it should be fixable, I'm not expecting fundamental issues here, just compounded bugs which result in a spectrum of exceedingly aggravating and destructive behavior.

As I progress on this, I'll keep you abreast of what I'm finding.

Have a good one!

Peace.


[0] Unitys new DOTS systems change a lot about the performance of such operations. However, we could not move to it for the Beta as until very recently, their hybrid renderer did not support custom per-instance data, and that is critical to TaleSpire.

[1] We apply most operations in parallel using Unity's job system, which means we can make very large data changes very quickly. There is also a lot we can do to improve this more, but that's a subject for when things are more stable.

[2] This obviously should be adaptive to the current load, but it isn't yet.

[3] This is a recurring theme of the rewrite from the alpha to the Beta. We have tried to put ourselves in a place where the goals we set for Early Access are achievable, even though certain features might not exist right now.
