---
layout: post
title: Rolling forward
description:
date: 2017-09-08 11:24:57
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Once again I don't have much to report but things are going ok.

The streams are still going and still going well, this week was using a cute approach from http://nullprogram.com/blog/2014/06/01/ to make voronoi diagrams. You can see that stream [here](https://youtu.be/82o5NeyZtvw)

I also revisited my bindings to the [Newton Dynamics](http://newtondynamics.com/) physics engine. Last time I had tested it seem they were abysmally slow. Luckily a quick review showed me that the 'max fps' for the simulation was set too low and that to something sensible made a world of difference. Some profiling also revealed that the bindings have a non-trivial amount of overhead which I believe I can remove by declaring the types and turning up the performance optimizations.

That's all for now
Ciao
