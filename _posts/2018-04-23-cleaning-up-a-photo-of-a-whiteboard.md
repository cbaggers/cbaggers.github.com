---
layout: post
title: Cleaning up a photo of a whiteboard
description:
date: 2018-04-23 15:20:47
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I have small bad handwriting so some of the automatic methods out there really didnt work for me.

This is an excellent start:

https://gist.github.com/adsr303/74b044534914cc9b3345920e8b8234f4

In case that gist is gone:

```
Duplicate the base image layer.
Set the new layer's mode to "Divide".
Apply a Gaussian blur with a radius of about 50 (Filters/Blur/Gaussian Blur...)
```

> Note: Alternatively just perform a 'difference of gaussians' on the image setting the first to radius 70 and the second to radius 1


I used a Gaussian blue with radius 70 in both directions. `Mode` is a dropdown near the top of the layers panel.

Next pick a background color and paint over the world offending background features; stuff like magnets and erasers.

Then flood fill with that color to normalize as much as possible.

Then flood fill with white. This should be pretty effective now.

Open up the hue/saturation (some name like that) tool up the saturation as the colors will be a bit washed out.

Finally open up brightness & contrast and drop the brightness a tiny bit.

That's it. Much nicer to stare at and maintains all the features.

![dif](assets/images/wboard.png)
