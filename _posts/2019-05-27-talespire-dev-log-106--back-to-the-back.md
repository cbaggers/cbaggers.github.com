---
layout: post
title: TaleSpire Dev Log 106 - Back to the back
description:
date: 2019-05-27 20:06:37
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today started off with looking into how we are going to handle copying and sharing boards. We store all the board files on S3 so the first part of the job was to look into how to copy/delete etc files on s3 from erlang.

I've been using this nice little library called [mini_s3](https://github.com/chef/mini_s3) so far. It has done all the hard work but there are a few places where certain options have not been handled. For example the ec2 instance gets temporary permission to s3 from it's IAM role and so when we make requests we need to pass along the `x-amz-security-token`. The functions in `mini_s3` don't handle that yet.

I have [forked the library](https://github.com/cbaggers/mini_s3/commits/master) and have added support for the amz token to `s3_url` and `list_objects`. I'll be adding support to more functions soon but `list_objects` served as a good test.

For the rest of the day I was working on some other experiments that aren't worth rambling about.

Seeya in the next dev log
