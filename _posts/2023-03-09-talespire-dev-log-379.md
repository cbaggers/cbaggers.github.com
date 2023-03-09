---
layout: post
title: TaleSpire Dev Log 379
description:
date: 2023-03-08 09:56:07
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

I'm back again with an update on what I've been touching this last week.

### Symbiotes

Chairmander and I kicked off with a long meeting to design the manifest. We took some inspiration from other projects but ensured the file grew naturally in step with what Symbiote features are being used. This amounted to talking through user stories, though we didn't think explicitly about that then.

After the designing, it was time to work!

I knew quickly that if a Symbiote failed to load, I wanted some UI in the panel to show that. I've previously mentioned that I wanted to support different kinds of Symbiote (as in, not just web-based ones), and this felt like an excellent place to use another type.

Knowing what I wanted and getting it were two different beasts, though. I spent many hours going in circles until I found an approach I liked. The details of the solution aren't particularly exciting. Still, the upshot is that we can now support not just different user-made symbiotes, but we can technically support hot-loading the infrastructure for entirely different kinds of Symbiote. This won't matter for a while, but I'm happy to have that worked out.

We are still improving the API Symbiotes have access to. As of today, we have calls for the following:

- fetching the initiative list and the currently active member
- sending chat messages both as players and as creatures
- putting dice into the tray
- fetching info about the currently active ruler
- fetching a slab string based on the current selection
- putting a slab into the hand (switching into build mode if necessary)
- unpacking slab strings to binary representation
- packing binary slab representation to slab string
- getting the data size of a given slab string
- sending a network message to the same Symbiote running on other clients
- fetching the ids of any parties
- fetching the ids of creatures in a given party
- fetching info about the active campaign
- fetching the board list for the active campaign
- fetching a list of the bookmarks in the active campaign
- fetching the distances units for the active campaign
- fetching the names of creature stats used in the active campaign
- fetching a list of the players in the campaign
- fetching a list of the players in the board
- fetching the id of your player
- checking if a given client-id is yours
- fetching info about a given player
- fetching a list of the connected clients
- fetching the id of your client
- checking if a given client-id is yours
- fetching info about a given client
- fetching the list of loaded asset-packs
- fetching info about the asset pack (including all tile/prop/creature info)
- fetching the list of unique creatures
- fetching a list of creatures owned by a given player
- fetching a list of currently selected creatures

We also let you subscribe to events such as:

- unique creature created/deleted
- dice roll results
- slab copy events
- asset pack loaded/unload events
- client join/left board events
- player joined/left campaign events
- player permissions changed events
- initiative list changed events


Next up, I'll be experimenting with exposing a picker tool that sends the kind and id of the picked *thing* to the Symbiote.

We also will be handling documentation generation soon. With that and another big pass on the API, we will be nearing something we can ship.[0]

### Community mod repository

I just put this section here to reassure you that slab sharing and mod.io integration have not been forgotten. They have had to wait a bit as pushed to find out what the Symbiote feature needed to be.

### Performance

Ree is working on a cool feature that requires scanning over any tiles or props in a given area. We have some code for this that is used by the hide plane. However, it was not currently Burst compiled due to limitations on how generics work in HPC#.

I changed the logic so that a caller-defined visitor was passed chunks of data. The caller could then return jobs that processed these chunks with the help of some methods on the chunk. 

The result was a significant performance improvement to the hide plane and code that could be used in more cases.

### GameObject Alternative

The perf work above got me very motivated for more. So I sat with a pad of paper and started trying to design a jobified alternative to GameObjects we could use in some places.

I'm not looking to cover all the cases GameObjects do, but I want something that is more familiar than ECS (and more compatible with our setup) while being easy to integrate with jobs.

Not much to report here yet, but you'll be hearing more about this in the future.

### Physics

Next up, I worked on a bug in our physics library helpers which resulted in sphere-overlap checks not working.

My reason for the bug was that, at the time, the library we wrapped had no documentation, and the name of the field I was incorrectly using was bafflingly named[1]. That's my excuse, and I'm sticking to it :P

### macOS News

Some good news on the macOS front. The bug that has been blocking us has been fixed in Unity 2023.2 (which we don't use) and is in review to be released in 2021.3, which is one we can upgrade to[2]. 

This is exciting! When that lands, we can finally get a good look at what we have and work out how to get the alpha of macOS support into your hands.

### Backend

Work progresses rapidly on the backend. I was in some discussions about how we will be handling certificates and looked into our options. Nothing interesting to report, but it took some time, so it gets a spot here!

### Wrapping up

More than ever, my dev-logs fail to capture the breadth of what is going on behind the scenes. Seeing so much moving from backend code stuff to community outreach is a joy.

Until next time!

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*



[0] You may have noticed that no calls change the state of the game directly or require server requests. This is very intentional for the first version. We will look into state-modifying functions in future updates to symbiotes (an example of such a function would be setting the initiative list)

[1] The field is called `MaxFraction`. Normally it holds a fraction of another distance. However, in some cases, this same field holds the distance directly and not a fraction. Very annoying.

[2] Those not in software development might wonder why we can't just upgrade to 2023.2. The answer is that changes between major revisions can be substantial, so while technically we *could* switch, it means putting a lot of time into repairing systems broken by those changes. Also, software always has bugs, so we would be trading a known set of bugs for an unknown set, so the time it would take to get back up to speed is uncertain.

It's very likely that we will make that jump eventually, but we will let you know when, as that will likely stop us from shipping new features for the duration of that transition.
