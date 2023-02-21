---
layout: post
title: TaleSpire Dev Log 377
description:
date: 2023-02-21 20:02:25
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

Since we last spoke, I've carried on with my main two tasks, slab upload, and symbiotes.

## Slab Upload

Slab upload is going well. It's always nice when something starts feeling like a feature and not a hack. 

The progress isn't exciting to show, though. It's been things like:

- Side-load the slab data into the cache, so it doesn't have to be redownloaded
- Show an hourglass cursor when the slab tool is waiting on a slab to download
- Tuning camera behavior so that transitioning in and out of publish mode does what is expected.
- Get tabbing between fields working as expected
- Subtle visual changes to screenshot view to improve the experience
- Add loading graphics to the entries in the community-mod browser to show that thumbnails are downloading

And so on.

I've still got a list of tickets to get through to get a good first version, but there don't seem to be any show-stoppers.

The one (happy) distraction has been helping with the Symbiotes feature.

## Symbiotes

> Context: Symbiotes is an upcoming feature allowing community-made mods to dock on the right side of the TaleSpire window.

Our first version of this feature supports Symbiotes powered by WebViews[0]. 

We are providing an API that allows communication with TaleSpire. The messages between TaleSpire and the Symbiote travel over a simple interconnect provided by the WebView.

The code on either side of such a bridge needs to match and, from my experience, are places where simple user errors result in extremely annoying bugs. To deal with that, I prefer to write the API specification as a simple document, which is then used to generate the plumbing code for either side.[1]

And so that's what I'm making. I have a JSON document specifying types, calls to TaleSpire, and the arguments and return types of those calls. I load that document, type-check it, and produce an intermediate tree of objects which describe the bridge.

Next, I'll write code to walk over that tree to spit out the JavaScript and C# boilerplate code required.

Some of you may be asking, "why aren't you using <favorite-serialization-library> for this?". The answer is slightly dumb and slightly sensible, and it's "I can make this approach more quickly as I haven't used that library yet." I'm actually quite interested in 
[flatbuffers](https://google.github.io/flatbuffers/) for symbiotes and server communication. But that said, I am very used to writing these kinds of generators. I know how to make something that solves our exact problem and do it in a short amount of time. 

Given that we are trying to ship mods quickly and that this approach doesn't stop us from using a different serialization approach in the future, this is the way we are going for now.

Alright, enough rambling for today. Looking forward to sharing more with you soon.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*


[0] In the future, I want to explore supporting other languages/environments.
[1] This is exactly what we do with our server-side API. The API is represented as an erlang data structure, and from that, we generate C# and erlang plumbing code. It's been a huge boon.
