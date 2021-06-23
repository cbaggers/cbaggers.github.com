
---
layout: post
title: TaleSpire Dev Log 281
description:
date: 2021-06-23 10:09:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks.

At the time of writing, I'm about to push an update. In the changelog, there is a line that looks like this:

> This patch also contains a change that will hopefully improve performance for machines with high numbers of cores.

I wanted to talk about this change as I think it will be interesting (and maybe counterintuitive) to some non-programmers out there.

Here is the change: I've told TaleSpire to use fewer cores.

!(dun Dun DUUUUUUUUn)[/assets/videos/gopher.gif]

Let's set things up so I can explain why.

So TaleSpire uses a job-system. A job-system (in this case) is something that you give a chunk of code to run, and it runs it on one of your machine's cores[0].

This is good as we can queue up lots of work and let the job-system work out where and when to run that code. This often gives us nice performance improvements as we are using all the cores on our machine.

As a programmer, your job then involves writing code in such a way that it can be run concurrently. For example, let's say you work in catering and you need to make 20 of a certain kind of sandwich, and you have 5 workers. In this case, the 'job' is making one sandwich, and with a bit of orchestration, we can have all 5 workers making sandwiches concurrently.

Recently I upgraded my PC to keep up with how TaleSpire is growing[1]. I was very lucky to get a Threadripper CPU with 24 cores[2] which you would think would make TaleSpire much faster, but in some places, it didn't. In this next image, you can see that running the physics got slower.

!(overhead)[/assets/images/overhead.png]

Weird right?! The version running on more cores took over three times longer.

Why? The overhead of orchestration.

Let's imagine our sandwich situation again, but let's say we broke down the steps of making a sandwich even further. So one job is to fetch the bread, another job is to butter the bread, another job is to bring the filling, etc. Now, imagine we still need to make 20 sandwiches, but we have 100 workers. As you can imagine, things get messy.

Not only does instructing each worker take time, but the workers also share access to limited resources (like access to the one fridge). Furthermore, the workers need to sync up to actually assemble the sandwich, and this coordination also takes time.

This is a long way of saying that, at some point, the overheads of managing workers can outweigh the benefits of having more. And so we come right back round to the physics situation.

I like that we can spread the physics work over multiple cores, but I need to limit how many so we don't get lost in the overhead. As I don't yet know the right way to restrict the worker count just for the physics, I've **temporarily** lowered the max worker count.

This will be increased again when I understand how to control this stuff for each system I care about.

That's all I've got for this post. Thanks for stopping by, and I hope you have a great day.

[0] This is an approximate description. We could say it runs it on one of the worker threads. But given that they are locked with affinity, and we don't want to have to explain software threads, this will do for now.
[1] I run dev builds of TaleSpire, which gives more information to me, but the cost is that these builds run much slower.
[2] With hyperthreading, that's 48 logical processors
