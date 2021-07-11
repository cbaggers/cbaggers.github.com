---
layout: post
title: TaleSpire Dev Log 284
description:
date: 2021-07-11 03:16:02
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks!

The last few days of works have felt really good. I've been back in the flow as I have got through the 'working things out' stage.

The first thing I did was write the serialization code for the new creature data. With this done, I could hook up the code to upgrade from the old format, refactor a whole bunch of stuff and get creature saving to the backend again[0].

With that taking shape, I switched to the backend.[1]

We have an API description as erlang data, and we use that to generate both the erlang and c# code for communicating with the server. I extended this generator so it can make c# structs as previously it only made classes[2]. I also improved the serializer on the erlang side, so it needed a little less hand-holding.

I then used the generator as I defined the new API for creating and updating unique creatures.

My old code for updating unique creatures was lazy/expedient (you decide :P). I previously pushed all of a creature's data to the server on any change. This was obviously much more data than was needed in almost all cases, but it worked. This time I made many more entry points, each of which applied smaller changes. This will reduce work for the server and hopefully make things a little faster.

With a draft of the API, I could generate the c# code and then add it to TaleSpire to see if I had missed anything. Once I was satisfied with that, I head back to erlang to implement the server code.

While working with the database, I hit an error when calling a SQL function where one of the arguments took a [user defined type](https://www.postgresql.org/docs/9.5/sql-createtype.html). It turned out that epgsql, the library I use, didn't support auto-conversion of erlang data to custom SQL types. They did, of course, have a way for you to add your own 'codec' to do this, so it was time for me to learn about that.

The [manual was helpful](https://github.com/epgsql/epgsql/blob/devel/doc/pluggable_types.md) but didn't provide concrete examples for what I was trying to do. I read the [codecs included with the library](https://github.com/epgsql/epgsql/tree/devel/src/datatypes), and that helped me to draft out the basics. What is nice is that you get to write and read the binary data directly, and erlang's binary pattern matching is wondeful[3], so I didn't worry about that.

What was not clear from the existing codecs was how to pass data for a user-defined type rather than a built-in one. I first tried just sending the data for the two fields of the type, but I got an error along the lines of "-34234324 columns specified, 2 expected", which told me I needed to pass the number of columns, and that it knew what I was trying to send. A bit of googling led me to the [source code for the part of postgres throwing this error](https://doxygen.postgresql.org/rowtypes_8c_source.html). What was great was that at line 520, we can see that it reads a four-byte int to get the number of columns. We can then see exactly what else we need to provide, the most interesting of which is the type-oid for each field we are sending.

At first, I just muddled through as the errors I got told me which type-oids it was expecting[4]. However, this felt very fragile, so I jumped back into the code for the library to see what I was meant to do[5]. What was lovely was that on the init of your codec, they pass you an object you can use to query the type-db for your connection. This let me cache the type ids on init, which was ace.

As I didn't see an example of this anywhere else, I've put up [this gist](https://gist.github.com/cbaggers/0bce6c3d08c7a5d9441210af507afe1e) of what I came up with. Maybe it can help someone, or perhaps someone will correct me and show me how it's done :)

With that hurdled officially jumped, I got back to the slog of writing and testing the code.

I finally got unique-creatures working again[6], so it was time to start work on how we will upgrade to this new code. This involved the following SQL code:

- Code to apply the changes to the DB
- Code to initialize all the new columns with data from the existing columns (where sensible)
- Make the old unique-creature SQL code forward compatible. This will let you keep playing on an older version without the new and old data going out of sync.

With that DB patch looking promising, it was time to test on my local setup. This means:

- Starting a build of the old DB and server
- Building some stuff and placing some creatures in TaleSpire
- Applying the SQL patch
- Checking that nothing has broken
- Shutting down TaleSpire and the server but leaving the DB and files up.
- Switching TaleSpire and server to the new branches
- Starting up the new TaleSpire and server builds.
- Checking that everything still works

This is now working well, so I'm feeling pretty happy. I still need to finalize some details with Ree when I meet up with him[7], but maybe we can push the DB patch next week. This gives us what we need to progress on things like polymorph, persistent emotes, 8 stats per creature, and more.

Right, I should get some sleep.

Have a good one!

[0] All of this has been done on a local build of our backend, so I wasn't risking messing stuff up.
[1] Uniques creatures are stored in the DB instead of being packed with the non-uniques in a file. So to change what data is stored for creatures means changing both places.
[2] In some cases, it's nice not to be allocating another object.
[3] Seriously - Low-level languages need to steal it. There is some general erlang'y ugliness to it, but the idea is excellent.
[4] I also verified them using `select typname, typelem from pg_type`
[5] Erlang, like many venerable languages, grew up before the modern culture around documentation. Common-lisp felt very similar, and you have to get decent at reading other people's code to learn how to use many things.
[6] with a few hacks in TaleSpire as I haven't finished the polymorph code yet.
[7] We have to nail down the data we need for persistent emotes.
