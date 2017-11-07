---
layout: post
title: Multiple Contexts are ALIVE..kinda
description:
date: 2017-11-07 08:56:13
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Allo again!

Its now November which means it's [NanoWrimo](https://nanowrimo.org/) time again, each year I like to participate in spirit by picking a couple of features for projects I'm working on and hammer them out. This is well timed as I haven't had much time for adding new things to CEPL recently.

The features I want to have by the end of the month are decent multi-context support and single stage pipelines.

### Multi-Context

This stuff we have already talked about but I have bit the bullet and got coding at last. I have support for non-shared contexts now but not shared ones yet, the various hosts have mildly-annoyingly different approaches to abstracting this so I'm working on finding the sweet spot right now.

Regarding the defpipeline threading issues from last week I did in the end opt for a small array of program-ids per pipeline indexed by the cepl-context id. I can make this fast enough and the cost is constant so that feels like the right call for now. CEPL & Varjo were never written with thread safety in mind so it's going to be a while before I can do a real review and work out what the approach should be, for now its a very 'throw some mutexes in and hope' situation, but it's fine...we'll get there :p

One side note is that all lambda-pipelines in CEPL are tied to their thread so don't need the indirection mentioned above :)

### Single Stage Pipelines

This is going to be fun. Up until now if you wanted to render a fullscreen quad you needed to:

- make a gpu-array holding the quad data
- make a stream for that gpu-array
- make a pipeline with:
 - a vertex shader to put the points in clip space
 - a fragment shader

The annoying thing is that the fragment shader was the only bit you really cared about. Luckily it turns out there is a way to do this with geometry shaders and no gpu-array and it should be portable for all GL versions CEPL supports. So I'm going to prototype this out on the stream on wednesday and, assuming it works, I'll make this into a CEPL feature soon.

That covers making pipelines with only a fragment shader of course but what about with only a vertex stage? Well [transform feedback](https://www.khronos.org/opengl/wiki/Transform_Feedback) buffers are something I've wanted for a while and so I'm going to look into supporting those. This is cool as you can then use the vertex stage for data processing kinda like a big pmap. This could be handy when you data is already in a gpu-array.

### Future things

With transform feedback support a few possibilities open up. The first is that with some dark magic we could run a number of variants of the pipeline using transform feedback to 'log' values from the shaders, this gives us opportunities for debugging that weren't there before.

Another tempting idea (which is also easier) is to allow users to call a gpu-function directly. This will

- make a temporary pipeline
- make a temporary gpu-array and stream with just the arguments given (1 element)
- run the code on the gpu capturing the result with a temporary transform-feedback buffer or fbo
- convert the values to lisp values
- dispose of are the temporaries

The effect is to allow people to run gpu code from the repl for the purposes of rapid prototyping. It obviously is useless in production because of all the overhead but being able to iterate in the repl with stuff like this could really be great.

### That'll do pig

Right, time to go.

Seeya soon
