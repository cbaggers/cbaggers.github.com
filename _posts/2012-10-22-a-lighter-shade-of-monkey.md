---
layout: post
title: A Lighter Shade of Monkey
description:
date: 2012-10-22 20:43:39
category:
tags: ['CEPL', 'lisp', 'opengl']
---

<iframe width="420" height="315" src="http://www.youtube.com/embed/YsQZ20ZnnNA" frameborder="0" >  </iframe>

BWUHHAHA I'm back! I have spent the last while working on code that really doesnt make for good
videos so I have been quiet but I have something new to show.

This video shows a really simple directional light acting on the monkey head model.

What I'm really pleased with is that I didn't need to be rigorous which checking that the code would work before I ran it. I was able to run it an mess around with things until they worked. A good example was with the ordering of the matrix multiplications. If you have done some of this stuff yourself you'll know that, unlike multiplying regular numbers the order you multiply matrices is very important to the outcome so while:
     2*4 = 4*2 
..It is not the same when using matrices. I frankly was confused with the orders when I first ran the demo and it showed! The monkey head was all over the place and the light wasn't working. What was wonderful was I was able just to keep swapping things around until I got it working. I caused a lot of bugs in the process but as you are still able to compile parts of your code while the debugger is waiting I was able to undo my mess and carry on running the demo as if I wasn't incompetent!

All in all I'm pretty chuffed with all of this and have plenty more to show too. I will defintely have to write a long post about all the work I have been doing on the opengl wrapper part of CEPL, it makes the code much easier to write, and read for that matter! 

Well that's all for another day,
Take care.