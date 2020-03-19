---
layout: post
title: TaleSpire Dev Log 165
description:
date: 2020-03-19 10:40:57
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good morning folks,

Yesterday went pretty well. I was able to deploy the production DB, two servers, and show that they are communicating as expected.

One thing that took me a little bit to grok was how the two Erlang nodes were meant to find each other given only the node's name. [This article](https://hackernoon.com/running-distributed-erlang-elixir-applications-on-docker-b211d95affbe) talks about how epmdless is great, but how you also need something extra when the docker containers are on separate machines. We use [epmdless_dist](https://github.com/scrapinghub/epmdless_dist) like the article shows, but the example has this code:

```
epmdless_dist:add_node(‘app1@host1.com’, 17012). % Add node into epmdless database
```

And I didn't understand how erlang would know the address of my other server unless it was assuming that the `@host1.com` portion was the name for the server as well as the container. It turns out this confusion was valid, and there are a few variants of the `add_node` function. The one I needed was `add_node/3`. Here is the spec:

```
-spec add_node(Node, Host, Port) -> ok when
      Node :: atom(),
      Host :: inet:hostname() | inet:ip_address(),
      Port :: inet:port_number()
```

After calling that, I could `net_adm:ping(‘app1@host1.com')` from the second server, and it correctly resolved the Erlang node. The two were then correctly connected, and [erlbus](https://github.com/cabol/erlbus) messages started working across them transparently. Very cool :)

My job for today is to get 'unique creature' support back into TaleSpire. It got ripped out during the big engine refactoring as the code was so entwined with how we used to handle creatures. This time it should end up much more straightforward.

Until the next one,
Ciao
