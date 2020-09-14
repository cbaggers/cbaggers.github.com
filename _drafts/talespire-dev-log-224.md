---
layout: post
title: TaleSpire Dev Log 224
description:
date: 2020-09-14 13:16:05
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi everyone, last week was intense but productive. I've been down at Ree & Heckbo's place, working on the engine. We had hoped to start integrating our branches but my stuff still was not ready so instead we just kept working making use of the collaboration advantages that being in the same room gives.

As there has been a bunch of stuff happening I'm gonna spread it over a few posts.

Let's start with something random. Line of sight.

Line of sight chekcs came up in conversation in the community recently. The issue was that people we having issues with the fact that one creature can block the line of sight to other creatures behind it. This is one of those things that sounds like it makes sense but just doesnt feel right in game. Any gaming using miniature is always an approximation of the fantasy setting and so sticking to it like it represented reality doesnt make sense. 

Lets take a concrete example. A giant is blocking the view to a goblin behind it's leg. This sounds fine but imagine the scene as it would play out in 'real life'. The giant would be towering above you, and as it walk you catch glimpses of the goblin as it darts about trying to get a shot on your comrades. This fidelity is lost if we treat the table as ground truth.

Instead it's better to ignore iter-creature relationships when checking line of sight.

This poses a little implementation issue however that is worth exploring. Back in the alpha we checked line of sight using collision rays. This worked but it was inaccurate you can only perform so many checks per frame, back them we checked less than 10 positions per creature and false positives and negatives were common. People rightly asked for more accurate line of sight. 

In response we changed the approach to rendering the whole scene from the point of view of the creature in question. We rendered tiles and creatures into a cubemap using colors as IDs. We then used compute shaders to gather the results and report back what colors (and thus creatures) we visible. As you cna see this introduces the issue that any creature that is fully obscured by another creature will be considered invisible.

To fix this limitation we are going to need something new. The idea is to render the scene without creatures into the cubemap, then we will render all the creatures but they will not write their depths (in-fact we will probably discard all their fragments). This means that we should get a fragment shader call for every fragment in each creature that isn't discarded by depth. We can then write the visibility info from the fragment shader into the result buffer, hopefully giving us the result we need.

This will also help fog of war, which uses the same visibility cubemap. (more on fog of war another day).

I'm gonna jump back into code now, more dev-logs coming when I take breaks!

Ciao
