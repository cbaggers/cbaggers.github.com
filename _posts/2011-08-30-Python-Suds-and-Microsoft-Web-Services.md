---
layout: post
title: Python, Suds and Microsoft Web Services
description: 
category:
tags: ['python']
---

Hey all,

If you need to interact with Microsoft Web Services from python grab suds from whatever repo you have handy and relax! I had a brief dabble with Soappy  but it seems this is now deprecated and had a couple of issues.



Here is your quickstart to talk to a soap based .net web service with suds



<pre class="brush: python;">

from suds.client import Client

#chances are you have a asmx url, make sure you add

# ?wsdl on the end

url = "https://www.yourwebsite.org/yourwebservice/webservice.asmx?wsdl"

client = Client(url)

print client.service.yourMethod(args)

</pre>



Hope it helps!