---
title: 'Rails Gotcha: Global Methods can Cause ActiveRecord Attribute Methods to Never be Defined'
author: peter
layout: post
categories:
  - Ruby on Rails
---
I just ran into a sort of edge case gotcha in the interaction between the acts\_as\_taggable\_on Rails plugin and RSpec. It turns out both libraries define a context method. In the RSpec case it is a global method (defined in Object) and in the plugin case it is the Tagging model context attribute. As you may know ActiveRecord uses method\_missing to allow invocations such as Tagging.find\_by\_context(‘foobar’). However, method\_missing is also used to define attribute accessor methods (context, context= etc.) the first time you use them. The issue in my case was that the first Tagging model accessor that I invoked was context and since context was already defined in Object by RSpec, method\_missing was never invoked and the attribute accessors never defined.

I haven’t had time yet to think this issue through thoroughly, and I am no expert on ActiveRecord, but the most obvious solution seems to be to have the attribute accessors defined when an ActiveRecord::Base class is first loaded. Are there any potential drawbacks to this solution?