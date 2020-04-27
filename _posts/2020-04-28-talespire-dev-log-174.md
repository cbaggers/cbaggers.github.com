---
layout: post
title: TaleSpire Dev Log 174
description:
date: 2020-04-28 00:23:03
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Ah, it's a been a day that reminds you that rarely in programming can you 'just' do something.

On Saturday, I wanted to push a patch that fixed a bunch of bugs, one of which was related to an SSE4 requirement in the current build.

The fix should have been simple, SSE2 support has been added in later versions of the Burst compiler, we can *just* upgrade the package and rebuild.

The package is still in preview, and so there can be bugs. However, sometimes that kind of tradeoff is worth it, especially given that we are still in Beta. If it passed our tests, I'd have been happy.

Alas, we quickly saw issues which only occurred in the Burst compiled jobs. Time to get testing :)

As expected, the commit that caused the issue was the one where we upgraded the packages. We then tested each version of Burst and Unity.Collections to track down the first version, which caused the error.

In the release notes for the earliest problematic version we saw that one change was:

> Improve codegen for structs with explicit layout and overlapping fields.

This was a great candidate as we had a type that is essentially a c# union. You can write such structs in c# like this:

```
[StructLayout(LayoutKind.Explicit)]
public struct Test
{
    [FieldOffset(0)] public int A;
    [FieldOffset(4)] public Bar B;
    [FieldOffset(4)] public Baz C;
}
```

I took our structs and removed parts until I had a simple case that showed the issue. I then moved it to a separate project, re-confirmed the packages and pulled other Unity versions to make sure it was still an issue.

I've made [a repo for the test](https://github.com/Bouncyrock/potential-burst-sizeof-bug), and tomorrow I'll report this as a potential issue to Unity.

Now the take-away from this is not to shit on Unity. This is an under-development build of the package explicitly available for finding these kinds of issues. This is just development.

However, it's a fun example of how 'just download it' is almost never really the case in any significant project.

Hope this finds you well, sorry I've not been around the community today, constantly recompiling does not make one that sociable :p
