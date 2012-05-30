---
layout: post
title: Exchange Autoforwarding
description: 
category:
tags: ['']
---

Look like we have a good number of people who need Auto forwarding removed from their account: This is <a href="http://osherove.com/blog/2007/8/23/auto-forward-emails-from-exchange-to-your-gmail-or-other-ema.html">easily done</a> through AD Users and Computers on Windows (though in that link where it says 'Mail Delivery' it should be 'Delivery Options' ) However it looks like this will be very easy through LDAP also as their is an attribute called 'altrecipient' which returns the distinguished name of the contact in AD. I'll ensure I wrap this in my library, might be a job for idle time tonight!

Anyhoo back to business!