---
title: Building a Hugo Blog on Firebase and GCP
description: Sellout Yan using his company's products :)
toc: true
authors:
  - Ben
avatar: images/profile.jpg
tags:
  - CI/CD
  - GCP
  - Firebase
  - Hugo
categories:
  - CI/CD
  - GCP
  - Firebase
  - Hugo
series:
  - 
date: '2022-05-20'
lastmod: '2022-05-20'
featuredImage: images/logo.png
draft: false
---

_After putting this off for far too long, we're finally live!_

# Introduction
Hello! I finally sat down and made myself get this blog off the ground, which means we may as well write a bit about the whole process.

Starting out with a blank sheet, I knew that I wanted to do (and not do) a few things:

1. Host the thing on GCP
2. Not touch frontend with a 10 foot pole
3. CI/CD the thing

This isn't really anything groundbreaking or crazy, but looking back at the end of getting this doodad working there's a bit of nuance involved with each of the steps. Let's take a look. 

# Hosting a static site on GCP



<div class="warning" style='padding:0.1em; background-color:#E9D8FD; color:#69337A'>
<span>
<p style='margin-left:1em;'>
yo why tf 
</p>
<p style='margin-top:1em; margin-bottom:1em; margin-right:1em; text-align:right; font-family:Georgia'> <b>- Gary Provost</b> <i>(100 Ways to Improve Your Writing, 1985)</i>
</p></span>
</div>