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

{% highlight2 console %}
-- Order by one indexed column (FAST)
newsdesk_production=# explain analyze select * from pressreleases order by published_at DESC limit 100;
                                                                                   QUERY PLAN                                                                                    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Limit  (cost=0.00..249.91 rows=100 width=1207) (actual time=26.070..716.453 rows=100 loops=1)
   ->  Index Scan Backward using pressreleases_published_at_index on pressreleases  (cost=0.00..964766.62 rows=386042 width=1207) (actual time=26.067..716.343 rows=100 loops=1)
 Total runtime: 716.709 ms
(3 rows)

- Order by two separately indexed columns (SLOW)
newsdesk_production=# explain analyze select * from pressreleases order by published_at DESC, id DESC limit 100;
                                                              QUERY PLAN                                                               
---------------------------------------------------------------------------------------------------------------------------------------
 Limit  (cost=73343.67..73343.92 rows=100 width=1207) (actual time=6644.005..6644.024 rows=100 loops=1)
   ->  Sort  (cost=73343.67..74308.77 rows=386042 width=1207) (actual time=6644.004..6644.012 rows=100 loops=1)
         Sort Key: published_at, id
         Sort Method:  top-N heapsort  Memory: 170kB
         ->  Seq Scan on pressreleases  (cost=0.00..58589.42 rows=386042 width=1207) (actual time=0.028..6445.128 rows=385821 loops=1)
 Total runtime: 6644.102 ms
(6 rows)

-- Creating composite index
newsdesk_production=# create index pressreleases_id_published_at_index on pressreleases (published_at, id);
CREATE INDEX

- Order by two columns in composite index (FAST)
newsdesk_production=# explain analyze select * from pressreleases order by published_at DESC, id DESC limit 100;
                                                                                    QUERY PLAN                                                                                    
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Limit  (cost=0.00..267.80 rows=100 width=1207) (actual time=0.145..1.955 rows=100 loops=1)
   ->  Index Scan Backward using pressreleases_id_published_at_index on pressreleases  (cost=0.00..1033215.58 rows=385821 width=1207) (actual time=0.143..1.926 rows=100 loops=1)
 Total runtime: 2.009 ms
(3 rows)

- Order by two columns in composite index but in the wrong order (SLOW)
newsdesk_production=# explain analyze select * from pressreleases order by id DESC, published_at DESC limit 100;
                                                              QUERY PLAN                                                              
--------------------------------------------------------------------------------------------------------------------------------------
 Limit  (cost=73333.01..73333.26 rows=100 width=1207) (actual time=532.632..532.666 rows=100 loops=1)
   ->  Sort  (cost=73333.01..74297.56 rows=385821 width=1207) (actual time=532.631..532.649 rows=100 loops=1)
         Sort Key: id, published_at
         Sort Method:  top-N heapsort  Memory: 177kB
         ->  Seq Scan on pressreleases  (cost=0.00..58587.21 rows=385821 width=1207) (actual time=0.008..407.786 rows=385821 loops=1)
 Total runtime: 532.842 ms
(6 rows)
{% endhighlight2 %}

 [1]: http://www.postgresql.org/docs/8.4/static/queries-order.html