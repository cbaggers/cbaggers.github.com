---
layout: post
title: Quick and Dirty query to find out what instances of SQL a Windows box has (if any)
description: 
category:
tags: ['']
---

Needed something to do this from Linux quickly. 



net rpc service list -W domain.work.org -S servername -U domain\\username | grep "SQL Server ("



Hope it helps