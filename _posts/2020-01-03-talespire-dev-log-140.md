---
layout: post
title: TaleSpire Dev Log 140
description:
date: 2020-01-03 14:44:39
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I've continued on server work:

I rewrote the sign-in code to promote the connection to a secure websocket if sign-in succeeds. 

I've started wiring up erlbus so that the client will subscribe to the campaign and board and receive messages broadcast by other clients or the server.

One interesting note with that is that when messing with erlbus from the repl you might run into a badrpc error when trying out `ebus:messages`. This was happening as `ebus:messages` uses `rpc:multicall` behind the scenes and it wasn't finding the function on one of the nodes. The node with the issue was the one I was connecting my remote shell from as, naturally, that doesn't have the app compiled. The quick fix is just to use `-hidden` in your erl arguments as multicall contacts `[node() | nodes()]` by default and when you are hidden you wont show up in `nodes()` unless that behavior is specifically requested.

That's the lot for today (other than the usual misc bug fixes). I'm flying back to Norway on Monday so most of the day will be a write-off due to travel. I may get a little done on Sunday but tomorrow I'll be visiting some friends I haven't seen in a few years (which is gonna be great!).

Ciao!
