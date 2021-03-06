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

I also spent some quality time getting to know [gproc](https://github.com/uwiger/gproc), [erlbus](https://github.com/cabol/erlbus), websockets a little better and I'm much happier now with where I'd use them in my projects. I've not got much more concrete to say on these but having a good understanding of common tools is helping with the design of the next iteration of the backend (more news on this in future too).

Lastly as fun bit (for me). For a time I was really enjoying that I was able to use docker to test the TaleSpire backend locally. I had webservers, db servers etc on their own little network and I could get a reasonable idea of how things should run. However at one point we started using amazon's S3 for storage and that put a spanner in the works as I couldn't test that locally. I could make a alternative approach for local dev but that's more code to maintain and that could fail in ways that confuse my testing. Luckily there is a project called [minio](https://docs.min.io/) which runs in a docker container and presents the same api as S3. I've already modified the image to include my preferred tweaks and have tested making presigned urls which work **GREAT**. 

So now I get to go remove some 'local dev' hacks and I get a simpler, more realistic, fully local, dev environment. LOVE IT.

Alright, that's all the fun nerdage for now, back to planning :)

p.s. 
One thing I got stuck on initially with epmdless was that I was using `rebar3 shell` in the entrypoint script for my container. This will not work with epmdless as the epmd module vm args would need to be passed to shell and that would then freak out as it wouldnt know where to find the epmdless module. So instead try having the entrypoint script make a debug release of your erlang app and then start that in foreground mode. Then your `config/vm.args` will be used and everything will work fine. 

Also, remember to expose the remsh port for epmdless so you can use the remote shell. I used this for my erlang repl settings in emacs. Once you have the official examples running most of this should be fairly self explanatory.

```
(setq inferior-erlang-machine-options
      '("-env" "EPMDLESS_DIST_PORT" "18999"
        "-env" "EPMDLESS_REMSH_PORT" "18000"
        "-name" "emacs@vanguard0.com"
        "-setcookie" "cookie"
        "-remsh" "myapp@myapp.com"
        "-proto_dist" "epmdless_proto"
        "-epmd_module" "epmdless_client"
        "-pa" "/app/_build/default/lib/epmdless/ebin/"))
```
