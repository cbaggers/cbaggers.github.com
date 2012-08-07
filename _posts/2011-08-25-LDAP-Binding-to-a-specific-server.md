---
layout: post
title: LDAP and Binding to a specific server
description: 
category:
tags: ['python']
---

Hey all,

I was looking into the <a href="http://support.microsoft.com/kb/891995">uSNChanged</a> value in ldap and saw that (at least in AD) it is not replicated. This meant I would have to query each server individually...then I realised I didnt know how!...turns out I've been connecting just by domain, so a quick google later and I found <a href="http://blogs.technet.com/b/heyscriptingguy/archive/2005/05/17/how-can-i-access-active-directory-on-a-specific-domain-controller.aspx">this</a>:



<pre class="brush: python;">

	"LDAP://servername/dc=work,dc=co,dc=uk"

</pre>



Nice!