---
layout: post
title: TaleSpire Dev Log 413
description:
date: 2023-09-26 00:46:18
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Evening all. 

All I wanted to do today was finish off the slab-sharing UI, but instead, I spent the day looking into an input bug.

It goes like this. Sometimes, you have keybindings that overlap. For example, on Mac, Undo is `Command+z`, and Redo is `Command+Shift+z`. This is an overlap; in order to hit the keys for Redo on Mac, you also need to hit the keys for Undo.

This was a problem we hit when releasing on Mac, so we turned on a setting in Unity's input system called `shortcutKeysConsumeInput`

Here's how Unity describes it:

> **improves shortcut key support by making composite controls consume control input**
>
> actions are exclusively triggered and will consume/block other actions sharing the same input. e.g. when pressing the 'shift+b' keys, the associated action would trigger but any action bound to just the 'b' key would be prevented from triggering at the same time. please note that enabling this will cause actions with composite bindings to consume input and block any other actions which are enabled and sharing the same controls. input consumption is performed in priority order, with the action containing the greatest number of bindings checked first. therefore actions requiring fewer keypresses will not be triggered if an action using more keypresses is triggered and has overlapping controls. this works for shortcut keys, however in other cases this might not give the desired result, especially where there are actions with the exact same number of composite controls, in which case it is non-deterministic which action will be triggered. these conflicts may occur even between actions which belong to different action maps e.g. if using an uiinputmodule with the arrow keys bound to the navigate action in the ui action map, this would interfere with other action maps using those keys. however conflicts would not occur between actions which belong to different action assets.

Seems ideal, but I think it has a bug. When `shortcutKeysConsumeInput` is enabled, we see a problem by following these steps:

- User selects a region of tiles
- User cuts with `Ctrl+x`
The user dismisses the slab from the hand with a right-click of the mouse
- The selection mode is stuck on.

What is happening is that when the user presses `Ctrl+x`, Unity fires an event for Cut and for the selection modifier (the `x` key by default), but it doesn't fire when the key is released.

I've prodded a bunch of settings, but it does seem to be a bug. My plan is to give up on `shortcutKeysConsumeInput` and handle the collision cases myself. This sucks, but we really need to get this fixed so we can get back to everything else on the todo list.

It's rather late here, so I'm gonna get to bed. Hopefully, this isn't too painful to deal with tomorrow.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
