---
layout: post
title: Ubuntu on Adam Tablet...again
description: 
category:
tags: ['']
---

<img src="http://i173.photobucket.com/albums/w51/Cbaggers/ubuntu_adam.jpg" alt="ubuntu 12.04">

A good while back I got hold of an Adam tablet in order to try and forfill a desire. You see I'm a geek but I love being outdoors and I want technology to progress to where it doesn't limit where I can do what activity.
The Adam seemed to fit the bill nicely; it uses a PixelQi screen which is perfectly readable in direct sunlight and, being a tablet, runs on an Arm processor which means the battery life is good. The only thing that was missing was the OS.
The tablet came with a really bad skin over android which was quickly removed in favour of a decent build of android, and it was OK for a time. But for me I need to be able to create on my machines, I want to code, I want Ubuntu on the tablet.

I, sadly, haven't got the knowledge to put an image together for the tablet but it has been done by a bunch of fantastic people online.

Their ROM got me a working build of Ubuntu 11.04, which I've since upgraded to 12.04 to fix the massive slowdown issues I was having. I have also identified an issue where ttyS0 is being respawned constantly, if your not worried about serial then simply remove ttys0.conf from /etc/init. 

While this is fantastic it still is a little slow for heavy use but this is likely down to the fact that I haven't currently got the graphics drivers installed. I'll post back my findings when I've done this (again an awesome person on the forums has this sussed)

If you would like to follow along or have an Adam Tablet gathering dust the forum is <a href="http://www.tabletroms.com/forums/adam-rom-development/3451-%5Brom%5D-ranbuntu-0-1-ubuntu-11-04-natty-adam.html">here</a>.

Once these last little issues are licked I'll be having a play around with writing tablet apps for Ubuntu, so drop by if thats your thing!
Ciao