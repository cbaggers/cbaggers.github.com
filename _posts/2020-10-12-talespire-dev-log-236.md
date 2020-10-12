---
layout: post
title: TaleSpire Dev Log 236
description:
date: 2020-10-12 12:13:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today in "I've not been working on line-of-sight but…", we'll be talking about things that ended up higher priority than starting on line-of-sight :P

On Friday, I was chatting to Ree about the next tasks on the list, and he suggested that it could be helpful to convert the boards from the beta format sooner rather than waiting for the per-zone-sync. At first, I balked at this, but after a sleep, I realized that much of the work would be valid even when we come to rewrite the new persistence system.

And so that's where I've been. Format conversion is always sensitive, but Ι took the opportunity to simplify how we handle it. Now, each time we need to make a new version, we'll:
- copy all the current types into a new numbered 'legacy' namespace
- strip out everything beyond the deserializing code
- write a converter from that version to the latest version.

It's dumb and involves more leg work than fancier solutions, but it works. It's reassuring to know that you aren't accidentally messing with something needed by an older version of the board you still need to support when making a change. We'll see how this feels to work with, but I've tried a couple of other approaches before and have found them lacking. Let's see how Ι do with this one.

After getting conversion working, I realized the codebase was in a good place for some cleanup. I had a bunch of naming (of variables and types) that needed updating, and there is not likely to be another good time to do it before the Early Access. So Ι threw a few hours at this.

I noticed a case where, on photon failing to connect, we wouldn't retry, essentially leaving you stuck with no board. I've put a simple retry in on my branch and will cherrypick this into master tonight. This will be available in the next release.

During testing of board format conversions, I found a case where the physics wrapper would incorrectly schedule some jobs. This is now also fixed.

That's the lot for now. Back soon with more
