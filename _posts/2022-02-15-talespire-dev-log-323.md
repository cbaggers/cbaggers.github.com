---
layout: post
title: TaleSpire Dev Log 323
description:
date: 2022-02-15 13:00:39
category:
tags: ['Bouncyrock', 'TaleSpire']
---

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

Hey everyone,

Last week was quite a doozy. I didn't mention it at the time, but I was in the process of trying (and failing) to buy a house, so lots of extra distractions. 

#### Photon issues

A few of you also got bitten by a Photon issue earlier in the week so let's talk about that first.

We'll kick off with a bit of background. Photon is a service we use for handling real-time network communication in TaleSpire. By adding their SDK to your game (which we will call "the client") can create "rooms." Other clients can then join the same room. When multiple clients are in the same room, they can send data to each other. The data can be transmitted reliably (via RPC) or unreliably[0]. Photon also has a lot of functionality to help with managing networked GameObjects to trivially keep them in sync. When a room is closed, no information persists.

Photon operates servers worldwide so that the time to send messages between clients is minimized. Running such a service is a huge undertaking, and even the [pros struggle with it](https://www.youtube.com/playlist?list=PLzVi6Kh_HMIWKS1aXOV4XobuymoF0P-_S). Without something like Photon, TaleSpire wouldn't have happened as we would not have been able to make something similar in the time our funds allowed.

We do have our own backend and data message service, but it is not designed to handle real-time traffic. Instead, we focus on what you can't do with Photon: communication between rooms and persisting data. 

Right, that's a lot of background, but hopefully, Ι got across that this service is valuable and is not trivially replaced[1]. But when Photon has issues, we feel it. 

That was what happened earlier this week. For about an hour, some people whose campaigns are hosted in the EU were having issues connecting. We were confused as Photon's [status page](https://www.photonengine.com/status) was not showing a problem. We have confirmed with other developers using Photon that the event occurred and are starting a conversation with Photon's support to see what we can learn. 

It's somewhat disconcerting to see such issues after Photon being solid for so long, but we aren't keen to throw out the baby with the bathwater, so we take these things slowly.

#### Voice chat

As we are talking about network stuff, let's ramble about voice chat. 

Voice chat is an interesting challenge. In principle, we can *just* find a suitable[3] webrtc client and hook it up. Of course, hooking it up is where it gets fun.

WebRTC is principally p2p. Setting up the connection requires exchanging some messages, so there does need to be a way to do that. Of course, we have our backend, so we can use that to handle the setup (of course, then it's not *pure* p2p, but the call will be). 

Next, you have [NAT travelsal](https://en.wikipedia.org/wiki/NAT_traversal). Basically, it is not always possible, so sometimes (let's say 15% of the time), you can't have a p2p connection. What to do? Well, WebRTC allows for relays that the problematic client can connect to in those cases. Of course, these relays have to be hosted somewhere (not p2p), and that costs money. You can certainly find people who host free ones (google does this), but as you can imagine, you need to trust them.

And we are far from done. Let's talk traffic. We will assume that we have five clients connected, and that all managed to connect peer-to-peer. That means that each person has to send their voice and video data to all four other machines. More players? More overhead. And all of those clients also have to receive data from every other client too.

That traffic adds up real fast! So being ingenious, you decide to change the approach. How about we have some central machine that all the clients send their voice and video to. That machine then has a stream for each player that you can connect to. So if there are ten clients, each client is uploading one stream and receiving nine. That is a significant improvement over the nine outbound and nine inbound approach when communicating p2p. That does mean we just lost p2p. Worse, we now need servers around the world for relaying this information. 

Ten media streams per client are still quite a lot. Instead, you could have the previously proposed central server take all the media streams, combine them into a single stream, and make that available to the clients. Now each client only needs two streams, one inbound and one outbound. However, now the servers behind the scenes aren't just relaying data; they are re-encoding audio and video, which is much more resource-intensive. So the cost of your backend just went up a lot!

The above is a clumsy, simplified view of some of the problems in adding voice and video chat. However, these are precisely the kinds of tradeoffs discord, zoom, and others make. It is not viable to hand-wave these problems away if you are selling a product. Your customers will care.

And so we go back to TaleSpire. We need voice and video chat. And we are going to use WebRTC to provide p2p calls. However, this will take time. Unity also has a service for voice chat called [Vivox](https://unity.com/products/vivox). I will look into this as a means to bring voice chat to TaleSpire faster without us having to provide extra infrastructure.

I don't yet know if it will be a good fit, but it is definitely worth a look. It could even be that we use it as the default for those who don't want to trade off the overhead for the additional security of p2p.

#### HeroForge integration

In the last dev-log, I talked about how Ι had the core HeroForge functionality working, but also that I had written some of it quickly rather than robustly so we could start testing other parts of the system.

My job since then has been rewriting to make things solid. The short version of the problem is that we have code that:

- queries which miniatures we are meant to have
- downloads miniatures
- converts miniatures
- caches miniature data
- deletes miniatures that are no longer needed from the cache

All of these processes are asynchronous, so things can get fun when operations overlap.

For example, let's say that someone links a miniature to their campaign, and then when trying to link a different mini, accidentally unlinks the first one, realizes their mistake, and links it again. Let us also say they have a good internet connection and that by the time they correct their error, the miniature data has been downloaded and that the conversion has begun (this is a realistic situation).

Now! They unlinked, so we should delete the mini from the cache, but it's not there yet as it's still converting. So we can try cancel the conversion process, but it's not guaranteed that the message will be processed before the other thread has finished. Instead, let us assume we mark somewhere to delete the mini when it hits the cache. 

We can also recall they have now relinked while this was happening. So we do need the mini. So if we delete it, we need to download it and convert it again. And if we do try and download it, we need to make sure we download it to a different folder as deleting the local data takes time too.

I'm belaboring the point a bit, but Ι hope it's clear that by assuming different timings of user input, downloads, and processing, we can end up with various different situations to handle. [4]

After a good few flow diagrams and pacing around my room, we have something that behaves reliably. Ι can't think of a way to describe it without making this post much longer. The short version is that there is a strict hierarchy between the HeroForge account handling code, the cache manager, and the download manager. The cache also needs to handle the possibility of multiple versions of the same mini being in flight at once (rare case but needs handling).

#### Caches and running multiple copies of TaleSpire

Now I hear you calling out, "caching is just so fun, tell us more!" and to that Ι say, get help. But also… sure.

Some of you out there run multiple copies of TaleSpire on the same machine. It's a relatively uncommon case, but it can be helpful for streamers or TV play. 

The problem is that TaleSpire caches files (parts of boards, HeroForge minis, etc.) in a specific directory. If we have two copies of TaleSpire running, both are trying to write to those folders simultaneously. Worse, they might try and delete the same stuff too.

So far, we have ignored the problem as conflicts were rare, and the use case was niche. However, these problems will become mainstream with the upcoming package manager.

To handle this, we have to detect whether a cache is in use and, in such a case, make a new cache (or reuse an old one). And we have to do this in a cross-platform fashion.

This is really a variant of the "detect whether my program is already running problem," so let's look into it.

We need some construct to signal that a resource is taken across processes. Another wrinkle in the problem is that this not only needs to be cross-platform but ideally needs to be supported in MS and Mono's DotNet implementations.

If this were an interthread problem, we would use a mutex, but your traditional mutex doesn't communicate across processes. Windows does have the [named mutex](https://docs.microsoft.com/en-us/windows/win32/sync/using-named-objects), which might work, but isn't supported on 'Nix[5]. Linux does support named semaphores, so there might be a route there. 

Windows also has file locking, and while Unix OS' don't tend to (afaik), they usually have advisory locking. This soft construct doesn't prohibit overwriting but instead indicates that you shouldn't. As we can query this information for a given file, we can use this as our communication construct.[6]

With this tool in hand, Ι made a system that does the following on TaleSpire start:

- tries to acquire the primary cache
- if it succeeds, it also tries to clean up any old caches which are not in use[7]
- if it fails, it looks to see if there is an older temporary cache it can claim else, it makes a new one

Thanks to the locking, we can trust that multiple clients can't claim the same cache simultaneously.


#### Wrap up

That's most of the news from me Ι think. Naturally, everyone else in the team has plenty going on too, but this log is long enough :)

Hope you are all doing well,

Until next time!




[0] If you are unused to network programming, you might be wondering why unreliable messages would be desirable. The reason is that for many cases, the latest values are all you care about, and older values are nearly useless. Player position is often a good example of this. 

Making messages reliable takes a lot of work and sleight of hand performed either by your program or the network protocol. This adds a lot of overhead that you simply can't afford in most cases in games.

This is a big subject, and I'm butchering it in this little comment, but hopefully, this helps a little.

[1] peer-to-peer is its own challenge, and it has plenty that makes it significantly trickier than the client-server approach. We will tackle it in the future, but it's not something I expect to go smoothly :D

[2] And still isn't, which is annoying

[3] It's got to work and not be a resource hog.

[4] I've also left out a bunch of fun details such as file locking and OS/filesystem-specific considerations

[5] We are using Proton on Linux for the foreseeable future, so that case should be easy, but we still need a native client for Mac

[6] This approach is hairy but commonly used.

[7] It leaves a few around as the small cost in disk space is worth it to allow multiple clients to work faster. There would only be multiple caches if there was a time when multiple copies of TS were open at the same time.
