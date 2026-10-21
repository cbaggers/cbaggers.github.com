---
layout: post
title: TaleSpire Dev Log 519 - Font-a-thon
description:
date: 2026-10-21 10:21:24
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Over the last couple of days, we have been going through TaleSpire, replacing the old font with our new one, which supports many more languages and simply works better!

TaleSpire predates Unity's nested-prefab feature, which means we have a lot of copies of similar UI elements that, if we were starting fresh, would definitely be made of reusable prefabs.

One big pain is that we are on an old version of Unity's TextMeshPro library, and we can't upgrade until we reach Unity 2023.x, and that version doesn't support GPOS kerning tables in fonts. SO, we did a ton of work, and now we have to tweak the font to get proper kerning. GAH.

Luckily, our font guy is a star and is on it.

All our existing work should still be basically correct, so it's not a disaster.

One big gotcha, though, is that swapping out the font file in the Unity project doesn't actually update the TMP-font-asset, EVEN IF you right-click and "reimport" the font file and the TMP-font-asset.

Instead, I had to make a new TMP-font-asset from the font file. This'll mean swapping out the asset on font files, but swapping out the guid in all files is trivial, so that'll be fine.

On the symbiote side of things, I've started work on putting the audio from symbiotes through the Unity audio system. This will allow master volume to work across Symbiotes and also give opportunities for post-processing work to work consistently across all things in TaleSpire. Exciting! The plugin gives us most of what we need, but I want a more efficient version of the cross-thread communication[0], so I have a little work to do there (just a ring-buffer, nothing fancy).

I'm also doing some testing of markdown->tmp-text for the notes feature. Looking forward to getting those in the game.

Things are really on a tear right now. Chat with you all again soon!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] We force some specific details that the Vuplex plugin can't assume, such as channel count and audio rate. So we get to make some simplifications. Also we can use Burst, which Vuplex currently doesn't depend on, so we can do sample interleaving much faster they they could.
