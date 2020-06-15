---
layout: post
title: TaleSpire Dev Log 191
description:
date: 2020-06-15 23:13:04
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks.

Today, some of the things Ree and I have been working on don't make for great writeups. He was on business tasks, which included working on our GDPR documentation, and I was finishing a feature we'll be shipping in a few days if all goes well.

I'm not ready to say what it is as, although it's tiny, we want to ship it alongside an upcoming asset update. You'll just have to wait :p

After I finished off that feature, I started work on the in-game chat system. For messages to players within the same board, it's easiest to use the RPC calls of the networking library we use. However, for campaign-wide messages, we will use our server to relay the message to all connections which are subscribed to that campaign. This is likely to get changed up depending on what we can do with WebRTC down the line.

I wrote the code to handle this on the backend, and while I was at it, I added some code to handle the upcoming bookmarks feature.

With that done, I started mocking up the UI for the chat panel. I'm no designer, so please excuse the ugliness, but a rough shape is forming at least.

![uuuugly](/assets/images/chat0.png)

The little boxes underneath the message text box are attachment slots. I think we will allow you to drop references to creatures, locations in the board, dice rolls, and other stuff and send them along with your message.

By the time this is ready, it will replace the session history, the messages you had there will now be in this log, which will support tag-based filtering.

You will be able to target your messages at any of the following:
- Everyone in the board
- Everyone in the campaign
- Everyone in a specific party
- A specific player

My current thinking is that you'll usually use the drop-down to pick the target of the message, but if your message starts with `@<someone>`, it will as if you switched the target to that player, sent your message, and switch the target back. This is handy in cases when you need to send something very quickly, like if you are co-ordinating a mutiny with another player.

In time we also will want to support language proficiency in the chat. This would mean you can read messages that arrive in languages your creature understands (e.g. elvish), but the text will be garbled if it doesn't support that. Obviously, this needs more information to be attached to each creature, so this is more likely to happen closer to when we add rule-system support. It doesn't feel like something that should be prioritized over other features, though.

Time to call it a night.

Seeya!
