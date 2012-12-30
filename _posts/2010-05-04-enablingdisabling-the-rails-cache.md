---
title: Enabling/Disabling the Rails Cache
author: peter
layout: post
categories:
  - Ruby on Rails
---
We needed a way to globally enable/disable the Rails cache so we came up with this patch:

{% highlight2 ruby %}
# This patch gives us an easy way to globally disable/enable the cache
Rails.cache.class.class_eval do
  attr_accessor :enabled
  def read_with_enabled(*args, &block)
    enabled ? read_without_enabled(*args, &block) : nil
  end
  alias_method_chain :read, :enabled
end

if File.basename($0) == "rake" && ARGV.include?("db:migrate")
  # The cache might mess up migrations sometimes so don't use it
  Rails.cache.enabled = false
else
  # Keep caching enabled in development and test environments since this might help us find issues
  # with the cache. We use Rails.cache.clear in-between tests.
  Rails.cache.enabled = true
end

describe "Rails.cache" do
  it "can be disabled and enabled" do
    Rails.cache.enabled = true
    Rails.cache.enabled.should be_true
    Rails.cache.write("foobar", "foo")
    Rails.cache.read("foobar").should == "foo"

    Rails.cache.enabled = false
    Rails.cache.enabled.should be_false
    Rails.cache.write("foobar", "foo")
    Rails.cache.read("foobar").should be_nil

    Rails.cache.enabled = true
    Rails.cache.read("foobar").should == "foo"
  end
end
{% endhighlight2 %}

By default ActionController will use the Rails.cache (RAILS\_CACHE) object configured by config.cache\_store for its fragment caching (action caching is an around filter that utilizes the fragment cache). However, it is possible to configure a different cache store via ActionController::Base.cache_store. If you do that you’ll end up with two different cache stores each with its own independent enabled attribute. For the sake of simplicity I try to make sure we stick to a single cache store object in our application, namely MemCacheStore.