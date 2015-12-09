---
layout: post
title: Spaces still in progress
description:
date: 2015-12-09 23:48:23
category:
tags: ['lisp', 'cepl']
---

Bloody hell that month went fast. Well the obvious is that the spaces feature is not done. I lost a week an a half to business, but it was in palo alto so I got to visit the computer history museum which was badass.

I'm also stoked at how much I did get done in that time. Varjo progress is marching onwards and the parts of cepl I found hairy are slowly becoming less so.

The big changes are happening in varjo purely because varjo is outgrowing the simple model that has served it up to now. Right now I'm making sure that a varjo compile pass not only gives you the result (and metadata) but also includes the ast. Varjo is currently single pass (and that was fine) but cepl wants to be able to use varjo in a multipass fashion. It needs the ast so it can create the varjo code for the second pass (where, in this case, spaces are turned into matrices).

It is likely that, in time, varjo will just become properly multipass. But I'm going to do this in steps as I don't want to be working on features for features sake, I want the space feature to drive this.

More news as it comes
