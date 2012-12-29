---
title: 'Rails bug fix: ActiveRecord::Base#exists? now compatible with include scopes'
author: peter
layout: post
categories:
  - Ruby on Rails
---
I was using a default scope for a model with :include and <img src='http://devcorner.mynewsdesk.com/wp-includes/images/smilies/icon_surprised.gif' alt=':o' class='wp-smiley' /> rder options and ran into [an issue with the exists? method][1]. We have now refactored the exists? method to invoke find_initial which handles scopes correctly. Thanks Michael for [committing this!][2]

 [1]: https://rails.lighthouseapp.com/projects/8994-ruby-on-rails/tickets/2543-make-activerecordbaseexists-invoke-find_initial-to-support-include-scopes#ticket-2543-2
 [2]: http://github.com/rails/rails/commit/afcbdfc15f919a470e4cfca97fb0084eebd2ab1f