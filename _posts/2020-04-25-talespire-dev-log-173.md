---
layout: post
title: TaleSpire Dev Log 173
description:
date: 2020-04-25 02:29:01
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks. Recently, all the dev updates have been patch release notes, so I thought I'd drop in to say hi.

Bug fixing has been intense, and we've covered a lot of ground. Over 100 tickets are closed on github, and we have no worries about being able to tackle plenty more. After seemingly fixing one of the significant connection issues the other day, I've felt pretty sluggish. Feels like my body telling me to take it easy, so I've tried to do that for the last two days.

This evening I started pulling exception logs to see what easy wins I could find there. Where possible, we push the first exception we find to the server, and that can be quite a goldmine. It went pretty well, and the next update is going to have a bunch of random fixes in it.

The more exceptions we can remove, the less spurious or random bugs we will see reported. Some times a bug we find is just the side-effect of some earlier thing breaking. Kind of like if your car flips off the road, it might have had something to do with the wheel that fell off just before :P

The fixes had stuff like:
- selections still trying to highlight tiles while switching boards
- A state-machine for board sync hadn't registered one of its failure states
- UI trying to access stuff that had been disposed while transitioning to the main menu.

Yeah, a real mix.

Ree has (amongst other things) been looking into a GPU instancing library and Unity's new ECS. No conclusions reached yet, but we'll let ya know once there is news there.

We've also switched to using some Unity packages, which are still in preview. This is a risk, as they are still finding bugs, but we've done it for a couple of reasons:

1. It allows us to dig into the ECS stuff mentioned above
2. The preview version of their Burst compiler now seemingly supports fallback to SSE2 instead of requiring SSE4 (which has caused some users issues)

The next patch will have these changes so we can see if that helps.

Alright, it's rather late, so I'm going to get some sleep.

Peace
