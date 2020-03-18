---
layout: post
title: TaleSpire Dev Log 164
description:
date: 2020-03-18 01:51:20
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Time for another little update on what I'm poking at.

This week I'm focusing on server-side work, and the first thing I needed to look at was the new server setup. Previously the servers themselves were in a private subnet with the load-balancer being the only thing that was public-facing. With the move the websockets, I'm no longer using Amazon's load-balancer, and so now the servers are public-facing again.

I needed a way for messages to be delivered between servers as players might be connected to separate instances in the same AZ. This should be made easy by [erlang's distribution system](https://erlang.org/doc/reference_manual/distributed.html). However, we also want to make sure that we don't accidentally connect servers, which shouldn't be connected.

The first answer to this is to use erlang's "cookie" setting. The cookie simply stops and two erlang nodes connecting if their cookies don't match. Knowing that we set the cookie in the settings file before we push the build to aws. We also generate a random name for the node at that point. It's a bit clumsy but will do the job for now.

One wrinkle is that our REPL is just another Erlang node, and so it's cookie must match the server's. To enable this, I wrote some elisp to look at the setting file of the node we are connecting to and extract the details we needed to initiate the connection. With this done, we keep the ability to connect and quickly query details or push changes if required.

I've also spent a little time fixing bugs on the client but nothing of significant interest right now.

The next step is to set up the production database, get two production servers set up, and check that the messages being sent across [erlbus](https://github.com/cabol/erlbus) are being delivered to both nodes. I'll also need to set up something simple for node discovery. Hopefully, that won't take long.

Just a small Corona related update to wrap up. I'm definitely starting to come down with something, but I have no idea how much it's going to interfere with development yet. Hopefully, it won't, but I'll keep you posted.

I hope you are doing well,

Back soon with more
