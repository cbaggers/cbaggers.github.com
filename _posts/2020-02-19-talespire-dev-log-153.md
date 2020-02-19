---
layout: post
title: TaleSpire Dev Log 153
description:
date: 2020-02-18 17:51:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Since the end of last week, my focus has been looking into unknowns on the code side of the project. To that end, I've jumped over to the server-side of things.

The first task change was that, in the alpha, all the communication with our server (which only handle persistence) was over https. This means that polling was used in some places, and overall it was too slow. The simplest change to be able to push messages, given our current stack, was to move much of this communication to websockets. Once picking a [c# library](https://github.com/sta/websocket-sharp) and getting basic communication set up, I needed to do something about all the request based code that was previously using https.

In order to make the changes on the c# side more iterative, I decided to write a simple request system that sent its messages over the websocket connection. When making the alpha, I had made a code generator in erlang that took a spec (written as an erlang data-structure) and generated erlang and c# code entry-points. This has made keeping both sides in line trivial and, now, it made it easy to update the code as I just had to update the code generator.

With that done, I spent some time fixing bugs from the refactor until I could log in, create boards, etc again. I then switched tasks again to certificates. We had used amazon's certificate authority for production previously as they were free for AWS infrastructure, and our servers were behind one of amazon's load-balancers. Now that we are using websockets, we would have to switch to their 'network load balancer', which I don't have any experience with yet. For now, we've just picked up a good ol' fashioned wildcard cert and will work with that. We spread load across servers in the new approach anyway, so using the load-balancer for that is no longer an issue (though there are other benefits). We can look back into their TCP level 'network load balancer' at a later date.
As an aside, we have previously used letsencrypt in staging, and while it did work well enough, I would need to make some changes to use it in production. This ends up being one of those cases where it's cheaper to buy something simple that works than to use the free thing.

Also, on the server side, I've been reviewing our AMIs and docker images. One question I'm trying to get resolved right now can be found [over here](https://stackoverflow.com/questions/60296445/where-does-a-container-get-its-limits-no-file-and-similar-from-when-not-set-b). It's a simple thing, but I have failed to find an official text stating the answer. If you have some expertise, it would be super useful to hear.

Yesterday I started fixing a bug, which caused us to assign identical script ids to tiles in different zones of the board when a drag spanned multiple zones. It's simple enough but still requires some focus not to introduce a dumber bug in the process :p I also fixed a small bug that, for tiles only 0.5units, we spawned them too close together.

Today I'll be finishing off the script Id issue and then possibly looking at rewriting the unique creature support.

Ciao!
