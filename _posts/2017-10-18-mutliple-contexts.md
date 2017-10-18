---
layout: post
title: Mutliple Contexts
description:
date: 2017-10-18 11:41:55
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

This last weekend I put a little time into multi-context support in CEPL.

CEPL has it's own context (`cepl-context`) class that holds both the gl-context handle[0] and also state that is cached to improve performance. `cepl-context`s are passed implicitly[1] down the stack and are tied to a single thread.

Most of the work was just finding simple errors in my code an shoring them up, but I did find one tricky case and that was in pipelines. So a pipeline is usually defined in a top level declaration like so:

```
(defpipeline my-pipeline ()
  :vertex some-gpu-function
  :fragment some-other-gpu-function)
```

And this generates all the bootstrapping to compile the gpu functions, get the GL program-id, etc. However that program-id is a GL resource and belongs to a single GL context. As it is right now it'll be the context that calls this pipeline first..ew.

So how to tackle this? We could create one program-id per context, however this means either looking up the program-id based on the context per call in a pipeline local cache..or looking up the program-id in a context local cache based on the pipeline. Neither is great, as extra lookups per call are something we should be avoiding.

Another option is to have shared GL contexts. This is nice anyway as it means we can share textures/buffers/etc between threads which I think is a nice default behavior. However even with this solution there are still issues with pipelines.

The state of a gl program object is naturally shared between the two threads too, that state includes which uniforms are bound, so if two threads try to use the same pipeline with different uniforms then we are in a fun data-racey land again.

This seem to lead back to the 'gl program per gl context' thing again. I'll ponder this some more but I think it's the only real option.

Happy to hear suggestions too,

I think that's all for now

Peace



[0] in the future I expect I will allow multiple GL contexts per CEPL context
[1] or explicitly if you prefer
