---
layout: post
title: The simple Wordpress Library in practice
description: 
category:
tags: ['python', 'wordpress']
---

Ok so here is how you make a simple post using the new python library I'm writing



<pre class="brush: python;">

import wpress



blogs = wpress.WP("https://cbaggers.wordpress.com/xmlrpc.php",

                   "username", "password")



blog = blogs["Baggers on the Web"]





new_post  = wpress.WPPost()



new_post.title = "First Post Test!"

new_post.body = """Hey all!

This is my first live test of my python library. 

In the next post I will show the code used to post this post.

Wooo!"""

new_post.tags=["wordpress","python"]

new_post.live=True



blog.append(new_post)

</pre>



That's all there is to it! When you connect to wordpress a dict of your blogs is returned, you pull out the one you want and append on a post.



If you want to pull the first 6 posts you do this

<pre class="brush: python;">

import wpress



blogs = wpress.WP("https://cbaggers.wordpress.com/xmlrpc.php",

                   "username", "password")



blog = blogs["Baggers on the Web"]



first_six = blog[:6]

</pre>



I'm crashing out now but I'm going to post on working with xmlrpc in general soon...there have been some interesting gotchas.

Goodnight!