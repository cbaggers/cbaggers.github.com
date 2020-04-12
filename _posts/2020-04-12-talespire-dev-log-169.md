---
layout: post
title: TaleSpire Dev Log 169
description:
date: 2020-04-12 13:34:47
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Some people have noticed that TaleSpire will sometimes put a ♥ in the system clipboard and have been wondering why.

Our paste strings are a base64 encoded, compressed chunk of data. In order to use it in-game when need to decode and decompress it, which takes time.

Given that people want to paste a lot, we don't want to have to go through that process every time. So we do the following:

If the system clipboard contains a string that isn't ♥ we try to process it and paste it.

We store the processed version in an internal clipboard in the format we want

Now that we have done that, we want to use the internal version, but there is still a valid paste string in the system clipboard, which means we have to clear it or compare it to the last string we pasted. Those strings can be large, so it would be nice if we could have some shorter indicator.

We don't want to clear the clipboard as we also want to give a nice message when a user tries to paste something that isn't a valid paste string (we need to do this or paste can feel unresponsive, and we'll get bug reports).

So instead, we put a little one character token into the system clipboard. Something that is rarely going to be there accidentally. I just used the Unicode heart as that felt nice at the time.

This is fast to check, and met the other requirements I had, letting us use the internal clipboard in most cases.

Ciao

p.s note the above is important even if you aren't sharing pastes as when you copy we have to populate the clipboard with the string
