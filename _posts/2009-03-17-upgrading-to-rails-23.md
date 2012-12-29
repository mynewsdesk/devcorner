---
title: Upgrading to Rails 2.3
author: peter
layout: post
categories:
  - Ruby on Rails
---
We just upgraded our app from Rails 2.2.2 to Rails 2.3.2 and were able to get all our tests passing after having ironed out a number of deprecations, upgraded a few plugins, and patched a few others. Here is an outline of the changes we had to make:

- Rails has an issue with tests that issue multiple POST requests. We created [a patch for this][1].

- Changed patch of request object to use new class Rack::Request  
- Renamed app/controllers/application.rb to app/controllers/application_controller.rb  
- Changed Test::Unit::TestCase in test/test_helper.rb to ActiveSupport::TestCase. Changed Test::Unit::TestCase in test/unit/*.rb to ActiveSupport::TestCase and in test/functional/*.rb to ActionController::TestCase.  
- Upgraded will_paginate gem from 2.3.5 to 2.3.8  
- Removed session :disabled => true since sessions are lazy loaded now  
- Upgraded rspec and rspec-rails to 1.2.0, see http://wiki.github.com/dchelimsky/rspec/configgem-for-rails  
- Patched the acts\_as\_bitfield plugin. The ActiveRecord::Base#attribute_condition method now takes two arguments.  
- Added a bunch of new I18n keys:

+  support:  
+    array:  
+      last\_word\_connector: “, and ”  
+      sentence_connector: and  
+      skip\_last\_comma: false  
+      two\_words\_connector: ” and ”  
+      words_connector: “, ”  
+  time:  
+    am: am  
+    formats:  
+      default: “%a, %d %b %Y %H:%M:%S %z”  
+      long: “%B %d, %Y %H:%M”  
+      short: “%d %b %H:%M”  
+    pm: pm

- Fixed a failing test related to cookie handling.  
- Fixed test of Array#to_sentence that uses different I18n keys now

All in all we spent 2-3 hours or so on the upgrade so it wasn’t too bad, baring any severe issues that we haven’t found yet…

 [1]: http://rails.lighthouseapp.com/projects/8994/tickets/2271-reset-request_params-in-testrequestrecycle#ticket-2271-1