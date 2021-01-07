---
layout: post
title: TaleSpire Dev Log 253
description:
date: 2021-01-07 01:53:21
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Evening all.

It's been a reasonably productive day for me today:

- I fixed two bugs in the light layout code
- Fixed pasting of slabs from the beta [0]
- Fixed a bug in the shader setup code
- Enabled use of the low poly meshes in line-of-sight (LoS) and fog-of-war (FoW)
- Started writing the server-side portion of the text chat

I have not made any UI for chat in TaleSpire yet. My focus has been on being able to send messages to different groups of players. Currently supported are:

- all players in the campaign
- all players in the board
- specific player/s
- all gms

Next in chat, I need to look at attachments. These will let you send some information along with the text. Currently, planned attachment kinds are:

- a specific position in the board
- a particular creature in the board
- a dice roll

We will be expanding these, but they feel like a nice place to start and should help with the flow of play in play sessions.

We'll show you more of the UI as it happens.

One cute thing I added for debugging is the ability to render using the low-poly occluder meshes instead of the high-poly ones. Here is an example of that in action. You can see that, in this example, the decimator has removed too much detail from the tiles. This is handy for debugging issues in FoW and LoS.

![occluder bug example](/assets/videos/renderOccluders.gif)

Alright, that's all for today,

Seeya


[0] The new format is a little different, and I'll publish it a bit closer to shipping this branch
