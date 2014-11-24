---
layout: post
title: Android Ndk Build Error Multiple Targets
description:
date: 2014-05-05 13:57:56
category:
tags: ['android', 'c++', 'ndk']
---

Had this error today:

    * multiple target patterns. Stop.

(Turns out I needed to delete the obj file from the android project)[http://stackoverflow.com/questions/10293659/getting-multiple-target-patterns-stop-error-when-attempting-to-build-for-and]

This post is mainly to help me remember this but hopefully it helps someone else too.