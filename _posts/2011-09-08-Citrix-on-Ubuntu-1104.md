---
layout: post
title: Citrix on Ubuntu 11.04
description: 
category:
tags: ['']
---

Hey folks,

I'm slapping this here as its may save a couple of minutes of googling in the future.



- <a href="http://www.citrix.com/English/ss/downloads/details.asp?downloadId=2309164&amp;productId=1689163&amp;ntref=clientcenter">Grab the citrix ICA client</a>

- sudo apt-get install libmotif-dev

- Add   net.ipv4.tcp_window_scaling=0  to your /etc/sysctl.conf file to fix a strange popup error



Run free in the bountiful citrix'y hills!



Cheers to <a href="http://karuppuswamy.com/wordpress/2010/05/06/how-to-solving-libxm-so-4-error-in-citrix-receiver-icaclient-on-ubuntu/">this site</a> and <a href="http://joepcremers.com/wordpress/connect-to-citrix-access-gateway-on-ubuntu-9-10/">this site</a> for the fixes.



Ciao!