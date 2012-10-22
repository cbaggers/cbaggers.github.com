---
layout: post
title: Time to Grind
description:
date: 2012-08-17 11:20:39
category:
tags: ['lisp','cepl']
---

Missed a day of posting here again but progress is being made. 

I seem to be falling into nice rhythm with developing cepl. First I work through some tutorial to add functionality to cepl, then I spend a few days trying to find abstractions for what I've just done.

Right now I'm in the later of those two. I'm getting annoyed with having to remember what type of vector I am using when I run vector functions against it so I'm currently working on generic function which take care of calling the correct function for me so I don't have to think about it. I've also got tired of writing:

    (make-vector3 1.0 2.0 3.0)

..every time I need a new vector and having to make sure the numbers are floats. To that end I have made a macro v! so that

    (v! 1 2 3)

..works exactly the same and the long version except for also handling type conversions.
