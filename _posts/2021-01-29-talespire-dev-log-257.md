---
layout: post
title: TaleSpire Dev Log 257
description:
date: 2021-01-29 02:18:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks, the last week has been one where giving updates would have been challenging as most days have consisted of me grumbling at a whiteboard, but let's do it anyway :p

At least half the week was spent working out how I wanted to store board data on the backend. We knew we didn't want sharing boards to result in everyone having a duplicate copy of the board on the backend as this would mean 1000 people using a shared board would result in 1000x the storage requirement. Obviously, that means we want different people's boards to refer to the same data. However, almost all play sessions involve small changes to the board (for example, if a fireball destroys a wall), and we don't want that first change to result in duplicating the whole board, so we want to be able to store only what has changed.

This has been long-planned under the title 'per-zone sync', the implementation details are a little trickier as we start looking at worst-case numbers, however. A board (today) is 2048x1024x2048 units in size, and we divide that into 16x16x16 unit zones. That means the worst-case number of zones is 128x64x128=1048576 per board. Not only is that a lot, but each zone isn't (usually) much data, and we definitely don't want the book-keeping to be larger than the zone data. Furthermore, TaleSpire needs to be told what to download, so more separate files mean more things the backend has to tell the frontend. Let's do something simple to help this; let's serialize zones together. We will pack zones in 2x2x2 zone chunks (which will later be called sub-sectors),  meaning the worst case is now 64x32x64=131072. Not great, but better.[0]

However, these chunks are still reasonably small. They are great for localized changes, but it's overkill for the board regions that aren't changing frequently. So let's make an alternate chunk size of 8x8x8 zones. We can then choose between these larger 'sectors' and the smaller 'sub-sectors', which gives us options when dividing up the world[1]. 

So we have 4x4x4=64 sub-sectors to a sector, but quite a few more sectors to the board. If we follow the same 4x4x4 pattern of sub-sectors to sectors, we get 4x2x4=32 regions of 32x32x32 zones per board. Having a consistent branching factor (64) is nice, and 32 regions will be handy in a couple of paragraphs time.

Now we have something that can work as a sparse hierarchy of spatial data, let's look at copies again. Rather than tracking ancestry at the board level, We can have each sector point to the sector it is descended from. This lets us easily walk backward and find all the files contributing to your version of that sector.

One concern arising from recursively walking the sector data is query time. We directly address rows, so the main factors to query speed (I think) will be table size and index kind. Focusing on size, we can actually do something to help here. The first thought is to use [table partitioning](https://www.postgresql.org/docs/10/ddl-partitioning.html), but I didn't like it for two reasons:

- It's built on table inheritance, and that doesn't seem to maintain the reference constraints in the child tables
- It looks tricky to ensure we keep sectors from the same region in the same partition

However, we previously mentioned those 32 regions of the board. What if we treated those regions as our partitions, made 32 tables, and handled it manually. Apart from it being a lot of work, it seems promising. However, most boards aren't going to use every region, so we'd still get clustering around the origin. Unless, of course, we introduce some factor to more uniformly spread the data across the partitions. The way I'm looking at doing it is taking the region-id, adding the low bits from the board-id of the oldest board ancestor, and then modulo by 32. This spreads the data across partitions and means that regions of copied boards live in the same partition as regions of the boards they were copied from[2].

With the DB side starting to make sense, I had hoped to find a way to trivially dedupe the uploaded data itself in S3. The original idea was to name the uploaded chunks by their hash. That way, two identical chunks will get the same name and thus overwrite each other. The problem is that you can get hash collisions, but these can be minimized by hashing the data with one hash[3], prepending that to the data, and then hashing with a second hash. S3 will validate an upload with md5 if you request it to, so that seemed a logical choice. 

However, a huge issue is that we have only made it hard to accidentally get a hash collision; it doesn't stop people from being malicious and trying to break other peoples' boards. There is no great answer to this. The only thing to do is upload with a unique name and then have a server-side process validate the file contents before allowing it to be de-duplicated. I'll be adding this after Early Access has shipped.

Speaking of md5, now that compression is jobified, the md5 calculation is one of the only parts of saving a board that we don't have in job yet. To remedy this, I'm looking at porting the md5 implementation from https://github.com/kazuho/picohash to Burst. This looks straightforward enough and will be a nice change from the planning of the last week.

While planning, I've been weighing up different potential implementations, and this has required reading into a bunch of SQL and Erlang topics. One neat diversion I went on was looking at binary serializing for our websocket messages to the server. Previously I had looked at things like protobufs and captnproto. However, I had ignored the fact that Erlang has [ETS](http://erlang.org/doc/apps/erts/erl_ext_dist.html). ETS is a simple binary format which it uses when communicating between nodes. Some of the message kinds only make sense within Erlang, but one possible subset was identified for external use called [BERT](http://bert-rpc.org/). 

I played around with BERT, but I didn't like how floats were encoded. ETS has a newer, nicer way of encoding them, so I took some inspiration from BERT and started writing a simple Burst compatible way to write ETS messages from C#. I was quickly able to encode maps, tuples, floats, and strings, and sending them as binary messages to the server was also trivial.[4]
The next step would be decoding and updating the backend's code generator to use this. It should result in faster encoding/decoding with no garbage and smaller messages over the wire. Still, I'm going to leave this until after the Early Access release to avoid risking delaying the release even further.

I think that is the lot for this last week! I'm hoping to start making progress more quickly now that the plan has taken shape. I've already started writing the SQL and aim to start the erlang and c# portions as soon as possible.

Hope you are all well,
Peace.


[0] Of course, we can also add limits of how many zones can be in each board, and we'll almost certainly do that.

[1] This hierarchical spatial subdivision is probably making some of you think of octrees. Me too :P however, this approach will give us much shallower trees, which will be good for limiting the depth of the recursive SQL queries to get the data. 

[2] Naturally, this still means that popular boards will introduce a lot of extra rows to the partition in an unbalanced way. Still, each popular board will be uniformly spread across the partitions too, so it all should even itself out.

[3] I'm looking at xxhash3 as it's super fast and has a Burst compatible implementation already made by Unity

[4] Given that we are now not using BERTS, we could go another step further and make our own binary format with only what we need. This is definitely an option, but moving to ETS first is a decent step that means we don't have to write an encoder/decoder for the erlang side. We can swap out the implementation once we have moved to binary messaging and ironed out the kinks.
