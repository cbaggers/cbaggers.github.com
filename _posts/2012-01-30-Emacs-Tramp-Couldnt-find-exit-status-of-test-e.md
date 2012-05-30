---
layout: post
title: Emacs Tramp "Couldn't find exit status of `test -e"
description: 
category:
tags: ['']
---

<em>"Couldn't find exit status of `test -e /tenet/path/to/thing"</em>

I was greeted with this today as I was using eshell and looking after one of our servers. The quick fix is to run <em>"M-x tramp-cleanup-all-buffers" </em>and all should function normally again.
Hope it helps someone.
Baggers