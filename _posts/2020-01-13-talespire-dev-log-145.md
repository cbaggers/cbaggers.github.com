---
layout: post
title: TaleSpire Dev Log 145
description:
date: 2020-01-13 00:18:03
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I didn't write a log yesterday as I was really struggling with undo/redo histories, and it was just too painful!

In short, I was fighting with the behavior and then making sure the board was behaving the same as the simplified model we use for tests. I finally got that behaving this morning and so have turned my attention to copy/paste.

The short version is that I think I almost have Copy done, and so tomorrow morning, I'll wrap that up and start implementing Paste. The main thing that takes time has been just working out how best to make it fast and then triple checking that it will behave correctly with our deterministic board update system.

The nice thing with how copy works is that you only need to send a fixed number of bytes of information across the wire for any size selection. As operations are applied in order, we send the selection and the point in the history the selection was made, and this results in the same tiles being selected on every client.

An annoying case, however, is pasting slabs of tiles from text strings (like you can find at [TalesBazaar](https://talesbazaar.com/)). It's a really powerful way to share content, but all the tile data does have to be sent to each client. It'll be alright though, it's just a case of making it as pain-free as possible :)

Ok, I'm getting super tired, and I'm not convinced that what I'm writing is coherent, so I'm gonna get some sleep.

Goodnight folks.
