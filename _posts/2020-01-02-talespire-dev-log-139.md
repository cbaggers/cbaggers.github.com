---
layout: post
title: TaleSpire Dev Log 139
description:
date: 2020-01-02 17:38:36
category:
tags: ['TaleSpire', 'Bouncyrock']
---

Over the break, I've put down the front-end code to begin getting a handle on the changes that will be made to the backend. Most of this has been focused on reading, here are some of the things I've been dipping into:

### Reading things

- https://erlang.org/doc/man/gen_event.html some of the http handling code got a bit centralized a was slowing me down when making changes. I hadn't used `gen_event`'s before as I found them a bit confusing. They are somewhat different than `gen_server` and `gen_fsm` as you don't implement the manager but instead just the callbacks. It's pretty neat once you grok it, though.
- http://blog.differentpla.net/blog/2014/11/07/erlang-sup-event/ one part of understanding `gen_event` is knowing how to include it in your supervision trees. This covers those bits that mostly seem left out of other tutorials.
- https://github.com/cabol/erlbus/ erlbus is an important piece of the changes I'm making, and so I've been getting familiar with it's approach. It also contains one of the more lucid examples of using websockets in erlang, which was helpful in other tests.
- https://www.phoenixframework.org/blog/the-road-to-2-million-websocket-connections An inspiring read. Little bits and bobs scattered throughout that were useful. Also, it's just nice to see what certain pieces we use can potentially deliver (although we naturally aren't doing this).
- https://github.com/uwiger/gproc/blob/master/doc/erlang07-wiger.pdf This paper describes the evolution and technical realities that led to [gproc](https://github.com/uwiger/gproc) which is something I intend to use in place of the standard global (across multiple erlang nodes) process registry.
- http://erlang.org/doc/apps/stdlib/stdlib.pdf this naturally is WIP. Skimming the standard lib of any language is a great way to find out things you didn't know were there, avoiding redundant work is nearly always a blessing
- https://www.amazon.com/WebRTC-Cookbook-Andrii-Sergiienko/dp/1783284455 I've been skimming bits of this again as it's likely to form part of the first implementation of the p2p voice & video chat
- https://ninenines.eu/docs/ dear god ninenines is the best. Cowboy, gun, ranch. All fantastic quality, super robust and well documented. I have no idea how I'd function without their stuff.

### Making things

As we use [Photon](https://www.photonengine.com/pun) for realtime networking in TaleSpire, we only need to focus on lower frequency events, such as persistence, for now. The alpha used a hacked together REST'ish api, which did the job but has all the expected issues (e.g. having to poll for changes). We are switching to using websockets for the Beta as it's a relatively incremental step, will let us overcome some shortcomings, and still makes sense given the scale we will be at. It, too, will be a target for replacement in the future, but that is a subject for another day.

All of the server api the game uses is described by a data-structure that we then generate erlang & c# code from. I've updated the generator to create websocket handlers rather than the http/s ones. In doing so I've also been undoing some of the code that centralized some of the http management.

The last thing I did was update the code that polls to see which domain it is at before attempting to pick the right certificates and start serving over https. 

The next step will be to rewrite the session handling code as it currently assumes a stateless connection. 

Until next time,

Peace.

