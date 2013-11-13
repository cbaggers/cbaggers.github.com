---
layout: post
title: Getting Close
description:
date: 2013-11-12 22:27:00
category:
tags: ['lisp', 'cepl']
---

Been a crazy week of headbuttng a macbookpro until a cocoa application worked followed by a fantastic demoparty (thanks everyone at kindergarten for a phenominal weekend!). 

I'm finally back in lisp code (phew!) and I'm around 2/3rds of the way through a rewrite of the Varjo lisp->glsl compiler. 

The reason for the rewrite now is that someone asked for a tour through the code explaining how it worked...after 5 minutes of reading I decided it could only be dark-magic holding it together so I have tried to rewrite it to take into account a few of the things I've learnt.

Given that I wrote it when I really didnt know anything about glsl I had a lot to change! I have also taken a few of the additions from cepl I had been hacking on and made them core features of the compiler. The big one in this category is you can define shader functions that are only included in the glsl source if they are used.
This means you can make a nice 'standard library' of functions for your shaders and know they won't be included unless they are used in a given shader.

There are also macros (regular for now much compiler soon) and generic functions.

I'm hopefully finishing the last of the argument validating this evening and then I think the last big things are reimplementing the special functions and rewriting the string generation. Hopefully I will be done in a week or two.

Right, enough procrastinating, back to work!