---
layout: post
title: TaleSpire Dev Daily 34
description:
date: 2018-11-16 00:27:02
category:
tags: ['Bouncyrock', 'TaleSpire']
---

It's a little past midnight but who cares, it's been a great day.

Jonny has got the basics of the new placement code working. It's now much easier to move pieces under overhangs regardless of if they are on the same floor or not. He also got drag-select in so I will hook that up in a few days so we can delete/move multiple tiles.

I've been working on the game state and player handling code. It's now much more reliable and a fair bit cleaner. This work makes sure that if 'host' (the client tasked with syncing to the server) leaves that the job is passed to another client. 

I've also started work on supporting having the same user logged into multiple clients simultaneously. In the future this could allow us to provide additional tools that hook into the current play session.

It's hard to stress how much nitty gritty stuff has been, if not implemented, at least properly speced out for the release. I'm stoked for the next 2 weeks of progress.

G'night folks.
