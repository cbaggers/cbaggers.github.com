---
layout: post
title: Phew! Lots Happening
description:
date: 2013-10-19 17:27:00
category:
tags: ['lisp', 'cepl']
---

It has been a crazy few months. I got offered, and accepted, a job at a startup in Norway so about a month back I moved and set up in Oslo.

I'd finally settled in and suddenly CEPL turned up on reddit and people were checking it out, which was great...except I had been making loads of breaking changes in master so it was a bloody mess!

Things are really coming along well though so I need to make some videos showing how the memory handling works. I think I'll do that tomorrow.

The most recent change I have been making is a rewrite of CEPL's front-end to the Varjo shader compiler. Previously, when you defined a shader pipeline in CEPL you have to write the shaders inside the defpipeline form. Now they can be defined as separate entities and then the defpipeline stitches them together. Also you can define individual functions outside of the shader stages and they will get mixed in only if they are used in a stage. Along with shader macros I now have some nice tools to experiment with different ways of extending glsl.

I am going to rewrite my ray marcher example to use these features to see what effect it has on the ease of writing and reasoning about the shader pipeline.

Here is a video of the compiler stuff:

<iframe width="420" height="315" src="http://www.youtube.com/embed/2Z4GfOUWEuA" frameborder="0" >  </iframe>
