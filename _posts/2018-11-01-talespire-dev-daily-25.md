---
layout: post
title: TaleSpire Dev Daily 25
description:
date: 2018-11-01 21:50:42
category:
tags: ['BouncyRock', 'TaleSpire']
---

Well today was a major pain in the ass. It started ok, I was exposing the recent changes to the api but it all went south when I got to syncing board data. Up until I had been lazy and had been saving the boards in the database as a binary blob, not good practice I know but it let us get moving quickly. Now of course we need a better path and so it was time to do something, there were a couple of choices that seemed obvious.

### Sync board zones to database

This one is still keeping binary data in the db but we sync individual zones. This is cool as we can then pull data on demand which gets us in the direction of large board support. The issue though is that we don't need that feature for the alpha and it's a bit of an unknown what extra code we will need on the client side.

I don't imagine it would be hard, but that never stopped problems turning out to be a nightmare before :)

Seriously though, given how tight it's going to be to get the alpha out as it is we don't need more unknowns.

### Sync the whole board to S3

This one was the clear winner from the client side, we use the same code we have but upload to amazon s3 instead. However the unknown here was on the server side. I'm still kind of a noob with s3 and the erlang library ecosystem often puts you in a place where you need to cobble some things together.

This time I was lucky in that the folks who make Chef have a library called [mini_s3](https://github.com/chef/mini_s3). However, as documentation is often thin of the ground, I needed to read parts of Chef to understand how to use it.. and this is where the noob thing caused issues.

Almost the biggest issue when trying to learn from code you don't understand is that you have very poor metrics for separating things, it's hard to derive what is intentional and what is a hack and terminology doesn't yet give you the usual clues to help separate concepts. I went on a wild goose chase as I thought that Chef's OrgID & Checksums were intrinsic to the s3 code so I was reading far back into the codebase trying to derive how they were getting the values so I could work out to do for our code.

Eventually, after a lot of shouting, I stumbled on my mistake and got things working. It's painful as it was all for a single function, one that generates presigned urls to allow upload or download to a specific file on s3 for a limited period of time.

With that done I got back to what I though would get finished this morning, finishing exposing the new api functions and pushing the a version to staging.

That brings us to now. I need to do a little more testing (which I'll do now) and then I'll get back to hook this up to the engine.

Here's to tomorrow
