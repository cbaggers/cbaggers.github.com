---
layout: post
title: TaleSpire Dev Daily 57
description:
date: 2019-01-16 00:36:24
category:
tags: ['BouncyRock', 'TaleSpire']
---

Quick rundown of client-side from today:

- Fix a bug that occurred in battle mode when clicking next with an empty creature list
- Made the `?` button hide the help if already open
- Made the atmosphere music loop with a configurable delay between each loop.
- Fixed drag-select so that it would work properly when you let go over a UI element
- Atmosphere block are now drag-selectable
- Try to ensure that steam definitely knows that the game has shutdown [0]
- Don't sync board if you are leaving due to the board being deleted.

I also started on the following
- Can't stand on `dirt (Tilled)`
- Unique creature panel is not updated on creature rename.

The dirt one I though would be a doddle and it was missing some required information, however after adding that it still doesn't work. It'll be something stupid I'm overlooking but that's where it is now

The unique creature ticket is the start of a few issues I'll be working on in this area. For some reason the ui is not updating when the rename is applied and I have to close the panel and open it again.

After that I've got to look into some issues with creature ownership. Those are annoying as I don't know what triggers them so it's gonna be a lot of logging, testing and code review. We've already seen this mess with people's streams so it's a high priority one for me right now

I also took a couple of hours to look at some work I need to do on the backend. One of the pressing tickets is that, currently, there is no limit to the number of older versions of your boards we store. We obviously need this as storing more that we need increases our costs and thus limits how far each sale of TS can take us. It's not a hard task but having some time to muse over the best way to structure the queries was nice. The second backend task was to design a basic stats collecting module so we can start getting better info on how many people are active at different times of day and, on average, what kinds of requests they make. This is important so we can make some better guesses at how load etc will increase as we move from hundreds to thousands of users and beyond.

One last thing, when I went to bed last night I realized that the line of sight technique is pretty simple if we limit the number of creatures involved. It goes roughly like this (for this example we will use 256 as the max number of creatures):

- make an ssbo which contains an array of 256 bools
- find all creatures in range and give each an id between 0 & 256
- clear the ssbo so all entries are false
- render the scene from the selected creatures position, in the 6 cardinal directions, into a cubemap
 - render the occluders as black and creatures with their red color component based on their id. e.g. `vec4(id/255)`
- draw a quad and use the fragment stage to read a single texel from each face. Write `true` into the array using the red component of the texel as the index into the ssbo array e.g. `result[(int)(sample.r * 256)] = true`
- pull the data back to the cpu side.

The slots that are true state that the respective creature is in line of sight. We can use a 512x512 texture to trade off accuracy for speed but given that we only need this once per time something moves it should be no problem to have something a little higher res.

I haven't thought of a way to do it without an ssbo (or writable image) yet though.

I might make a prototype of this in lisp as a stream next week. Sounds pretty achievable in 2 hours.

Goodnight folks, back tomorrow with more.

[0] We can the Steamworks shutdown call in an OnDestroy which should have been triggered on all normal terminations. However sometimes it still didnt work, so we've added another call to it from a method that is explicitly called on shutdown.
!
