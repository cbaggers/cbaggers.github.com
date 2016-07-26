---
layout: post
title: A bunch o stuff
description:
date: 2016-07-26 13:10:32
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Ok so it's been a couple of weeks since I wrote anything, sorry about that. 2 weeks back I was at [Solskogen](www.solskogen.no) which was awesome but I was exhausted afterwards and never wrote anything up, let's see if I can summarize a few things.

### PBR
I'm still struggling with the image based lighting portion of  physically based rendering. Ferris came round for one evening and we went through the code with a comb looking for issues. This worked really well and we identified a few things that I had misunderstandings about.

The biggest one (for me) was a terminology issue, namely if an image is in `SRGB` then it is non linear, but an image in an `SRGB texture` is linear when you sample it. I was just seeing SRGB and thinking 'linear' so I was making poor assumptions.

It was hugely helpful to have someone who has a real knowledge and feel for graphics look at what I was doing.

I have managed to track down some issues in the point lighting though so that is looking a little better now. See the image at the top for an example, the fact that the size of the reflection is different on those different materials is what I am happiest to see.

### C Dependencies
Every language needs to be able to interface with C sooner or later. For lisp it was no different and the libraries for working with shared objects are **fantastic**. However the standard practice in the lisp world (as far as I can see) is not to package the c-libraries with the lisp code but instead to rely on them being on the user's system.

This is alright on something like linux but the package manager situation leaves a lot to be desired on other platforms. I have been struggling with Brew and MacPorts shipping out of date or plain broken versions of libraries. Of course I could work on packaging these myself but it wouldnt solve the Windows case.

Even on linux sometimes we find that the cadance of the release cycles is too slow for certain libraries the [asset importing library](http://www.assimp.org/) I use is out of date in the debian stable repos.

So I need to package C libraries with my lisp libraries, that ok, I can do that. But the next issues is with users shipping their code.

### Shipping Lisp with C Libraries
Even if we package C libraries with lisp libraries this only fixes things for the developer's machine. When the dump a binary of their program and give it to a user, that user will need those C libraries too.

The problem is that 'making a binary' in lisp generally means 'saving the image'. This is like taking a snapshot of your program as it was in a particular moment in time..

Every value

Every variable

Every function

..is saved in one big file. When you run that file the program comes back to life and carries on running (some small details here but meh).

The problem is that one of the values that is saved is *the location of the c libraries* :p

So this is what I am working on right now, a way to take a lisp project, find all the c-libraries it uses, copy them to a directory local to the project and then do some magic to make sure that, when the binary is run on another person's machine, that it looks in the local directory for the C dependencies.

I think I have 90% of the mechanism worked out last night. Tonight I try and make the bugger work.

### Misc
- Fixed bugs running CEPL on OSX
- Added implementation specific features to lisp FFI library
- Add some more functions to `nineveh` (which will eventually be a library of helpful gpu functions)
- Fix loading of cross cubemap hdr textures
- add code to unbind samplers after calling pipelines (is this really neccesary?)
- fix id dedup bug in CEPL sampler object abstraction
- added support for Glop to CEPL (a lisp library that can create a GL context and windows on various platforms, basically a lisp alternative to what I'm using SDL for)
- GL version support for user written gpu functions
- super clean syntax for making a fbo with color 6 attachment bound to the images in a cube texture.
- support bitwise operators in my gpu functions
- Emit code to support GLSL v4.5 implicit casting rules for all GLSL versions

And a pile of other bugfixes in various projects
