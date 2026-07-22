---
layout: post
title: TaleSpire Dev Log 510
description:
date: 2026-07-22 17:51:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

We've found a bug with creature tags where they were only working in the community browser and not the library. That fix is in the works and will be shipped as soon as it passes the tests.

It also seems that mod.io has a bug (or has changed it's api) so that tag searches now return results for things that match any of the tags, rather than all of them. This breaks the expected behavior in TaleSpire, as you can't refine searches properly now. We've reported the issue and will keep you posted.

I've got a build of TaleSpire with the new physics and batching performance improvements. It needs final testing, but then it'll be on its way to you. We'll probably keep it in public Beta for one week, just in case anything else has slipped through the cracks.

The batching changes have finally put us in a good place to look at replacing the shadow implementation, which has been blocking us from upgrading Unity and getting some of the fixes and improvements we could really benefit from. I'm currently investigating how difficult it will be. Hopefully, it doesn't put up too much of a fight.

Whatever happens there, my attention will return to the internal systems upgrade as soon as possible. That's a very vague-sounding task, but it should make it much easier to fix a long-standing bug if I can get the code in the shape I want.

More news soon.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
