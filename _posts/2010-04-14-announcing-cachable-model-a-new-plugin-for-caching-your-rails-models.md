---
title: 'Announcing Cachable Model &#8211; A New Plugin For Caching Your Rails Models'
author: peter
layout: post
categories:
  - Ruby on Rails
tags:
  - plugins
  - Ruby on Rails
---
When you scale a Rails website the database layer tends to become the bottleneck sooner or later as it’s more difficult to scale horizontally with hardware than the Ruby/web server layer. At MyNewsdesk we have started looking at database caching in the model layer to offload our PostgreSQL database. Unfortunately caching does no longer seem to be in vogue in the Rails community and plugins related to model caching such as Cache Money, Cached Model, and Cache Fu are all dated and do not use the Rails.cache object available for in Rails as of version 2.1. Caching is complicated by the fact that our queries do complex joins and therefore have intricate dependencies which makes flushing the cache difficult since we require that what we display on the site is always in sync with the database. As a first stab we decided to take the Cached Model approach of caching primary key single record lookups and reserve more aggressive caching for a few models that change very rarely. We’ve put together our work in a new plugin called [Cachable Model][1] and we hope you find it useful.

 [1]: http://github.com/mynewsdesk/cachable_model