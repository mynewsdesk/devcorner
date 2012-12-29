---
title: 'Rails Gotcha: Mocking/Stubbing on an Associated ActiveRecord Object'
author: peter
layout: post
categories:
  - Uncategorized
---
In RSpec it is still not possible to have mocks or stubs on all instances of a class, although this [has been suggested][1] and it is a feature of the Mocha framework. Stubbing or mocking an object returned by an ActiveRecord belongs\_to association (i.e. article.author) won’t do what you expect. Instead of mocking the associated object you end up mocking the AssociationProxy object that wraps the associated object. To work around this you can use the AssociationProxy#proxy\_target method, something like this:



So when we think we are working directly with an ActiveRecord model, we are in fact working with a proxy object, and this can be confusing:



As far as I can tell this behaviour won’t change with Rails 3 since its AssociationProxy class works the same way.

 [1]: https://rspec.lighthouseapp.com/projects/5645-rspec/tickets/28-6791-stub-return-values-for-all-instances-of-a-class#ticket-28-2