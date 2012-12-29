---
title: Migrating RSpec to Mocha
author: peter
layout: post
categories:
  - Ruby on Rails
tags:
  - mocha
  - rspec
---
I have been using the builtin RSpec mocking framework for a couple of years and haven’t had any issues with it. The one feature I have been missing though is the any_instance method of the [Mocha][1] framework. What this method lets you do is mock a method on all instances of a given class. This is useful is you can’t get to the instance that you want to mock from your tests. It might be user object of the logged in user in a controller test for example. I also needed this feature when testing route generation since our routes depend on the subdomain of the host and the routing assertions in Rails will create a request object on the fly that I can’t get to.

Configuring RSpec to use Mocha is a oneliner. Since the Mocha DSL differs slightly from the RSpec DSL though the switch got a little more involved than that. Below are some notes that outline the details. Most noteworthy is that Mocha doesn’t support dynamic return blocks. This means you can’t mock/stub a method to execute a piece of code that takes the arguments of the method. What you need to do instead is map every expected set of argument values to a single return value. I suppose this works for most cases. James Mead intentionally designed the Mocha API with this limitation for what seems to be ideological reasons. His explanation is that being able to mock methods with code blocks [“encourages the use of complex logic for computing return values which is not sensible mocking practice”][2]. Maybe it is, maybe it isn’t. What I do know is that Mocha is an example of a really beautiful Ruby DSL. Don’t you agree?

 [1]: http://mocha.rubyforge.org/
 [2]: http://rubyforge.org/pipermail/mocha-developer/2007-June/000358.html