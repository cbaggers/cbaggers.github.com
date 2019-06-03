---
layout: post
title: TaleSpire Dev Log 110
description:
date: 2019-06-04 00:11:26
category:
tags: ['BouncyRock', 'TaleSpire']
---

Good-evening folks,

Today I spent most of my time working on the code that deletes old boards from the backend storage.

Every time you load a board TaleSpire takes a new snapshot of the board that you then modify during that session. We keep hold of these snapshots so that we can at some point give you the ability to roll back to one of these backups in case something drastic happens.

Of course this means we are slowly accruing more and more board snapshots so they have to be cleared up. The goal was to keep only the 10 most recent snapshots. Any older than that will be deleted.

Now even though a snapshot is old it would still be good to be able to recover it for a short window of time so we have set up 'buckets' on S3 which auto-delete files that have been in there more than 14days. This means the task is to identify the old board snapshots and move them to these buckets.

As this operation needs to touch every row in the 'board files' table in the database it could be rather costly so we are using postgresql's [materialized views](https://www.postgresql.org/docs/9.3/rules-materializedviews.html) to help with this. This lets us chose when to update the view at the cost of it being out of date after that. This is fine for us as the worst that happens is that we don't delete a few snapshots as soon as we could, it will never result in us deleting something we shouldn't.

After that it was a case of setting up the erlang process that would take care of this at regular intervals. I got the hard part done, namely identifying and moving the files, but for whatever reason the event isn't firing automatically. This will be some simple mistake on my part so I'm not worried about fixing that relatively soon.

This also means I've tested the bulk of the code I would need to implement 'duplicate board' so if all goes well I'll get that out this weekend.

Until next time,

Peace.
