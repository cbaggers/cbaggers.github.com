---
layout: post
title: API Changes
description: 
category:
tags: ['python', 'wordpress']
---

Hey folks, 

Made some changes to keep things a native as possible.



Posts behave as dicts now, to make a post you now just have to write:



<pre class="brush: python;">

post= {"title": "example title",

		"body": "example body text",

		"tags": ["wordpress", "api", "python"],

		"live": True}

blog.append(post)

</pre>



Internally the library uses a dict like object to present posts but that is just so that some data can be retrieved lazily.



I'm close to an beta release now so I'm really going to have to hurry up and get on launchpad!