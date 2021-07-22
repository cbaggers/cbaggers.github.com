---
layout: post
title: TaleSpire Dev Log 287
description:
date: 2021-07-22 11:20:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hellooooo!

It's time for another dev log, and things are moving along well.

Since the last patch, I've fixed touched a few things.

## Links in and out of boards

We are working on two new and complementary features. The first is a `talespire://` URL that takes you to a specific position within a board, and the second, which allows you to attach a URL to creatures and markers.

Between these, you could do things like include links into boards from campaign management software like WorldAnvil, or link directly from your creature to their D&DBeyond page.

I'm currently working on the database, backend, and TaleSpire patches to make this work.

## Download validation

We have hashes of all board and creature data, but we hadn't been using it until now. We now hash the data of downloads and use that to validate the download result. This should reduce the cases we've seen of invalid cache files.

## Pixel-perfect camera focus

Moving the camera using double right-click is incredibly common; however, it used to only use physics ray-casts to work out where to move to. This didn't work well in cases like the portcullis, where the collider definitely *should* be a single cuboid, but that doesn't let you pick through the gaps.

I tried using the depth information from the pixel picker to get the position, but the accuracy was too low.

The new approach is a hybrid. We use the pixel picker to identify the thing under the cursor, and then we cast a ray only against the colliders in that object. This gives us the expected result and will be in the next patch.

## Network Tester

We have some users with issues connecting to the game. To help with these cases, I'm updating a little tool we made to test and log the connection process. I might end up shipping this with the game or maybe build it into the game and using command-line arguments to switch to the tester on launch.

## Performance improvement

I also just have to shout out some work Ree did the other day. To get shadows with reasonable performance in TaleSpire, we misuse [BatchRendererGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.html)s. Due to them not being designed to work with Unity's built-in rendering pipeline[0], they seem to have a bug where the layer information is not respected. In practice, this means that we end up running culling routines for tiles and props even when the camera has specified that it doesn't need to include those assets (using layers). Because of this, culling code was running whenever the reflection probe was updating, even though only a couple of objects were being rendered. 

Ree made a replacement probe that totally avoids this bug. It's really cool as the bigger the board, the more time this can save. 

This will be shipping in the next patch.

## And the rest 
As always, there are bugs to fix. One that jumps to mind was that, since the last patch, creatures can be unnamed. In those cases, the name displayed should be the name of the kind of creature. However, that wasn't happening.

That's all from me. Of course, the rest of the team is plowing ahead with all sorts of exciting things. It's gonna be great to see those land :)

Have a good one folks

[0] We did not know about this limitation when we started as the documentation was very lacking.
