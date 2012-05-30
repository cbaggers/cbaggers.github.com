---
layout: post
title: Wordpress and Python
description: 
category:
tags: ['python', 'wordpress']
---

I am starting to sketch out a python library for wordpress that makes wordpress appear to python as native lists and dicts. Maybe it's a phase I'm going through but so many things can be expressed in terms of either lists or dicts. 
Wordpress could have quite a nice fit for this.
The posts are dicts is the format {"title":"", "body":"", "tags":[]} etc. The tags are a dict with each tag being a key and the value being a list of posts. I would have to make things as lazy as possible so as not to try to pull down your entire archive when you log in...but it could be quite handy.
I'll let you know if/when I have a tinker with this.