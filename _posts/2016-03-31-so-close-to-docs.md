---
layout: post
title: So close to docs!
description:
date: 2016-03-31 11:14:19
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I got so close :D

I've been working on [these docs for CEPL](../cepl/api.html) over the easter break, and it's been great, got so much of the API cleaned up in the process.

When you own an entire codebase, writing documentation is probably the best way to get total overview of your api again. Also writing docs isnt fun, but writing docs for an api you dont like, which you wrote and have full control over is horrifying... so you fix it and then the codebase is better.

So aye, docs are good an I was within 2 macros of finishing the documentation. Specifically I was documenting `defpipeline` which was hard.. which annoyed me as, if I cant explain it, it must be to complicated for users. So I broke out the whiteboard and started picking the things about it that I liked. I came up with:

- automatic uniform arguments (you dont have to add the arguments to the signature as the compiler knows them)
- very fast blending parameters (It does some preprocessing on the params to make sending them to gl fast)
- local fbos (you can define fbos that belong to the pipeline)
- all the with-fbo-bound stuff is written for you.

After a lot of mummbling to myself I found that, with the exception of 'automatic uniform arguments', all of these advantages could be has without needing `defpipeline` to support composing other pipelines.

Local FBOs was basically a closure, but the problem was that the gl context wouldnt be available when the closure vars were initialized. To fix this I will make *all* CEPL gpu types be created in an uninitialized state, capturing their arguments in a callback that will be run as soon as the context is available. As a side effect this should mean that gpu-types can now be used from defvar defparameter etc with no issues, which will be lovely.

with-fbo-bound will still exist but we will add two things. First we will have `map-g-into` which is like `map-g` except it takes a render-target (not an fbo, more on that in a minute). `map-g-into` will just bind the target and do a normal map-g. This make the code's intent clearer that it was in the old `defpipeline` syntax.

fast blending parameters was interesting. To cache the blending details in a foreign array (which allowed fast upload) we need somewhere to keep the cache. You also want to be able to swap out blending params easily on an fbo's attachments. This resulted in the idea of adding `render-target` This is a simple structure that holds an fbo a list of attachments to render into and the blending params for the target and (optionally) attachments. Blending param will be remove from the fbos as it lives on the `render-target` instead. This means you can have multiple `render-target`s that are backed by the same fbo but set to render into different attachments with different blending params. **Much** better and we can keep the speed as we can cache the foreign-array on the `render-target`.


All this stuff got me thinking and I realised that `with-sampler` is terrible. Here is how it's used:

```
(with-sampler sampler-obj
  (map-g #'pipeline stream :tex some-texture))
```

Now with two textures:

```
(with-sampler sampler-obj
  (map-g #'pipeline stream :tex some-texture :tex2 some-other-tex))
```

The sampler applies to both! How are you meant to sample different textures differently? I was so fucking dumb for not seeing this when I made it. Ok so the `with-` thing is a no go. That means we should have samplers that (like render-targets) reference the texture and hold the sampling params. There will be a little trickery behind the scenes so that seperate samplers with the same attributes actually share the `gl sampler object` but itherwise will be fairly straightforward.

The advantage is that suddenly the api very behaviorally consistant.

You make gpu-arrays to hold data and then gpu-streams to pass the data to pipelines
You make textures to hold data and then samplers to pass the data to pipelines
You make fbos to take data and then render-targets to pass data from pipelines

This feels like progress. Time to code it up...and fix all the docs
