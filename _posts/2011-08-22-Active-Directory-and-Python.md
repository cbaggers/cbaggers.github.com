---
layout: post
title: Active Directory and Python
description: 
category:
tags: ['active directory', 'python']
---

At work we are a Windows house and as such behind most everything is Active Directory.
As those who have lost handfuls of hair will attest, the standard tool is very lacking as soon as you need to do any real bulk job. I've seen my boss delighted at a GUI tool we recently acquired which allows him to do batch queries and changes in AD as he has previously gone through hundreds of entries making manual changes (please note he's not a dumb guy...but he's not got a head for scripting). 
I, however, live with a python shell open most of the day and so obviously I went to see if I could get a library which would let me bend AD to my will. There is...it's awesome...Tim golden is a hero!..Check it out <a href="http://timgolden.me.uk/python/active_directory.html">here</a>. This library has let me perform some seriously extensive jobs on all areas of AD and let me write Windows tools to automate some of what we do. 
The only tiny downside for my is that it uses pywin32 under the hood and no amount of <a href="http://www.winehq.org/">'Wine'</a> will get it running under Ubuntu. As AD munging is one of the few things stopping me move to Ubuntu as my primary desktop at work I started having a dig at the python ldap library in my free time.
The python ldap guys and girls have done an awesome job making a very comprehensive and easy to use library.  I had however fallen in love with Tim's wrapper and so I set out to implement as similar as library as I could using ldap.
The process has been surprisingly easy, and I now have a library that works smoothly with AD. Here is a simple example of how to connect to the AD of domain "work.co.uk" and find user "cbaggers" and list what groups he's a member of:
<pre class="brush: python;"> 
import AD
ad = AD.ADEntry("work.co.uk","username","password")
u = ad.find_user("cbaggers")
print u.memberOf
</pre>
That't it! 
I'm pretty much finished with the code that pulls info (getting attributes) and have made sure that it translates the horrible windows time longs into python datetime objects. I've also fleshed out writing data back to ad for simple fields (anything string based is easy), but I still need to experiment with adding AD objects to groups, moving objects, creating new objects (users, groups and the like) and make sure its as tidy as I like. 
I'm going to get either a launchpad or bitbucket account setup soon to host the project (it'll be under the GPL) within the next few weeks.
I'll keep also you posted with the weird bits of AD and ldap (there are a few!) so hopefully others can avoid the head scratching.
Goodnight all!