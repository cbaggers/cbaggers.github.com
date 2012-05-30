---
layout: post
title: Fun with Tags
description: 
category:
tags: ['python', 'wordpress']
---

As I mentioned a few posts back, tags are a bit weird. I had assumed that  you would be able to query for a list of posts belonging to a certain tag but apparently not. 



What I have gone for instead is to scrape the rss feed for the tag, and from that grab the post ids. 

As there could be hundreds the call returns post objects which behave like dicts but can retrieve the data from the server as it is accessed.



So to grab all the posts tagged 'python' and print the  body text of the first post you can do this:

<pre class="brush: python;">

tagged_posts = blog["python"]

print tagged_posts[0]["body"]

</pre>



Simple!