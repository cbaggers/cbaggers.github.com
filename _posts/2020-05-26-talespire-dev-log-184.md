---
layout: post
title: TaleSpire Dev Log 184
description:
date: 2020-05-26 23:01:53
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again :) 

Today went reasonably well, although it was mostly getting my new laptop set up. 

I finally tried out i3 and decided that, right now, it's not for me, and I've gone back to using stumpwm. I'll probably spend more time getting used to i3's approach to splitting in the future. It's stacks and workspaces did seem very cool although I should experiment with stumpwm's groups feature first.

Getting my server dev environment set up was pretty painless (yay docker), although I currently have an issue where TaleSpire isn't uploading boards to my local minio server. I'm not sure what the issue is, so I'll need to do more testing tomorrow. I've prepared the patches for the DB and erlang server, so I now need to test TS with these changes. With that done, I'll be able to push to production, and then we'll be ready for creature scaling.

Ree's been working hard on fixing all manner of issues relating to creature control at low (and high) fps. Getting that stable is a heck of a project, but it's paying off.

To follow up on the log from earlier today, it does seem that the latest patch does fix the SSE4 issue, which is great! We'll be able to close a bunch of tickets from github real soon :)

Have a good night folks, 
Back with more tomorrow.
