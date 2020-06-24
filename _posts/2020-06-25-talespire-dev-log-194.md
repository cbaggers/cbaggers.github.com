---
layout: post
title: TaleSpire Dev Log 194
description:
date: 2020-06-25 00:05:40
category:
tags: ['Bouncyrock', 'TaleSpire']
---

> WARNING: The visuals for, and even the kinds of, rulers in this log are 100% temporary. The point is to work out a possible structure for the code and network sync. Drawing any data or conclusions from the clips presented in this update is meaningless.

Heya folks, today Ree has been prototyping props, and I'm going to ramble below about what I've been noodling with.

After seeing a conversation in the discord about area-of-effect, I got a strong desire to hack on rulers again. My goal was to make a prototype of one possible mechanism which could give us fairly a simple way to define rulers.

I wanted the ability to support simple point-to-point or volume rulers, but also ones with some additional adjustable parameter (like a cone area-of-effect)

The approach is as follows:
- A ruler is a container which holds some different visuals for rulers (which I'll call presentations in this update)
- The ruler is a simple state machine with four states
  - Placing the first point
  - Placing the second point
  - Adjusting the 'parameter' (optional step)
  - Done 
- The ruler might be visible to other players. In which case it handles the sync
- A presentation does not have to support a parameter
- Presentations can be cycled at any point before the 'Done' state

The 'parameter' is one part that might seem a bit vague right now. We should talk about it, but first, have a peek at this clip.

![testing rulers kinds](/assets/videos/ruler1.gif)

Unlike the sphere and line rulers, the circle area-of-effect presentation had an additional step after placing the center and defining the radius. It allowed the user to show a slice of the circle. To make this, I needed the third point to be on a plane defined by the first and second points. From the volume selection tool, I had already seen that it was handy to define a plane to raycast against, so I decided to add that to the parameter. The optional parameter is either a point on the board (just like the first two), or it can define a plane to constrain the point. Simple stuff but might allow some useful tools.

So this prototype is done we can kick the tires and see if it's in the right ballpark. It's totally fine if we throw it all away, or gut it and do something similar. The main thing is that it's something tactile in-game that we can play around with.

With this itch scratched, I've jumped branch to look at the session/chat panel prototype again.

That's all for now. 
Seeya


p.s. Here is a little clip showing sync working between two copies of TaleSpire.

![testing ruler sync](/assets/videos/ruler0.gif)
