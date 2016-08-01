---
layout: post
title: Stuck in port
description:
date: 2016-08-01 12:08:40
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I have been looking at shipping cl code this week, some parts of this are cool, some are depressingly still issues.

`save-and-die` is essentially pretty easy but as I use C libraries in most my projects it makes things interesting as when the image loads and looks for those shared-objects again the last place it found them. That's ok when I'm running the project on the same machine but not when the binary is on someone else's.

The fix for this is to, just before calling `save-and-die`, close all the shared-objects, and then make sure I reload them again when the binary starts. This is cool as I can use #'list-foreign-libraires and make local copies of those shared-object files and load them instead. Should be easy right?..Of course not.

OSX has this concept called [run-path dependent libraries](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/RunpathDependentLibraries.html), feel free to go read the details but the long and the short is that we can't just copy the files as they have information baked in that means it will look for dependent libraries outside of our tidy little folder.

Luckily the most excellent [shinmera](https://github.com/Shinmera) has, in the course of wrapping QT run into everything I am and plenty more and dealth with it ages ago. He has given me some tips on how to use [otool and install_name_tool](http://thecourtsofchaos.com/2013/09/16/how-to-copy-and-relink-binaries-on-osx/). Also from talking to Luis on the CFFI team we agree that at least of the unload/reloading stuff could be in that library.

To experiment with this stuff I started writing [shipshape](https://github.com/cbaggers/shipshape) which let's me write `(ship-it :project-name)` and it will:
- compile my code to a binary
- pull in whatever media I specify in a 'manifest'
- copy over all the C libraries the project needed and do the magic to make them work.

This library works on linux but not OSX for the reasons listed a couple of paragraphs up. However it is my playpen for this topic.

In the process of doing this I had a facepalm moment when I realized I hadn't looked beyond `trivial-dump-core` to see what was already available for handling the dump in a cross platform way. Of course asdf has robust code for this. I felt so dumb that I know so little about ASDF that I resolved to thoroughly read up on it. Fare's writeups are awesome and I had a great time working through them. The biggest lesson I took away was how massively underspec'd `pathnames` are and, whilst uiop does it's best, there are still paths that cant be represented as `pathnames`. Half an hour after telling myself I wouldnt try and make a path library I caught my brain churning on the problem so I started an experiment to make to most minimal reliable system I can, it's not nearly ready to show yet but the development is happening [here](https://github.com/cbaggers/pathology). The goal is simply to be able to define and create kinds of path based on some very strict limitations, this will no be a general solution for any path, but should be fine for `unix` and `ntfs` paths. The library will not include anything about the OS, so then I will look at either hooking into something that exists or making a very minimal wrapper over a few functions from libc & kernel32. I wouldnt mind using iolib but the build process assumes a gcc setup which I simply cant assume for my users (or even all my machines).

One thing that I am feeling really good about is that twice in the last week I have sat down to design APIs and it has been totally fluid. I was able to visualize the entire feature before starting and during the implementation very little changed. This is a big personal win for me as I don't often feel this in any language.

Wow, that was way longer than I expected. I'm done now. Seeya!
