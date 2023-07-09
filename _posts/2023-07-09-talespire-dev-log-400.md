---
layout: post
title: TaleSpire Dev Log 400
description:
date: 2023-07-08 20:41:02
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

Well, the Symbiotes release was great! There was one simple bug we needed to patch, but aside from that it's been going very well. We had crossed 10k Symbiotes downloads before Friday ended and it's been a delight to start seeing community symbiotes rolling in.

With Symbiotes out I could start looking at other things and so I upgraded Unity to see if the macOS graphics fix worked. The answer is: YES!

![TS on macOS with working water](/assets/images/macWaterFixed.png)

For context this is what water looked like before.

![how water looked before the fix](/assets/images/macWaterBroken.png)

You might have spotted the little red errors in the first screenshot, so clearly not everything is working, but it's a great start.

The plan is that @Chairmander and I will begin thoroughly testing the Windows build on Monday. If that passes then we will hopefully get this merged very soon.

The little errors mentioned above are shader related. I've started trying to hunt them down but I'm not sure what they are yet. Once they are sorted we'll get macOS builds set up in our CI and start the testing process.

I'm not happy with the performance on m1, but it might be worth it to mac players to ship sooner and fix the perf issues later. We'll see.

macOS support has been hanging over me for nearly a year now and I can't wait to get this out. Let's hope it's soon.

I've got plenty more little things in the works but I'll talk about them another day.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
