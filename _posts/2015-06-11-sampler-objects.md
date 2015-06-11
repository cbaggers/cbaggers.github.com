---
layout: post
title: Sampler Objects
description:
date: 2015-06-11 11:09:24
category:
tags: ['lisp', 'cepl']
---

Textures in opengl have parameters that affect how they are sampled. They dictate what happens when they are magnified or minified, what happens if you sample outside the texture etc.

Sampler objects allow you to override the sampling params in a texture. One sampler object can be used on multiple textures in a single draw call.

As of last night these are now available in cepl if `(>= (version-float *gl-context*) 3.3))`. You can use them like this


    (defvar sampler (make-sampler :wrap #(:repeat :clamp-to-edge :repeat)))

    (with-sampling (tex sampler)
      (map-g #'some-pipeline some-stream :tx tex))


With sampling overrides a texture's sampling params with the sampler object for the duration of the scope. The texture then reverts to it's own sampling parameters.

The functions that are used on samplers to change their parameters also work on textures so:

    (setf (lod-bias some-texture) 0.5)
    (setf (lod-bias some-sampler) 0.5)

are both valid.

Next up are blend-modes. I have had to hold off making new videos for a few days as they are going out of date so fast as we keep getting new features.
