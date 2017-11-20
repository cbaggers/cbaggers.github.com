---
layout: post
title: Little bits of progress
description:
date: 2017-11-20 09:49:00
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Hi again! This last week has gone pretty well. Shared contexts have landed in CEPL master, the only host that supports them right now is SDL2 although I want to make a PR to CEPL.GLFW as it should be easy to support there also. Glop is proving a little harder as we really need to update the OSX support, I started poking at it but it's gonna take a while and I've got a bunch of stuff on my plate right now.

I started looking at [multi-draw](https://www.khronos.org/opengl/wiki/Vertex_Rendering#Multi-Draw) & [indirect rendering](https://www.khronos.org/opengl/wiki/Vertex_Rendering#Indirect_rendering) in GL as I think these are the features I want to implemented next. However I immediately ran into existing CEPL bugs in defstruct so I think the next few weeks are going to be spent cleaning that up and fixing the struct related issues from github.

AAAAges back I promised beginner tutorials for common lisp and I totally failed to deliver. I had been hoping the Atom support would get good enough that we could use that in the videos. Alas that project seems to have slowed down recently[0] and my guilt finally reached the point that I had to put out *something*. To that end I have started making a [whole bunch of little videos](https://www.youtube.com/playlist?list=PL2VAYZE_4wRJi_vgpjsH75kMhN4KsuzR_) on random bits of common lisp. Although it doesnt achieve what I'd really like to do with proper tutorials, I hope it will help enough people on their journey into the language.

That's all for now,
Peace.


[0] although I'm still praying it get's finished
