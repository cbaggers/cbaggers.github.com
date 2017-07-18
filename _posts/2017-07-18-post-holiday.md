---
layout: post
title: Post Holiday
description:
date: 2017-07-18 10:28:25
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I haven't written for a while as I had a two week holiday and decided to take a break from most things except streaming. It was lovely and I got so much done that this can only really be a recap:

### Performance

I made a profiler for CEPL that is plenty fast enough to use in real-time projects. I then used some macro 'magic' to instrument every function in CEPL, this let me target the things that were slowest in CEPL. I could happily fill a post on just this, but I will spare you that for now. Needless to say, CEPL is faster now.

I am contributing to a project called [sketch](https://github.com/vydd/sketch) by porting it to CEPL and I was really annoyed at how it was doing 90fps on one stress test and the CEPL branch was doing 20. After the perfomance work the CEPL branch was at 36fps but still sucking compared to the original branch. In a fit of confusion I commented out the rendering code and it only went up to 40fps..at which point I realized that, on master, I had been recording fps from a function that was called 3 times a frame :D so the 90fps was actually 30!

The result was pretty cool though as the embarrassment had probably pushed be to do more than I would have done otherwise.

### GL Coverage

I have added CEPL support for:
- scissor & scissor arrays
- stencil buffers, textures & fbo bindings
- Color & Stencil write masks
- Multisample

and also fixed a pile of small bugs.

### Streaming

Streaming is going well although removing the 'fail privately' from programming is something I'm still getting used to. (I LOVE programming for letting me fail so much without it all being on record)

This wednesday I'm going to try and do a bit of procedural terrain erosion which should be cool!

### FBX

FBX is a proprietary format for 3D scenes which is pretty much an industry standard. It is annoyingly proprietary and the binary format for the latest version is not known. There is a zero-cost (but not open source) library from autodesk for working with fbx files but it's a very c++ library so using it from the [FFI](https://en.wikipedia.org/wiki/Foreign_function_interface) of most languages is not an option.

Because this is a pita I've decided to make the simplest solution I can think of, use the proprietary library to dump the fbx file to a bunch of flat binary files with known layout. This will make it trivial to work with from any language which can read bytes from a file. I'm specifically avoiding a 'smarter' solution as there seem to be a bunch of projects out there in various states of neglect and so I am making something that, when autodesk inevitably make a new version of the format, I can change without much thought.

### The effect on my life

It's odd to be in this place, 5 years of on/off work on CEPL and this thing is starting to look somewhat complete. There is still a bunch of bugfixes and newer GL features to add but the original vision is pretty much there. This seems to have freed up the mind to start turning inwards again and I've been in a bit of a slump for the last week. These slumps are something I have become a bit more aware of over the last year and so have become a little better at noticing.. Still, as this is about struggles as well as successes and as the rest of this post is listing progress it seemed right to mention this.

Peace folks
