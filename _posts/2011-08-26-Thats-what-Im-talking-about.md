---
layout: post
title: Thats what I'm talking about!
description: 
category:
tags: ['python', 'wordpress']
---

Dammit, sometimes it just all seems to fall into place.

Just 2 minutes ago, I realised I hadn't tagged the last 4 posts and so I thought I best test the library:



<pre class="brush: python;">

for index,post in list(enumerate(blog[:4])):

	post["tags"]=["python","wordpress"]

	blog[index]=post

</pre>



That'll do just fine!

Right, next stop.. comments!