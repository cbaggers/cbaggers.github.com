---
layout: post
title: CEPL meets SDL2
description:
date: 2013-10-26 12:58:00
category:
tags: ['lisp', 'cepl']
---

Hey folks,
Another cool week or so as cepl appeared in the lisp subreddit and the videos got 980 views in one day, which for me was pretty exciting!

One of the things that came up very quickly was some issues on osx which I originally thought was down to immutable textures. So I added mutable texture storage and the problem still persisted. 

Thanks to the fantastic people reporting bugs and in the irc channels I was informed that on some systems when you create a opengl context it picks a lower profile unless you specifically request a higher one. These request functions are only available in sdl2 so I have very hastily ported to sdl2.

This is obviously still a project in the alpha stage of development (is there an earlier term as I'm still working out what this project will be?) so there are plenty of bugs. If you do find them and have a chance please email me or make a bug report in github. This is a part time project but I want to get it in a state suitable for real experimentation with graphics so I will get to the bugs, just slowly sometimes :D

Righto, thanks for stopping by.

Oh almost forgot I have added a very crude compile chaining system so now when you compile a shader stage or sfun it will compile any stage or pipeline that uses it. Woo! I have doen this is a really hacky way but it proves the behaviour is nice so I will build from there.