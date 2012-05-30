---
layout: post
title: Remote execute windows command from Ubuntu (psexec for linux)
description: 
category:
tags: ['']
---

Hey all, just a quickly as this took a little googling.

If you need winexe to remote administer a windows box..



Grab a package from here: <a href="https://launchpad.net/~jdthood/+archive/winexe/+packages">https://launchpad.net/~jdthood/+archive/winexe/+packages</a>



Install it



Finally open a terminal and try:

winexe -U domain\\username //machinename 'cmd'



Also you may want to hunt down wmic (wmi-client), its also not in the repos, but is available to be compiled from zenoss.

If I find the guide for this again I'll make another post about it.



Hope it helps!
