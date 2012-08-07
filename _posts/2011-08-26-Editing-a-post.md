---
layout: post
title: Editing a post
description: 
category:
tags: ['python', 'wordpress']
---

Here's what you do to change a post's title with my library:





    # get most recent post

    post = blog[0]



    # edit the posts title

    post.title = "hey a new title!"



    # push post back to blog

    blog[0] = post





Tags are an interesting beasty with wordpress, I can't query for all posts with a tag, but I can get the rss feed for a tag, so I'm hoping I can pull the info from there.

Will keep ye posted