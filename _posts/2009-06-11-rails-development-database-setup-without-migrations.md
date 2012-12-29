---
title: Rails Development Database Setup Without Migrations
author: peter
layout: post
categories:
  - Ruby on Rails
tags:
  - Ruby on Rails
---
I think a lot of Rails developers have been using the db:migrate Rake task to setup new development databases. The problem with this is that over time migrations tend to grow out of sync with the code, especially if you use model classes in your migrations (which in some cases can be very convenient and DRY). A more robust approach to setting up a development database is to load the schema and seed it with data like the db:setup Rake task in Rails 3 does. Now there are a couple of issues for us in using this approach. First we are not on Rails 3 yet (obviously), second we use schema format SQL so the db:schema:load task doesn’t work (see [my patch for fixing this][1]), and third, we don’t have a proper db/seed.rb file yet for setting up all the data that so far has been set up by our migrations. To work around these issues I came up with a task [like this][2]. The db/development\_data.sql file was created by invoking the db:structure:dump task and exchanging the -s option to pg\_dump with -a for data only.

 [1]: https://rails.lighthouseapp.com/projects/8994/tickets/2789-make-rake-task-dbschemaload-compatible-with-sql-schema-format#ticket-2789-1
 [2]: http://gist.github.com/127891