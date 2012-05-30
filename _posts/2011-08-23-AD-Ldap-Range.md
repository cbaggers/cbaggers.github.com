---
layout: post
title: AD, Ldap, Range
description: 
category:
tags: ['active directory']
---

When using ldap to interact with Active Directory you can sometimes run into an odd issue with ranges.



Say for example you have a group with more than 1500 members. When you query the properties of the group where you would normally see 'member' you see 'member;range=0-1499' so you only get the first 1500 results.

 

Apparently you can change this setting on the server but odds are that you won't be allowed to do this just for your scripting pleasure!

To remedy the issue, check out <a href="http://stackoverflow.com/questions/1000541/ldap-ad-range-attribute-how-to-use-it">this great function on stackoverflow</a>


