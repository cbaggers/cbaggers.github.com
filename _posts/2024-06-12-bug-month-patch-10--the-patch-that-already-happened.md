---
layout: post
title: Bug Month Patch 10 - The patch that already happened
description:
date: 2024-06-12 10:14:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

This is going to be more of a dev-log than a release post. It turns out that, along with the assets we shipped yesterday, I had also accidentally pushed the fixes I was in the process of testing.

Luckily, the fixes have passed the tests, so I'm putting this out there to let you know what they were.

If you updated yesterday, then good news! You already have these fixes. If you didn't... GOOD NEWS! You're getting them in your next update!

The fixes in question were.

- We have fixed a shadow bug that would manifest if there were more than 500 of a single kind of prop in a 16x16x16 region
- We wait a little longer before retrying the pinging of Photon servers
- We removed the slight hover that props had during placing. It made it really hard to line up props accurately
- We now snap the position of the prop you are placing to hundredths of a unit while in your hand. The snapping is always done when the prop is placed, so this change ensures that you are seeing what you will get when you place the prop.
- Unity wasn't showing some lower framerates, so we tweaked settings to show lower frame rates if they are integer multiples of valid frame-rates. This is a hack, but should help some folks who want to set TaleSpire to 30fps

Lastly, Mac users may have noticed that, on macOS, we now save screenshots to the desktop. This is because the old location was super ugly and tricky to get to. We will replace this with a path you can configure in the future.

Alright, I'm gonna get back to work and see if I can get something ready for tomorrow.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
