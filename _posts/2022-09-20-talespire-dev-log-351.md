---
layout: post
title: TaleSpire Dev Log 351
description:
date: 2022-09-19 21:03:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks,

With AOEs shipped, I've returned to the lasso tool for the group-movement feature.

[Previously](https://bouncyrock.com/news/articles/talespire-dev-log-350) I worked on decomposing the lasso path into [simple polygons](https://en.wikipedia.org/wiki/Simple_polygon)[0]. This was required for the next step, which is using a technique called "ear clipping" to make a mesh of the selected area.

If you are interested in how it works, then [this video](https://www.youtube.com/watch?v=QAdfkylpYwc) is an excellent place to start. It's very simple, but I still managed to make it take a day :P

I made a separate project for the experiment to speed up compile times. This video shows the test code in action:

![lasso meshing](/assets/videos/lassoMeshing0.gif)

An additional requirement for ear-clipping to work is that no three consecutive vertices are colinear. It's a bit faint, but this clip shows that when the verts are in a line, the result is one triangle rather than two.

![colinear vertex removal](/assets/videos/lassoMeshing1.gif)

Now I have this working, I will clean up the code, so it is ready to combine with the previous lasso work. I've been careful to write the code in a fashion that lends itself to porting to [Burst](https://docs.unity3d.com/Packages/com.unity.burst@0.2/manual/index.html). As a single lasso path can result in multiple polygons, I will use Unity's job system to mesh them concurrently.

That's the lot for today. It's been wonderful seeing the response to AOEs. Hopefully, it won't be too long until I can get this into Beta.

Ciao.


[0] I am not yet satisfied with my approach, but it's good enough to allow me to write the rest of the lasso implementation.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
