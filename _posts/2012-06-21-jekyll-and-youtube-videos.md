---
layout: post
title: Jekyll and YouTube Videos
description:
category:
tags: ['youtube', 'jekyll']
---

If you are having problems with embedding youtube videos in Jekyll blogs and getting errors like this:
<iframe width="420" height="315" src="" frameborder="0" allowfullscreen></iframe>
Then check out [this link](http://www.whatwherewhy.me/blog/2012/01/17/youtube-iframe-code-gotcha-in-maruku/).

It turns out that the markdown processor doesn't like the 'allowfullscreen' part of the embed code.

Delete it and the video embeds just fine!