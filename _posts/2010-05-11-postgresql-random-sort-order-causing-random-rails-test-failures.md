---
title: PostgreSQL Unreliable Default Sort Order and Random Rails Test Failures
author: peter
layout: post
categories:
  - Ruby on Rails
---
We have been struggling with intermittent Rails test failures for quite some time. There were times when we blamed our search engine Ferret, sometimes we blamed database interference between tests, and sometimes higher powers. Lately we have realized that all this time the ghost haunting us might have been the unreliable PostgreSQL default sort ordering. Here is what the [docs][1] have to say:

> “If sorting is not chosen, the rows will be returned in an unspecified order. The actual order in that case will depend on the scan and join plan types and the order on disk, but it must not be relied on.”

Suppose you have a test that asserts that when requesting a certain action, then Article.first or @user.articles.first is assigned in the controller. This test may work five times, or a hundred times, or maybe even a thousand times before it fails. Maybe it so happens that your test fails when you run your full test suite and then succeeds when you run the test separately. Needless to say this can become frustrating very fast.

There is some inconsistency in ActiveRecord when it comes to first and last. ActiveRecord::Base#first will do a “SELECT * FROM “pressreleases” LIMIT 1″ (unordered and thus unreliable) but ActiveRecord::Base#last issues an “SELECT * FROM “pressreleases” ORDER BY pressreleases.id DESC LIMIT 1″ (ordered and reliable).

Note that the danger with unreliable PostgreSQL default sort ordering also applies when you are ordering by something that isn’t guaranteed to be unique, such as a publish date.

The solution we came up with was to issue a double sort order “published\_at DESC, id DESC”. Unfortunately the drama escalated since we had forgotten to create a composite index on published\_at and id leading to the database locking up and average response time going from 25 ms per transaction to 100 ms. The importance of composite indexes for ordering on multiple columns is illustrated by this example (note that the ordering of columns needs to match for the index to apply):

 [1]: http://www.postgresql.org/docs/8.4/static/queries-order.html