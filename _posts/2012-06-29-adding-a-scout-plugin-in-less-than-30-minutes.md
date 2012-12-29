---
title: Adding a Scout plugin in less than 30 minutes
author: Kristian Hellquist
layout: post
categories:
  - Ruby on Rails
tags:
  - Delayed Job
  - Ruby on Rails
  - Scout
---
On mynewsdesk we send a lot of emails as discussed in a [previous post][1]. We have a queue for outgoing email deliveries. Depending on our customer’s plan, the emails are sent with different priority using delayed_job.

Customer Support asks us about the status in the different queues “When will customer X’s email be sent?”. We resolve this question by doing manually SQL-queries against the delayed job table. A boring, tedious task… Scout to the rescue!

With Scout we can easily provide Customer Support with this info right away by creating a plugin. And it’s super easy!

I looked into the offical delayed\_job-plugin, tweaked it a bit, uploaded it to our utility-server (which runs delayed\_job). Scout automatically found it, and we could start using the plugin right away! See instructions for uploading your plugin [here][2].

![Number of pending mail jobs for different customer types](/images/wp/scout-plugin.png "Number of pending mail jobs for different customer types")

The source code for the plugin can be found [here][3].

 [1]: /2012/06/08/migrating-to-sendgrid/ "Migrating to Sendgrid"
 [2]: https://scoutapp.com/info/creating_a_plugin#run_plugin "How to activate your plugin in production environment"
 [3]: https://github.com/mynewsdesk/scout-email-dist-plugin "Source code"