---
title: Committing to Rails Core
author: Richard
layout: post
categories:
  - Uncategorized
---
In our last post we talked about [upgrading to Rails 2.3][1], and the issues we encountered. One bug we found was that multiple POST requests issued in a test didn’t reset the post parameters between requests. [The patch Peter submitted][2] is now [part of the Rails core][3], and we are very happy about it.

Perhaps not because we Saved the Rails Community with this minor feature, but our ambition at Newsdesk has always been to contribute actively to the open source community. And this is our first contribution to Rails!

[Peter talks more about this][4] in his private blog.

 [1]: /2009/03/17/upgrading-to-rails-23/
 [2]: http://rails.lighthouseapp.com/projects/8994/tickets/2271-reset-request_params-in-testrequestrecycle#ticket-2271-1
 [3]: http://github.com/rails/rails/commit/8fa4275a72c334fe945dada6113fa0153ca28c87
 [4]: http://marklunds.com/articles/one/407