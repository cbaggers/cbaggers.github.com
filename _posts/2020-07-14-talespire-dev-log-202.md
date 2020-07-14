---
layout: post
title: TaleSpire Dev Log 202
description:
date: 2020-07-14 20:28:46
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Allo folks,

Some good progress today. The new batcher seems to be laying out tiles correctly now, so I can kind of build again. I haven't hooked into the new physics engine so I can only build on the build plane currently.

What was nice was I could do some quick performance tests to see what ballpark we are in. I was able to spawn 10000 tiles in one frame without dropping below 60fps which is very promising. There are things the code doesn't have to deal with yet, such as animations and physics. However I'm feeling confident that we'll be able to get a large speed up in tile spawning over what we have in the beta today.

Here is a very wip, potentially misleading clip :P

<iframe width="560" height="315" src="https://www.youtube.com/embed/tPI9FeThDbc" frameborder="0" allowfullscreen></iframe>

Next, I'll be adding jobs to build the per-zone physics data. It won't be the code that ships, but like this, it will give me some insights into how the final system might behave.

Seeya folks!
