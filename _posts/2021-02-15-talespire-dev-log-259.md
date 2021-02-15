---
layout: post
title: TaleSpire Dev Log 259
description:
date: 2021-02-15 19:07:20
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi everyone!

Progress is excellent behind the scenes. Unfortunately for the dev-logs, my recent work hasn't made for very interesting writeups. 

My focus has been on and around the new board sync.

In TaleSpire, each change to the board is applied in the same order on all clients. Naturally, this means we need to start from a common state. So if you join a board after someone else, how do we catch up? We will need to fetch the board from the GM, but that is not enough as, in the time we are waiting for the board to transfer, the board could have changed. In the current build, we use the following approach:

- When you join the board, start recording all board change operations which arrive (each has an id)
- Ask the GM's client for the board. The client will write the ID of the most recent board change operation that had been applied before the sync along with the board data.
- When the board is finally delivered, deserialize it and discard all the recorded board change operations with IDs older than the one in the board sync file. 
- Apply the remaining board change operations in order

We now have a complication. Each client may only have a portion of the board locally. Ideally, we only want to sync the parts of the board that have changed this session and load the rest from the server. It took me a bit to work out that we should only need to pause applying changes if an operation intersects a zone that exists but is not local yet. We don't need to do the same 'id juggling' as the former approach (although it's still required for the parts coming from the GM).

Part of the above is about keeping info on what zones or sectors are loaded, modified, etc. I spent some time making sure we do this efficiently.

When we release this, we will need to upgrade all the boards that are already out there. We usually do this transparently on load; however, this time, the changes are so fundamental that I want to make it a dedicated step to reduce the chance for error. This means that once the update is out, players won't be able to connect to a campaign until it has been upgraded by the campaign owner. The owner will simply have to click an update button and allow the process to run to completion[1].

I've added a new board format to my branch where I am putting the new logic, and I've started writing the system that will handle the update itself. This change touches so much code that I find myself jumping back and forth doing small parts on one system to allow progress in another.

As we are making changes to the board format again, I made sure all the tests were still passing. We don't have a CI server, so these tests are run less frequently than they otherwise would be [0]

As I was working on the format, I decided to move board-wide settings like the water height and most recent atmosphere to the database. As I was implementing it, I got really annoyed at some poor JSON was for encoding the data, and, against my better judgment, I decided to spend the weekend on a little diversion.

That diversion was to implement binary messaging to the server. In previous posts, I had talked about experimenting with [ERTS](https://erlang.org/doc/apps/erts/users_guide.html), erlang's built-in binary format, and how I had already written some experimental code for making ERTS messages from C#. The plan was to update the code generator on the backend to support custom types in requests and replace the JSON serializer with ERTS. 

This was going pretty well, it was tricky, but by late Sunday, I had the first messages set from TaleSpire to the backend. However, when I thought I had about an hour of work remaining, I spotted a critical issue.

I had wanted to use specific sized integers for certain parts of the message. When sending messages from C#, this was easy to control as it was my code. However, on the server, I used a method called `term_to_binary`, which is analogous to `Json.Encode` or similar from your favorite language. What that method helpfully does is to look at the value it's encoding and pick a suitable encoding, which could be bigger or smaller than I needed[2]. 

This sucked. I didn't want to handle those different possibilities on the client-side, but there wasn't a way to control it on the server side without writing a custom ERTS encoder. Of course, that defeats the point of using ERTS as I wanted something to reduce the amount of work I needed to do.

Apparently, I refuse to pay heed to the sunken cost fallacy. If I were going to write an encoder, I would make something much less dynamic than ERTS that would make smaller messages. For example, there is no point in encoding the values' types if the message has a fixed layout. And so that's what the next 8 hours were — a dogged hammering at the keyboard until the format came to life.

So we now have our own format. Because it's a binary format, the data is usually smaller than the equivalent JSON text representation, and writing the messages is simply pushing bytes into an array rather than conversion to a more human-readable format. 

An awesome side-effect is that it's now easy to add support for more types. For example, I added a dedicated UUID type that sends them in their usual 16byte representation rather than the 36 chars needed for the text version. Given how many UUIDs we pass up and down, this alone is a win.

I cannot stress enough how awesome erlang's syntax is for handling binary data. It boggles my mind that low-level languages haven't jumped to add something similar. For example, let's start with 4 bytes of data in binary.

```
Data = <<255,255,255,255>>
```

Now let's say we want to unpack 3 coordinates from those 4 bytes, where the X and Z coords are 11 bits, and the Y coord is 10:

```
<<X:11, Y:10, Z:11>> = Data
```

That is binary pattern matching in action. X is bound to the first 11 bites, Y to the next 10, etc.

Obviously, we often want to read out specific values where we care about endianness. Below we see how to read the first 20 bits as a little-endian integer and the final 12 bits as a big-endian integer.

```
<<X:20/little-integer, Y:12/big-integer>> = Data
```

The result from the above is X=2047, Y=1023, and Z = 2047.

You can also bind the rest of the data to a variable:

```
<<ArrayLen:16/little-unsigned-integer, RestOfTheData/bytes>> = SomeLargeBinary
```
The above binds a `ushort` to `ArrayLen` and the rest of the bytes in `SomeLargeBinary` to `RestOfTheData`. This made chaining patterns trivial and allowed me to make a tiny binary encode/decode library in no time.

So yeah, erlang is still a great choice :D

That's enough for now. This week I'll keep working on board sync.

See you around 

p.s. Although I don't write for other team members, there is plenty of cool stuff happening. Although we are not changing our release date from 'Early 2021' we are still on track for our own internal goals. TaleSpire is coming. The release is gonna be fun!


[0] Tests run with a specific build flag as we have to track additional data that we do not want in normal builds.

[1] The reason is that some data which used to live in the database (non-unique creatures) is moving to the board files and the board data, which used to be one file, is now many. I am nervous about an error or network failure resulting in partially updated boards, and so making it a big, dedicated step seems safest.

[2] I want to stress again that this is an excellent decision for how ERTS is used within erlang. It was just a failure on my part that led to this issue.
