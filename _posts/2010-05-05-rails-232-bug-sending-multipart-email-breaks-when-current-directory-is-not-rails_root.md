---
title: 'Rails 2.3.2 Bug &#8211; Sending Multipart Email Breaks When Current Directory is not RAILS_ROOT'
author: peter
layout: post
categories:
  - Ruby on Rails
---
We had the debugging session of the year here at Newsdesk the other day and we thought it would be prudent to warn other people about it. It turns out that ActionMailer in Rails 2.3.2 fails in sending multipart emails when the current working directory is not Rails.root. This happened to us when invoking the mailer from a cronjob.

The bug has [been reported][1] and solved with two similar patches. One patch is by Tom Lea and it has been committed and is available in the latest Rails 2 version (Rails 2.3.5). The issue stems from ActionMailer::Base.template\_root not being a string (as you may naively believe when you print it out) but rather a subclass of ActionView::Template::Path. The Path class will return the relative path of the template when to\_s is invoked but the absolute path when to_str is invoked. Others have tried to figure out how [to\_s and to\_str are supposed to be used][2] – two small and confusingly similar Ruby methods.

It turns out that when you interpolate the Path object into a string with “#{path}/#{filename}” (Rails before the patch), then to\_s is invoked. When you use File.join(path, filename) then to\_str is used (Rails after the patch).

With to\_str and the absolute path in place you no longer have to stand in the RAILS\_ROOT for ActionMailer to work. Cause for celebration…

 [1]: https://rails.lighthouseapp.com/projects/8994/tickets/2263-rails-232-breaks-implicit-multipart-actionmailer-tests
 [2]: http://briancarper.net/blog/ruby-to_s-vs-to_str