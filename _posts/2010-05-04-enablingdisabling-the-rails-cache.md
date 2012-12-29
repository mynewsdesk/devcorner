---
title: Enabling/Disabling the Rails Cache
author: peter
layout: post
categories:
  - Ruby on Rails
---
We needed a way to globally enable/disable the Rails cache so we came up with this patch:



By default ActionController will use the Rails.cache (RAILS\_CACHE) object configured by config.cache\_store for its fragment caching (action caching is an around filter that utilizes the fragment cache). However, it is possible to configure a different cache store via ActionController::Base.cache_store. If you do that you’ll end up with two different cache stores each with its own independent enabled attribute. For the sake of simplicity I try to make sure we stick to a single cache store object in our application, namely MemCacheStore.