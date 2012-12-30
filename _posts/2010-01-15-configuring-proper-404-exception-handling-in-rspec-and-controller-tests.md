---
title: Configuring Exception Handling in RSpec and Controller Tests
author: peter
layout: post
categories:
  - Ruby on Rails
  - Services
---
The ActionController::Rescue module in Rails defines a number of exceptions that will trigger a 404 response, namely ActionController::RoutingError, ActionController::UnknownAction, and ActiveRecord::RecordNotFound. However if you trigger one of those exceptions in a controller test (Test::Unit or Rspec) then rescue and display of the 404 page will not happen and instead the exception will just be propagated. If you prefer checking the 404 response code in your tests over catching one of the exceptions then you can make use of the method ActionController::TestCase#rescue\_action\_in\_public!. The method will set the request.remote\_addr to something other than 0.0.0.0 and this will trigger real exception handling. In RSpec you can invoke the method in your spec_helper.rb like this:

{% highlight2 ruby %}
config.before(:each, :behaviour_type => :controller) do
  rescue_action_in_public!
end
{% endhighlight2 %}

One thing to note about this approach is that it means all exceptions will be caught. Sometimes it may be preferable to let the exceptions propagate so that you are able to assert in your tests which exception class should be raised. Even with exception handling activated though, you can still access the exception that was raised by invoking controller.send(:exception) in your tests.