---
title: Rails I18n and 404/500 Error Pages
author: peter
layout: post
categories:
  - Ruby on Rails
  - Services
---
If you have a multilingual site it makes sense for your 404 and 500 error pages to be multilingual as well. Rails supports multilinguality by serving up error pages with a path like public/500.sv.html, where sv is the locale. In the case where Rails itself is fundamentally broken or fails to respond your webserver will respond with public/500.html, so we need that file as well. A way to achieve this is to keep your 404 and 500 pages as ERB templates with I18n.translate lookups under app/views/errors and have a Rake task that generates the static files under public:



That Rake task can then be invoked by Capistrano in an after\_update\_code deploy hook to make the pages available in production.