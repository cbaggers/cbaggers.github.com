---
layout: post
title: Delays in Pushing Code
description:
date: 2014-07-28 09:55:06
category:
tags: ['lisp', 'cepl']
---

I have been hoilding off pushing the new features to Cepl recently as I have one bug in defglstruct that really is confusing me.

At first it seems very simple, it is complaining that certain functions (A) dont exist with compiling functions (B). Now (A) do exist by the time (B) is called so it doesnt cause any immediate problems, but the compiler wanrings are very annoying.

The weird bit is that (A) do exist at that time. For example if I expand the defglstruct macro and then run the resulting code it works with no warnings.

There is something I'm just missing but it is the reason I havent merged into master yet.

I may give this some more attention tonight.

Ciao