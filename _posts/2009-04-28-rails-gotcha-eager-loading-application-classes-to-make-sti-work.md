---
title: 'Rails Gotcha: Eager Loading Application Classes to Make STI Work'
author: peter
layout: post
categories:
  - Ruby on Rails
---
Recent versions of Rails (2.2 and later I think) will eager load all application Ruby classes when booting up the server. However, because of [issues with migrations][1] this is not done if you are executing a Rake task that depends on the :environment task (such as db:migrate). It is also not done if cache_classes is set to true, a setting that you have by default in development to allow you to change your Ruby classes and not have to restart the server to see the result. It turns that the lazy loading of application classes in development can lead to breakage of ActiveRecords STI (Single Table Inheritance) support. In our case we had an STI with YouTubeChannel that inherits from ApiChannel and we ran into this behaviour:

{% highlight2 ruby %}
>> ApiChannel.send(:subclasses)
=> []
>> ApiChannel.find(1)

# ApiChannel Load (2.5ms)   SELECT * FROM "channels" WHERE ("channels"."id" = 1) AND ( ("channels"."type" = ‘ApiChannel’ ) )
ActiveRecord::RecordNotFound: Couldn’t find ApiChannel with ID=1

>> YouTubeChannel
=> YouTubeChannel(…)
>> ApiChannel.send(:subclasses)
=> [YouTubeChannel...]
>> ApiChannel.find(1)

#  ApiChannel Load (0.9ms)   SELECT * FROM "channels" WHERE ("channels"."id" = 1) AND ( ("channels"."type" = ‘ApiChannel’ OR "channels"."type" = ‘YouTubeChannel’ ) )
=> #<YouTubeChannel id: 1…
{% endhighlight2 %}

So what is happening here is that since YouTubeChannel hasn’t been loaded Rails doesn’t know that it exists and ApiChannel.subclasses is empty and ActiveRecord (construct\_finder\_sql – add\_conditions! – type\_condition) cannot construct the proper SQL condition.

I’m not sure what the reason is that classes are not eager loaded in development (other than the obvious tomake the server startup time shorter), but for now I have patched the Rails::Initializer like this:

{% highlight2 ruby %}
Rails::Initializer.class_eval do
# Allows us to run rake jobs:work in development
def is\_delayed\_job\_rake\_task?
  $0 =~ /rake$/ && ARGV.detect { |arg| arg =~ /^jobs:/ }
end

# Eager load application classes
def load\_application\_classes
  return if ($rails\_rake\_task && !is\_delayed\_job\_rake\_task?)
  if configuration.cache_classes
    configuration.eager\_load\_paths.each do |load_path|
      matcher = /\A#{Regexp.escape(load_path)}(.*)\.rb\Z/
      Dir.glob("#{load_path}/*\*/\*.rb").sort.each do |file|
        require_dependency file.sub(matcher, ‘\1′)
      end
    end
  end
end
{% endhighlight2 %}

 [1]: https://rails.lighthouseapp.com/projects/8994/tickets/802-eager-load-application-classes-can-block-migration