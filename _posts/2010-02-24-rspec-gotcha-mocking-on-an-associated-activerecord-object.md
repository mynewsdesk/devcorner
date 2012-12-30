---
title: 'Rails Gotcha: Mocking/Stubbing on an Associated ActiveRecord Object'
author: peter
layout: post
categories:
  - Uncategorized
---
In RSpec it is still not possible to have mocks or stubs on all instances of a class, although this [has been suggested][1] and it is a feature of the Mocha framework. Stubbing or mocking an object returned by an ActiveRecord belongs\_to association (i.e. article.author) won’t do what you expect. Instead of mocking the associated object you end up mocking the AssociationProxy object that wraps the associated object. To work around this you can use the AssociationProxy#proxy\_target method, something like this:
{% highlight2 ruby %}
describe "Mocking/Stubbing Associations with RSpec" do
  it "can be done with proxy_target" do
    # Suppose the Article model has an after_save callback that invokes notify_change on the author
    # if the article has been changed.
    @article.author.proxy_target.should_receive(:notify_change)
    @article.update_attributes(:body => "new body")
  end
end
{% endhighlight2 %}


So when we think we are working directly with an ActiveRecord model, we are in fact working with a proxy object, and this can be confusing:
{% highlight2 ruby %}
class Author
  def invoke_inside(method, *args)
    self.send(method, *args)
  end
end

author = Article.first.author
def author.foobar
  "foobar"
end

author.object_id                              # => 9969140
author.respond_to?(:foobar)                   # => true
author.class                                  # => Author
author.invoke_inside(:object_id)              # => 9952490
author.invoke_inside(:respond_to?, :foobar)   # => false
{% endhighlight2 %}


As far as I can tell this behaviour won’t change with Rails 3 since its AssociationProxy class works the same way.

 [1]: https://rspec.lighthouseapp.com/projects/5645-rspec/tickets/28-6791-stub-return-values-for-all-instances-of-a-class#ticket-28-2