---
layout: post
title: TaleSpire Dev Log 291
description:
date: 2021-09-20 11:43:43
category:
tags: ['Bouncyrock', 'TaleSpire']
---

> This dev-log is an update from just one of the developers. It does not aim to include all the other things that are going on.

Hi everyone!

Just a quick warning. This is just a regular dev log, no big news in here.

Things have been good recently, but there are a lot of spinning plates to keep track of.

# server issues

We had a server issue last week that stemmed from a database setting I hadn't realized was enabled. The setting was to apply minor DB patches automatically. I had missed this, and so when the system obediently upgraded the DB, the server lost connection and got a tad confused.

Naturally, we want to schedule these patches explicitly so that the setting has been corrected. 

Backend isn't my strongest suit, so I'm reading and looking into the best way to handle this in the future. One likely portion of this is adding a 'circuit breaker' in front of the DB connection pool. 

# WebRTC

Unity has a rather interesting WebRTC package in the works, so I've been studying this topic again. We know we'd like to have audio and video chat in the future. Ideally, this would be p2p, but (IIRC) you'd be lucky to get NAT traversal to work for more than 85% of people, so this is usually paired with a TURN server to act as a relay for those folks. 

That, of course, means handling the cost of such a server, and more importantly, the fees for the data going through it.

By default, p2p would imply a bidirectional connection between pair of players. So if you have ten people, you are sending the same data 10 times to different places. What many video providers opt for instead is to have servers that mix the feeds for you, so you only have one bidirectional connection. However, naturally, that means you are no longer p2p, and the server's requirements (and thus costs) are MUCH higher than a simple relay server.

Lots to think about here. We'll likely focus on p2p (with TURN fallback) when we start, but we'll see how it evolves.

# Performance

Performance work is never done, and we know that we need to do a lot to try and support both larger maps and lower-end machines. This past week my brain had latched onto this, so I spent a good deal of time reading papers and slides from various games to try and learn more of what contemporary approaches are.

Not much concrete to say about this yet. I know I have a lot to learn :P

# Other

We had an internal play session the other day, which was a lot of fun and resulted in another page of ideas of potential improvements. 

I've been working on HeroForge a bit, too, of course. I'm not satisfied with our backend design, so I need to focus on that more this week. 

Hmm yeah, I think that's most of it. Last week was very research-heavy, so I hope I end up coding more in this one.

The above is, of course, just me. The others have been making all manner of assets for future packs, which is both super exciting to see but also agonizing as it's not time to show them publicly yet.

I hope you are all well. Have a good one folks!
