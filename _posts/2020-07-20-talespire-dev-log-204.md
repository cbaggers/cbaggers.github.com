---
layout: post
title: TaleSpire Dev Log 204
description:
date: 2020-07-20 00:11:50
category:
tags: ['Bouncyrock', 'TaleSpire']
---

On Friday, I got the code written to extract Unity's animation data and pack it into the AssetBundle file in the way we wanted. This data will be utilized by the upcoming animation system to be used by tiles and props.

Before we get there, and as mentioned in the last dev-log, we need to update the boardAsset format that TaleSpire uses. 

For the format rewrite, the first thing to review was what we were going to store. This quickly led me to look at the scripting system again. I tried to write a high level of some of the stuff I'm looking at, but it gets rather vague, so I'll just talk about something concrete instead.

I am 80% sure I want tile/prop animation to be driven by the scripting system (Rather than tile/prop I'm just gonna say tile from now on for brevity). I already intended to have two kinds of scripts for tiles, state-machine scripts, and realtime-scripts.

The state-machine scripts are driven by user interaction and are synchronized across the network. Realtime scripts run every frame and are unsynchronized. I've imagined the realtime scripts to be like shaders in that they'll run fast, in parallel, and don't hold their own state frame-to-frame. 

I'm looking into being able to specify a realtime script as the behavior for a given state-machine state. So if you are in a given state, the game runs the associated realtime script. The realtime script would set things such as the position/rotation/scale of parts of the tile, parameters of lights, and be able to drive animations.

The last item in that list of examples is the interesting one for today. If I can implement animation via the scripts, it would allow me to flesh out that part of the codebase now, and then look at custom animation code as a performance optimization for later.

Because of all this, I've started looking back into Unity's support for node-graphs, as Spaghet is meant to be a visual scripting language. In the last 11 months, it seems like [NodeGraphProcessor](https://github.com/alelievr/NodeGraphProcessor) has come a long way. I like a lot of what I see there, and so tomorrow I'm going to start digging into how to use it to implement the state-machine scripts. 

Once again, not making hard claims about the delivery of specific features is paying off as, if required, I'll be able to reorder a bunch of work without causing issues. I'm sometimes sorry that this means we can't give you as many concrete dates as would otherwise be ideal; however, the ability to roll with the punches is really helpful.

Hope you folks have had a good weekend, more news as it happens :)

Seeya
