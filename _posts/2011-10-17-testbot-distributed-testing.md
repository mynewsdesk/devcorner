---
title: 'Testbot &#8211; Distributed testing'
author: Kristian Hellquist
layout: post
categories:
  - Ruby on Rails
  - Testing
tags:
  - Ruby on Rails
  - testbot
  - testing
---
# TL;DR

If you have a lots of tests in a rails app that is annoyingly slow (+10 minutes), try testbot – a tool for distributed testing

# Testbot

## What?

[Testbot][1] is a tool for running tests distributed over several machines. Mynewsdesk test suite, ~1900 examples and growing strong, takes about 20 minutes when running on my local machine (a weak macbook air). By using testbot the suite takes about 3 minutes. Thats almost an performance win by 700%! Costs money though (ducks!)

## How?

Testbot is composed by three parts.

### Requester

The requester is the local machine that initializes the test-suite to be run with testbot. The requester will send its environment (rsync) to the master and waits for the test suite to be run.

### Master

The master will split the test-suite into smaller parts and let runners set up the environment (rsync, bundle install) and run the subset of tests. The results from the runner will be sent back to the requester.

### Runner

The runner is our work horse that will run a group of tests and sends the result (stdout) back to the master.

For more a detailed explanation see testbot’s [documentation][2].

## Our setup

We are using AWS EC2 to run our master(1) and runners(11). All our instances are of the type medium (2 cores, 1.7 GB memory)

### Our requester

Every developer’s own computer

### Our master

Some standard linux image offered by ec2. The testbot-master requires ruby and the testbot gem installed. For us, this machine also has a cron script that schedules when runners are to be run (office hours)

### Our runner

This machine has all the required software to run our rails application in a test environment. Our rails application depends on solr and postgres, so this software is also installed.

### How fast can the test suite run?

Distributed testing allows you to increase the number of runners to decrease the time it takes to run the whole test suite.

Bottlenecks for testbot is the time it takes to rsync the environment from the requester to the master, then rsync the environment from the master to the runner and lastly the time it takes for the runner to start the rails environment and run X number of tests.

For us this means that it should be theoretically possible to run our whole test-suite in seconds(!).

## Our gotchas

### Environment setup for the runners

In the beginning the runners didn’t run in a stable environment. For example solr didn’t always startup properly and we had to use monit to ensure solr was running.

### Cache store

During our upgrade to rails 3 we got errors when reading from Rails.cache

> Errno::ENOENT:  
> No such file or directory – /tmp/testbot/mynewsdesk/tmp/  
> cache/Site.local_sites20111013-3227-17jr2c4-0.lock

The problem could be avoided by using memory\_store instead of file\_store.

### “It only fails on testbot!11! FUUU”

We have had quite a lot of tests that only have failed on testbot. Almost all these errors can be traced back to the fact that the test doesn’t start in the state that we expect. Some test changes a global variable/database-table but doesn’t resets the environment when the test is finished (side-effect™). Similar to this is that a failing test doesn’t ensure a clean environment when test starts.  
Failures due to incorrect data in solr is one of our classics.

## Tips

### Simplify the process of updating runners

Testbot will handle your gem changes automatically if your are using bundler. Prepare yourself to make changes outside the rails stack smoothly. EC2 AMI’s functionality will solve this problem for you. Chef or Puppetmaster is an even better strategy if you someday want to migrate to another plattform.

### Split big tests into smaller

Large test files will limit how fast your test-suite can run with testbot. If a single test file takes two minutes, your test-suite can never be faster than two minutes regardless of how many runners you add.

## Final words

A lot of rails project would definitely benefit from testbot. Is there any other distributed test frameworks that are awesome?

 [1]: https://github.com/joakimk/testbot "testbot"
 [2]: https://github.com/joakimk/testbot/blob/master/README.markdown