---
layout: post
title: TaleSpire Dev Log 331
description:
date: 2022-03-17 10:10:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks,

Let's start with HeroForge news. We've been working closely, iterating, and finding shortcomings in the format for some time now. As of yesterday, we have finalized the format and conversion process and are looking to the next milestone. We obviously wouldn't announce any dates for a potential release without HeroForge's involvement, as there are naturally a lot of overlapping concerns. Not just technically, but from the PR side too. 
So today, all I'll say is: Things are looking good!

I've also continued work on the backend. I have been working heavily using [terraform](https://www.terraform.io/) to get all our backend infrastructure defined as code. In the future, we want to be able to spin up services[0] in whole new regions trivially, and this should let us do that.

Currently, I'm experimenting with an unfinished setup in the EU. This never went live but is a reasonably clean setup and makes for a good testbed. 

> Please note: I'm not experimenting with live infrastructure. None of this is currently used by TaleSpire, and so any bugs you hit are not related to this :)

As part of this process, I'm working with another tool from HashiCorp, [packer](https://www.packer.io/). Packer is letting me define our docker containers and AMIs from common scripts. 

Previously we have run docker containers on our EC2 servers. However, the separation from the host has caused complications in the past, and, personally, I prefer just using docker to make development cleaner. My goal with packer is to make our AMIs and containers as close to functionally identical as is reasonable. This will remove a small overhead[1], simplify some edge cases, and make building AMIs waaaay easier[2].

Today I'll continue with that backend work.

Seeya folks!


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] Using this term generally, we don't use a microservice architecture
[1] Which won't matter now, but that I believe certainly would if we start taking on more real-time traffic
[2] I could probably argue that it will make the dev environment closer to the running environment, but that's another ramble.
