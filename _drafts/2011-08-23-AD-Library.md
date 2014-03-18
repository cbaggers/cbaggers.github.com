---
layout: post
title: AD Library
description: 
category:
tags: ['']
---

Need to list the immediate children of a ou in AD?
In my library it is:
<pre class="brush: python;"> 
import AD
ad = AD.ADEntry("work.co.uk","cbaggers")
ou = ad.find_ou("finance_dept")
print ou.children
</pre>
<pre class="brush: python;"> 
Under Tim Golden's library its:
import active_directory
o=active_directory.find_ou("finance_dept")
for i in o:
	print i
</pre>

I'm not sure if I prefer his way....it does make a good deal of sense I guess as children are 'in' the ou, and in his case you can't retrieve them by index as that is used for attributes (which is how I have done mine)...I could just implement an __iter__ method and it would behave the same way.
I'll have a ponder on this and get back to you.