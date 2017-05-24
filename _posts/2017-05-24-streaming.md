---
layout: post
title: Streaming!
description:
date: 2017-05-24 18:53:35
category:
tags: ['lisp', 'cepl', 'varjo', 'nineveh']
---

This'll be quick post, both because I'm a day late posting this and also because I'm streaming in an hour.

Staying on that subject, I streamed a thing! I'm pretty stoked as it seemed to go well (despite nerves) and the response has been great. You can find the recording below.

Other than that I've been doing some research for future streams, fixing bugs in Varjo and adding some features to various libraries. I have a day off tomorrow so I'm thinking of hammering on with color conversion functions and also with mesh generation.

I've also been putting some time into porting [sketch](https://github.com/vydd/sketch) to CEPL but I'm still struggling with performance. The original had minimal state changes (1 program, 1 vao) and used buffer streaming to push tonnes of verts every frame.. however it makes >4000 draw calls a frame. This use case runs into some rather slow parts of CEPL so I'll need to do work there. More on this next week

Right, time to go.

Ciao

<iframe width="560" height="315" src="https://www.youtube.com/embed/82o5NeyZtvw" frameborder="0"></iframe>
