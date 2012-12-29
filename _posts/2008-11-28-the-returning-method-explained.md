---
title: 'The &#8220;returning&#8221; method explained'
author: Richard
layout: post
categories:
  - Ruby on Rails
tags:
  - Ruby on Rails
  - tip
---
This one you will find all over the Rails code. The “returning” trick is a really neat way to refactor ugly patterns, such as this one:
{% highlight2 ruby %}
object = []
object << "foo" if bar?
object
{% endhighlight2 %}

Into this:

{% highlight2 ruby %}
returning [] do |object|
  object << "foo" if bar?
end
{% endhighlight2 %}

So what **returning** does is, it creates an object using the first parameter (a new array in the above example), yeilding it to the block passed, and returns it when done. It all becomes clear in this example:

{% highlight2 ruby %}
def Post.update_record(post_id)
  post = find(post_id)
  post.update_attribute(:foo, "bar")
  post
end
{% endhighlight2 %}

That ugly (well…) method finds an object, updates an attribute and saves it. But because the “save” method called by “update_attribute” just returns true or false, you will need to explicitly return the post object last in the method if you want to get it back. The **returning** approach just feels better for me:

{% highlight2 ruby %}
def Post.update_record(post_id)
  returning find(post_id) do |post|
    post.update_attribute(:foo, "bar")
  end
end
{% endhighlight2 %}

Same lines of code in this primitive example, but it will make you look like a better Rails programmer. Here is the method in all its simple glory:
{% highlight2 ruby %}
Class object
  def returning(value)
    yield(value)
    value
  end
end
{% endhighlight2 %}