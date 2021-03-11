---
layout: post
title: TaleSpire Dev Log 261
description:
date: 2021-03-09 09:40:12
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Howdy folks!

We had a round of testing to see where we are at, and it's pretty positive. There are significant bugs to squash, and the new backend hasn't landed yet. However, it took a little while to start getting crashes, so that was positive. It is expected at this point for it to be somewhat broken, we just needed to see if there were things we weren't expecting (for the record, the first time we did this for the beta, it was a nightmare :D)

I've spent this last week on bugs:

- an issue with assets jumping when being picked up making tooling feel bad
- a crash when an asset is missing from the pack rather than using the dummy asset
- atmosphere not being correctly applied when joining the board
- fix sync of spaghet scripts
- get gm requests working again
- fix a bug where a scripted asset with no MeshRenderer freaked out my batcher
- an issue where static lights were not being invalidated on change
- fix an incorrect vertical offset of slabs that were elevated when copied

And then a stinker: Assets using our fake-translucency plastic shader don't look correct with the new lighting system.

This thing has dogged me for days now. It's on the critical path as this shader is fundamental to how TaleSpire looks and, more immediately, we can't record the trailer until it's fixed.

Here's what the problem looks like:

![incorrect fire effect](/assets/images/fireWrong.png)

It is happening because we had to write our own lighting to move tiles and props away from Unity's GameObject system (because they were too slow). However, this means that a lot of lighting magic that Unity did for us is our problem instead.

Fire, for example, used to render in multiple steps.

![incorrect fire effect](/assets/images/fireOld.png)

First, the base object is rendered. Then it is rendered once for each nearby light, with the result being additively blended with the previous pass.

I had a look at the shaders to see how things were split up, and I figured that we could replicate the effect in a single pass if we could get the light data in a buffer along with some info on where in the buffer to find the lights for the current rendering asset.

But first I needed to make the shader. This took me a day and a fair bit of swearing (visuals are not my strong suit), but I did get a proof of concept working, enough to get back to the renderer.

This also proved to be a real pain in the ass. The batching code that collects light information was written to group lights of the same kind, regardless of what tile or prop they belong to. This is done to ensure that, when rendering, there are a few unnecessary state changes as possible. However, we now need the information grouped per-tile/prop and in a ComputeBuffer. Swearing and coding continued until I had a nice little allocator for the ComputeBuffer, and all the batching jobs were updated to use it.

Finally, we could port over the new shader and wire everything up.

![incorrect fire effect](/assets/images/fireNew.png)

At last! We have something looking correct.

We still need to wire up the changes to the other plastic shaders used for crystals and the like, but that is trivial now that we know the approach works.

Today I'm back on backend work, and I will be for the next week. This thing is shaping up fast now.

Seeya in the next one folks.

p.s. This dev-log is rather late so the next one will be coming very soon
