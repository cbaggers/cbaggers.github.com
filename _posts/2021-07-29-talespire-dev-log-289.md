---
layout: post
title: TaleSpire Dev Log 289
description:
date: 2021-07-29 02:14:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Well, naturally, the biggest excitement in my day has been seeing the Dimension20 trailer go public, but code is also progressing, so I should talk about that.

Buuuuut I could watch it one more time :D

<iframe width="600" height="450" src="https://www.youtube.com/watch?v=0bJfIZyEP98" frameborder="0">  </iframe>


RIGHT! Now to business.

I started by looking into markers. Oh, actually, one little detail, soon you will be able to give markers names, at which point they are known as bookmarks. I'm gonna use those names below, so I thought I should mention that first.

Currently, markers are pulled when you join a specific board, and we only pull the markers for that board. To support campaign-wide bookmark search, we want to pull all of them when you join the campaign and then keep them up to date. This is similar to what we do for unique creatures, so I started reading that code to see how it worked.

What I found was that the unique creature sync code had some legacy cruft and was pulling far more than it needed to. As I was revisiting this code, it felt like time for a bit of a cleanup, so I got busy doing that.

As I was doing that, it gave me a good opportunity to add the backend data for *links*, which soon will allow you to associate an URL with creatures and markers. So I got stuck in with that too.

Because I was looking at links, it just felt right to think about the upcoming `talespire://goto/` links, which will allow you to add a hyperlink to a web page that will open TaleSpire and take you to a specific marker (switching to the correct campaign and board in the process). After thinking about what the first version should be, I added this into the mix.

So now things are getting exciting. I've got a first iteration of the board-panel made for gms...

![gm-board-panel](/assets/images/bookmarks.png)

> NOTE: I'll be adding bookmark search soon

We can add names to markers to turn them into bookmarks. And you can get a `talespire://goto` link from their right-click menu.

I've got switching between campaigns working, but I need to do some cleanup on the "login screen to campaign" transition before I can wire everything up.

TLDR:

![it's all coming together](/assets/videos/together.gif)

It would be great to ship all this late next week, but I'm not sure if that's overly optimistic. We'll see how the rest of it (and the testing) goes.

Until next time,
Peace.
