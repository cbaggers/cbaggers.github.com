---
layout: post
title: TaleSpire Dev Log 441
description:
date: 2024-07-01 22:08:06
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, 

Today, I've been working on something different. A feature, for sure. But not one that goes directly into TaleSpire.

Today, I was making a program that can fit into [TitanCraft's](https://titancraft.com/) pipeline so that, sometime in the future, you'll be able to download TaleSpire-compatible assets right from their creator!

For those who don't know, [TitanCraft](https://titancraft.com/) is an excellent way to create miniatures online, both for downloading and also 3D printing. If you haven't checked them out before, I'd recommend it. Their approach to asset packs is really neat.

We've been chatting for a while about working together, so getting started on this is a lot of fun. My main task for today was taking TaleWeaverLite and reshaping it into something that could run headlessly. The goal being that they can then use this as part of the export process. I haven't tried to share the codebase with TaleWeaverLite yet as I'm not sure how this project will evolve, but I expect we might do that once we know what shape this will be in the longer term.

I got the basics working. We have a command-line tool that takes a digital miniature, and repacks it for TaleSpire. They result opens in TaleWeaverLite, but I've got a few bugs in-game that I'll fix up tomorrow.

Oh, and just in case anyone was worrying, no, we aren't moving away from HeroForge or anything like that. We love what they do, and we'd like to work with more folks in the space, not fewer! So, if you have a favorite way to bring characters and content into your adventures, why not let us know at [https://feedback.talespire.com](https://feedback.talespire.com). If it's been suggested already, please upvote it. We keep an eye on the vote to help prioritize what we are up to.

Anyhoo, that's all for tonight. 

Have a good one!

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
