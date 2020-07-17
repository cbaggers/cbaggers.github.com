---
layout: post
title: TaleSpire Dev Log 203
description:
date: 2020-07-17 15:02:45
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Phew, this one took a while to get out.

The art team is hard at work, prepping the next big asset patch. Should be another fun one!

On Wednesday, I had some boring life stuff to sort out, so I wasn't working that day.

On Thursday, Jonny and I had a catchup meeting and roughed out a plan for the animation system for tiles & props. To get working on this, I first need to extract the animation data into a format we can work with from the job system. It took a while to realize that, if you load a prefab from a Unity [AssetBundle](https://docs.unity3d.com/Manual/AssetBundlesIntro.html), the full curve information is no longer available. This is ok, as in TaleWeaver we work with the original prefab files, but it was still a slow journey learning the ins and outs of this part of Unity.

With that worked out, I wrote the first pass on the extractor. This was looking ok, but the standard asset serializer only supports classes, and I want the data in a job-friendly format. Now, Unity does have a serialization scheme they use inside their ECS when serializing Entities to their Scene format. They call these [BlobAssets](https://docs.unity3d.com/Packages/com.unity.entities@0.2/api/Unity.Entities.BlobBuilder.html). These are fast, immutable, and pretty much ideal. However, they are not compatible with the AssetBundle serializer we mentioned early. To get around this, I have written an adapter that makes this possible. That took a long time, and I still would like to add a bit more data validation. However, I'm happy that I can now write the data in the way I want.

The next task is to rewrite the animation extraction code using this new store. I've been going slow here as I need to be able to uniquely a given resource (e.g. an animation clip) inside an asset, and I want to make sure that these are stable to the kinds of changes that are made when developing a tile or prop.

With that done, I'll be writing a new system for packing the board asset data. I've looked at this several times when I was investigating CaptnProto and FlatBuffers, now I have a good workflow with BlobAssets I'm going to use that instead. The current JSON format will continue being used by TaleWeaver to allow the art team to iterate on that format as required; however, when we export an asset pack, we will use the new system instead.

And once all that is finally done, I can start writing the animation system :)

I have also been researching Unity's new Physics engine again and am slowly designing how we are going to interface with that. It's going well, but I wanted to get animations working first as some assets (like doors) move colliders using animations.

Alright, until next time.

Peace
