---
layout: post
title: TaleSpire Dev Log 147
description:
date: 2020-01-15 08:20:14
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks, another quick 'kept of working' post :)

Yesterday was spent working on Paste and Deletion of single tiles. 

The latter is fun as we don't keep unique ids for individual tiles as the memory usage adds up fast. This means we have to take some network-safe aspects to use to identify the tile. This is slower than looking up an id in a hashmap, of course, but given the frequency it happens the cost isn't that egregious. The upside is not spending that memory, and not having to maintain whatever structures would have given us a direct lookup.

The above is why I've previously mentioned how late I left it to add the delete-single-tile feature to the new codebase, I wanted to be sure what data I would have available to work with. The less data we store per-tile, the better. So in the ideal case, we'd have very little to work with.

Whilst I've coded the hard, data-wrangling parts of paste, I've not finished the implementation as it requires hooking into the board-tool system in TaleSpire and @Ree has been writing on his branch. So that will get finished when we merge those branches.

Which leads us to right now. I'm on the train heading to @Ree's so we can work on that merge. It's gonna be a gnarly few days, but hopefully, we can get something working by Sunday. 

One of the things that has let us work separately like this is that we have not worried about performance on the tooling branch and not worried about tooling on my data branch. We, of course, had to be communicating a lot to make sure neither of us were doing things that couldn't be made fast/useful after the merge. So far, however, it's been working pretty well. This means that once they are merged, I can move to start writing the optimized versions of the operations the tooling uses. And poor @Ree has to make my stuff feel nice :p

Alright, that's the news for now

Ciao

p.s. 

This didn't fit above, but I wanted to mention it anyway. `UnmanagedMemoryStream` is cool!

It's a c# type that takes a pointer and length in its constructor and gives you a Stream type that is compatible with tons of .net's existing APIs. This has really come in handy when I've wanted to compress data from a NativeArray without unnecessary copying. Check it out!
