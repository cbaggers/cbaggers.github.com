---
layout: post
title: Eclipse project overlaps error
description:
date: 2014-05-05 14:02:42
category:
tags: ['eclipse', 'android']
---

I tried adding an existing android project today and got this error from Eclipse.

    “<name of project> overlaps the location of another project..”

It was reported as a project desciption problem. Turns out eclipse doesnt like importing projects from the direcotry it has designated as the workspace. Move the project outside that folder (or change the workspace directory) and try again.

What a pain in the arse!