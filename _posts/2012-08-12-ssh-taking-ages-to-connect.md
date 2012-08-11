---
layout: post
title: SSH Taking ages to connect?
description:
category:
tags: ['fix','ssh']
--- 

I had an interesting little issue the other day where connecting to some servers was taking aroudn 15 seconds before showing a login prompt. Everything after that was speedy so I knew it wasnt going to be a connection time issue. 

I ran the following to get verbose connection info:
    ssh -v servername

and saw this error:
    debug1: Authentications that can continue: publickey,gssapi-with-mic,password
    debug1: Next authentication method: gssapi-with-mic
    debug1: An invalid name was supplied
    Cannot determine realm for numeric host address

It turns out that ssh is trying to use Kerberos (GSS-API). To turn this off we can simply edit our ssh config file.

   # open up ~/.ssh/config
   # add the following line
   GSSAPIAuthentication no

And that fixes it!