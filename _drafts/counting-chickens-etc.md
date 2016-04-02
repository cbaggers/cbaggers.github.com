---
layout: post
title: Counting chickens etc
description:
date: 2016-04-02 17:59:13
category:
tags: ['lisp', 'cepl']
---

Well, whilst the post the other day was full of excitement I may have wanted to take a little longer thinking about it, as there a few things that will a bit more looking at the code I have decided were a bit 'off'.

### render-targets

Are not neccesary :) in OpenGL the FBO object is so lightweight that it is not even required to be shared between shared contexts. Many implementations still will but *dont rely on this behaviour!* I lost a good few days to a device & manufacturer specific android bug where this one phone actually adhered to the spec and didnt share the fbos (lots of unhelpful crash messages those days).

So if FBOs are cheap then we dont need a specific object to cache the settings on them, just make more all the extra cache I wanted
on the render-targets could just be in the FBO struct.

I think I do need a nicer way to copy an FBO though.

### samplers

Whilst I was wrong about the current api, I'm still interested in updating these. It will make more sense as the shaders
expect to take uniforms of type :sampler-* (:sampler-2d :sampler-cubes etc) but I am currently passing in the textures.

It will get rid of the with-sampler syntax.

I am also pretty sure I am dropping support for setting sampling params on the texture. There is never a good reason to not be using sampler objects. This means that either: minimum GL version for CEPL is 3.3 *or* I happy my sampler wrapper set the texture params.

I am leaning towards the latter right now.

Either way the interface is unified and simpler to reason about.



Let's see if this changes again in the next few hours :D
