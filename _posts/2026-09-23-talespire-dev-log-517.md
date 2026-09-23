---
layout: post
title: TaleSpire Dev Log 517
description:
date: 2026-09-23 17:54:14
category:
tags: ['Bouncyrock', 'TaleSpire']
---

The last few days, I've been working on the connection issues we mentioned in [recent posts](https://bouncyrock.com/news/articles/update-on-isps-blocking-talespire-good-news).

After reaching out to various ISPs, we've managed to get most folks back playing. However, one of our players in Switzerland still had an issue because DNS requests for our subdomains were returning incorrect addresses.

It's very odd, especially since it started so close to the other ISP issues. However, the folks at Sunrise GmbH claim they don't have anything affecting DNS lookups, so I've taken another route to resolve the issue.

I've added a fallback to our DNS lookups that uses Cloudflare's public [DNS over HTTPS](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/) service if the default lookup fails. We could technically use it as the primary system, but we don't want to risk affecting anything that already works, so this seems like the safest option.

I'll be testing this tomorrow, and if all goes well, it should ship on Monday.

At that point, I'll be getting back to work on shadows, where I'm currently reworking our frustum-culling compute-shaders as I prepare to move shadow culling to the GPU as well.

Ciao.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
