---
layout: post
title: Woops, wrong about the samplers
description:
date: 2016-04-02 15:58:21
category:
tags: ['lisp', 'cepl']
---

hehe turns out in my refactoring hubris I had forgotten how my samplers worked. THey are actually used like this:

	(with-sampling ((*tex* *sam*))
      (map-g #'prog-1 *stream* :tex *tex*))

Which is more reasonable :) I still want to change it though
{{ page.theme.name }}
