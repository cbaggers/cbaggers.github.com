---
layout: post
title: TaleSpire Dev Log 279
description:
date: 2021-06-10 14:30:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

While we work on the 'props as creatures' feature we talked about the other day, I've switched tasks to look at some bugs hitting folks in the community. To that end, the next patch will have the following:

- A fix for a memory leak in copy/paste
- Cases where exceptions should have caused TaleSpire to leave the board but didn't.
- Fixes in the board loading code to make it more robust
- A fix to the campaign upgrader, which was having errors

The campaign upgrade issue was a regression caused by me when I changed how some internal data was structured. I should have double-checked the upgrader before pushing.

I also am tracking another regression that is causing issues with picking tiles and props when in certain positions. I've got a solid idea of where these issues lie, so I'm hopeful that I can get this fixed today.

Once that is done, I'll push out a patch.

Hope you are all doing well,

Peace.
