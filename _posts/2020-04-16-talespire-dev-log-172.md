---
layout: post
title: TaleSpire Dev Log 172
description:
date: 2020-04-15 23:44:44
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good-evening once again from the north, it's been a good day.

I will be pushing a patch tomorrow that seems to fix the bug, which caused certain boards to break with `HardAssert` errors. I have also tested some of the broken boards, and 6 of the 7 I tested were able to load again.

Let's start with caveats. This will not fix all broken boards, tomorrow's patch only addresses this specific bug, and we don't yet know if all boards which had the issues are recoverable. If your issue manifested as something other than lots of `HardAssert` errors in the logs, then this fix won't recover your board. However, once this is out, I will start going after other bugs so we will see what can be fixed.

Ok, now the meat.

Many folks were stuck, not being able to get into their boards because of a bug which put this error in the logs:

> HardAssertFailure: HandleAddLayout: layout.AssetCount != _dequeuedAssetData.

An assert is a check that must be true for the program to continue. In this case, the presentation for a zone (see yesterday's dev-log for the details on what those things are) had been told it had to spawn a certain number of assets. However, there was not enough asset data in the queue to satisfy that request. Given that there was no sensible thing to do at that point, it threw an exception.

Now, I should have written some code to catch that exception, tell you something had gone wrong, and given uploading the board. However, I forgot to add the check for that code path before the release. Sorry about that :|

The first part of the work was what I did yesterday. Removing an optimization that I was concerned could have been hiding the issue due to its complexity. This did work, and this morning, I was able to open a previously broken board.

However, that fix, whilst required, was only part of the issue. The root cause was still there.

TaleSpire has to store a lot of asset info. Each kind of asset has a GUID that identifies it. A GUID is a 16-byte value, which is pretty large and so we don't want to store more than we have to. One simple thing to do is to group assets of the same kind together; then, we only need one GUID for all of them.

There are other values that can be treated similarly, and so we put these together in a struct called an AssetLayout. Each AssetLayout also has an ID (a uint in this case), which uniquely identifies it in the Zone.

An issue that was occurring was that, when a player deleted some assets that they did not place (or if they were from a freshly loaded board) and then pressed undo, the layouts that were restored were not given new unique Ids. This meant the Zone potentially contained multiple layouts with the same id.

This goes unnoticed at first, but on load of a board, we have to spawn all the assets. The information is sent off to the presentation, and that's where we hit the issue of layouts not matching the expected numbers of assets, all because our booking had been messed up.

Luckily the undo portion was relatively easy to fix. But we still have broken boards. What we needed was for something to go and clean up the ids of the layouts.

Let's take a slight diversion. Whenever a place places assets, we add a layout and the asset-data for whatever was just made. Over time, this will result in a lot of small layouts for the same kind of tile. We have to keep them separate at first due to how undo/redo works, but once they are out of the players undo/redo history (for instance, when we save the board), we have an opportunity to defragment this data.

I had not had time to implement this before release, but as luck would have it, this operation is exactly what we need to clean up the layout information in the broken boards.

So an afternoon of code later that is working. For now, I'm running this on load, rather than on save as this will hopefully fix some of the broken boards. However, once the patch has been out for a while, I'll change it to happen on save instead.

So that's that! I will review the patch tomorrow, test it with some more boards, and then I'll get it out to you all via Steam.

Huge thanks to Skye, lollypopunk, Al McWhiggin, Shmuky, and Mercer for sharing their broken boards with me so I could do better testing.

Have a good one folks
