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
featuredImage: images/logowhugo.png
draft: false
enableMath: false
---

_After putting this off for far too long, we're finally live!_

# Building this Site

Hello! I finally sat down and made myself get this blog off the ground, which means we may as well write a bit about the whole process.

Starting out with a blank sheet, I knew that I wanted to do (and not do) a few things:

1. Host the thing on GCP (_eat your own dogfood_)
2. Not touch frontend with a 10 foot pole (_no js pls god_)
3. CI/CD the thing (_best practices1!!_)

None of this is anything groundbreaking or crazy, but looking back at the end of getting this doodad working there's a bit of nuance involved with each of the steps. Let's take a look.

## Hosting a Static Site on GCP

### HTTPS Cloud Load Balancer = $$$

If you follow the [GCP-provided guide](https://cloud.google.com/storage/docs/hosting-static-website), hosting a HTTPS-enabled static site in GCP seems easy enough. Create a bucket, attach it to a load balancer, front it with Cloud CDN, and request a HTTPS certificate **until** you realize that an external HTTPS load balancer costs $0.025/hr (~$18.75/mo), which balloons monthly costs to _several_ orders of magnitude greater than competing options in Azure and AWS. Blob storage hosting costs between the 3 CSPs are within rounding errors of each other $.015-.025/GB/mo, data egress costs are negligible), which means the _entirety_ of the price delta comes from this external load balancer requirement.

If I were to take a NDA-friendly guess, this HTTPS Load Balancer requirement is necessary for GCP to spin up a listener on [GFE](https://cloud.google.com/docs/security/infrastructure/design#google_front_end_service) to proxy your requests from `https://mydanksite.com` to the desired GCS backend, which only then can this ELB be fronted by a CDN deployment.
<style>
    .div-1 {
        background-color: #EBEBEB;
    }
    
    .warning {
    	padding:0.1em; 
      background-color:#fce8e6; 
      color:#d50000; 
      margin:0.5em; 
      border-left:5px solid;
    }
    
    .div-3 {
    	background-color: #FBD603;
      
    }
</style>

<div class="warning" >
<span style='border-left-color:#d50000;'>
<p style='margin-left:1em;margin-right:1em;margin-top:1em;margin-bottom:1em;'>
Regardless of the technical reason for requiring HTTPS Cloud Load Balancing, it's a bit off-putting that the developer experience (especially for say, a student) for hosting a static site is not as straightforward as S3-CloudFront or Azure Storage-CDN.
</p>
</span>
</div>

Of course, there's caveats to this too, you could [map App Engine with a custom domain](https://cloud.google.com/appengine/docs/standard/python/mapping-custom-domains) and use [Firebase custom domains with Cloud Run](https://cloud.google.com/run/docs/mapping-custom-domains) but neither are immediately obvious and require a bit more tinkering.

### Firebase 
So instead we're using Firebase! It's not as 