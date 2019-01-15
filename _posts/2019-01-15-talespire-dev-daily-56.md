---
layout: post
title: TaleSpire Dev Daily 56
description:
date: 2019-01-15 00:59:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks, these dailies had to be put to the side during the crunch to get the alpha out of the door, but now a few days have passed I'm ready to get them started again.

Today has been a slow day as I guess my sleep schedule is still somewhat recovering, however I worked on a couple of bugs.

The first was ['Ambient lighting stacks rather than replaces prior atmospheric lighting'](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/47). This turned out to have a simple mistake which took me a while to find as I was sure it was an error in the LUT post-process. Instead it was that some atmospheres don't specify new values for the lighting and, instead of the defaults being used, the previous value just remained.

The next were ['Line of sight is behaving oddly'](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/51) & ['Line of sight indicators show for creatures on all floors'](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/61). These ones have a number of components to it that make it fiddly so currently I'm working on some tweaks (hacks?) to what we have so that the result is more acceptable whilst we come up with a better approach to replace it in future.

Previous, in order to see if one creature could see another we just had a simple sphere cast between the two.

![0](assets/images/los0.png)

Issues included not taking creature eye position into account and that if the ray clipped some scenery, as in the case above, the creature would disappear even though most of it was in view.

To mitigate this I'm now taking multiple raycasts between an approximation of the head positions of the two creatures. We take one ray from the center of the head, one from slightly to the left and again from slightly to the right. On the destination creature we aim a bit wider in order to take the model size into account.

We also cast 1 ray from the source creature's head to close to the base of the target creature. This lets us catch cases where a creature can part of another creature on a lower floor.

![1](assets/images/los1.png)

As mentioned before, these are really tweaks to patch over issues with the current approach. One thing we've mused over is capturing a low res version of the scene into a cubemap (as in some lightprobe techniques) but naturally instead of rendering the colors of the scene we render IDs of creatures into the result. we can then see exactly what the creature would be able to see (more or less) and could accumulate the IDs into an SSBO (or similar buffer) to be used the following frame for showing/hiding the creatures in view.

The only non-trivial[0] part is the accumulation pass but it sounds like a perfect task for a simple compute shader.

I'll write more about the new approach when I have done some more work there.

I'm gonna push my line of sight tweaks tonight so the rest of the team can have a play with them. Hopefully they'll be out in the next update.


In non technical news it's amazing to have the alpha out there. It's mind blowing how quickly you see things you didn't think of (someone playing chess in it for example :D ) and it's intimidating how quickly the community content overpowers your ability to both keep up to date, and get work done. Some folks have already been amazing at filing really good bug reports in our issue tracker and guiding the new people as they first settle in, so it bodes very well for this as it scales.

Another thing I've not personally found a good avenue for is feature requests. Any game, tool, whatever.. feels a certain way due as much to what it doesn't do as what it does. Setting good limits and excelling in your space makes for a better experience than piling it all on. Some features are non-brainers and we will definitely want to get them in, however there is larger set of perfectly reasonable features that just aren't a good fit for this project. I guess this is part of the reason I see TS to be part of the roleplaying ecosystem rather than something that's trying to replace anything. I'd sooner see 5 focused competitors than one bloated tool without focus.

The issue then is handling these tickets. If you've seen sites like the now dead Ubuntu Brainstorm you'll know the issue. Things that are accepted get fixed but the things that don't accumulate and can become little foci of disappointment. You can prune these but that too has to be done with care as otherwise you either get a lot of duplicate requests or you could piss off the great folks that took the time to request in the first place. I have no answer to this and I may just be over-thinking it anyway. I expect we'll just put up a trello (or something similar) and just see how we go.

Tomorrow I'm kicking off with some work on 'atmosphere music not looping', some small UI bugs and then I'll dive back into the big pool of bugs and see which can be swatted next.

Until then,

Peace.

[0] non-trivial as I haven't done much compute work and none in Unity so far.
