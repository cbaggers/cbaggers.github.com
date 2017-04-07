---
layout: post
title: Lisping Elsewhere
description:
date: 2017-04-07 12:53:13
category:
tags: ['lisp', 'varjo']
commentIssueId:
---

From Sunday to Wednesday I was away on Brussels at the European Lisp Symposium. It's an opportunity to see good talks but more importantly to me it's a catch up with some folks I haven't seen in a year and nerd out.

Aside from that I've been cracking away at getting [sketch](https://github.com/vydd/sketch) working using CEPL under the hood. It's rendering using lisp shaders now and live recompile works great. There has been a slight performance drop as `sketch` used [buffer object streaming](http://onrendering.blogspot.no/2011/10/buffer-object-streaming-in-opengl.html) to upload the vertices and I'm not doing that yet. In fact CEPL doesn't expose `glMapBufferRange` at all so first order of business is fixing that. After that I want to make a `vertex-ring` it's going to be an object that encapsulates the buffer object streaming pattern and will make using it seamless.

I have also been working on fixes to [Varjo](https://github.com/cbaggers/varjo) that will finally get Geometry shaders working. The task for the last few days has been rewriting how Varjo handles `return` & making Varjo use [interface blocks](https://www.khronos.org/opengl/wiki/Interface_Block_(GLSL)) for passing data between stages. The reason for working on `return` (apart from it being buggy) was that in geometry shaders your primary task is to 'emit' extra geometry rather than 'returning' transformed data via the `out` vars.

I'm hoping that this weekend I can get those two things coded and tested. After that I need to look at the Geometry shaders themselves, I know there are some ugly details around `in`s & `out`s so wish me luck.
