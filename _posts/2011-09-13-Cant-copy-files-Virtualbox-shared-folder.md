---
layout: post
title: Can't copy files Virtualbox shared folder
description: 
category:
tags: ['']
---

Just had a very weird issue here that turned out to have a very logical solution.



I set up a shared folder between my virtual guest and the host machine. I set it to auto mount and threw a couple of files into the folder. 

I booted up the virtual windows instance and browsed to the shared folder and tried to copy the files out of the shared folder...it worked but the files promptly vanished and were replaced by one file with 0 byte size. 



Bugger...I tried again; this time browsing to a samba network share and pasting the files....again within seconds they were replaced with a 0 byte file.



The problem was that the filename given by the linux screenshot program was not legal on windows. So I changed the file names and suddenly it all worked fine.



So the lesson is, check them names!

Hope this helps someone