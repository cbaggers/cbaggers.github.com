---
layout: post
title: TaleSpire Dev Log 152
description:
date: 2020-02-18 17:51:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Since the end of last week my focus has been on looking into unknowns on the code side of the project. To that end I've jumped over to the server side of things. 

The first task change was that, in the alpha, all the communication with our server (which only handle persistence) was over https. This means that polling was used in some places and overall it was too slow. The simplest change to be able to push messages, given our current stack, was to move much of this communication to websockets. Once picking a [c# library](https://github.com/sta/websocket-sharp) and getting basic communication set up I needed to do something about all the request based code that was previously using https. 

In order the changes on the c# side more iterative I decided to write a simple request system that sent it's messages over the websocket connection. When making the alpha I had made a code generator in erlang that took a spec (written as a erlang datastructure) and generated erlang and c# code entrypoints. This has made keeping both sides in line trivial and, now, it made it easy to update the code as I just had to update the code-generator.

With that done I spent some time fixing bugs from the refacotr until I login, create boards, etc, again. I then switched task again to certificates. We had uses amazon's certificate authority for production previously as they were free for aws infrastructure and our servers were behind one of amazon's load-balancers. Now that we are using websockets we would have to switch to their 'network load balancer' which I don't have any experience with yet. For now we've just picked up a good ol' fashioned wildcard cert and will work with that. We spread load across servers in the new approach anyway so using the load-balancer for that is no longer an issue (though there are other benefits. We can look back into their tcp level 'network load balancer' at a later date.
As a aside we have previously used letsencrypt in staging and whilst it did work well enough I would need to put some work into changes to use it in production, it's one of those cases where it's cheaper to buy something simple that works than to use the free thing.

Also on the server time I've been reviewing our server images and docker images. One question I'm trying to get resolved right now can be found [over here](). It's a simple thing but I have failed to find an official text stating the answer. If you have some expertise it would be super useful to hear.

Yesterday I started fixing a bug which caused us to assign identical script ids to tiles in different zones of the board when a drag spanned multiple zones. It's simple enough but still requires some focus to note introduce a dumber bug :p I also fixed a small bug that, for tiles only 0.5units, we spawned them too close together.

Today I'll be finishing off the script Id issue and then possibly looking at rewriting the unique creature support.

Ciao!
