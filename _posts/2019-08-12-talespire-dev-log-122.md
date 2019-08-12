---
layout: post
title: TaleSpire Dev Log 122
description:
date: 2019-08-12 18:35:16
category:
tags: ['TaleSpire', 'Bouncyrock']
---

The end of last week saw me playing with a cool toy so I thought I'd write about it.

First though, I've carried on with my digging into various erlang bits and bobs.

With a bit of prodding I was able to get [epmdless](https://github.com/oltarasenko/epmdless) working with my erlang/docker setup. This is great as [distributed erlang](https://learnyousomeerlang.com/distribunomicon) is one of the powerful features that many erlang libraries lean on and previously this was hampered by docker requiring explicit port mappings. If you are interested in epmdless in conjunction with docker you can check out [this article here](https://www.erlang-solutions.com/blog/running-distributed-erlang-elixir-applications-on-docker.html).

I also spent some quality time getting to know [gproc](https://github.com/uwiger/gproc), [erlbus](https://github.com/cabol/erlbus), websockets a little better and I'm much happier now with where I'd use them in my projects. I've not got much more concrete to say on these but having a good understand of common tools is helping with the design of the next iteration of the backend (more news on this in future too).

Lastly as fun bit (for me). For a time I was really enjoying that I was able to use docker to test the TaleSpire backend locally. I had webservers, db servers etc on their own little network and I could get a reasonable idea of how things should run. However at one point we started using amazon's S3 for storage and that put a spanner in the works as I couldn't test that locally. I could make a alternative approach for local dev but that's more code to maintain and that could fail in ways that confuse my testing. Luckily there is a project called [minio](https://docs.min.io/) which runs in a docker container and presents the same api as S3. I've already modified the image to include my preferred tweaks and have tested making presigned urls which work **GREAT**. 

So now I get to go remove some 'local dev' hacks and I get a simpler, more realistic, fully local, dev environment. LOVE IT.

Alright, that's all the fun nerdage for now, back to planning :)
