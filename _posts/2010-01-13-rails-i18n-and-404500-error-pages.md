---
title: Rails I18n and 404/500 Error Pages
author: peter
layout: post
categories:
  - Ruby on Rails
  - Services
---
If you have a multilingual site it makes sense for your 404 and 500 error pages to be multilingual as well. Rails supports multilinguality by serving up error pages with a path like public/500.sv.html, where sv is the locale. In the case where Rails itself is fundamentally broken or fails to respond your webserver will respond with public/500.html, so we need that file as well. A way to achieve this is to keep your 404 and 500 pages as ERB templates with I18n.translate lookups under app/views/errors and have a Rake task that generates the static files under public:

{% highlight2 ruby %}
# In lib/tasks/deploy.rake
namespace :deploy do
  def write_error_page(status, locale = nil)
    dest_filename = [status.to_s, locale, "html"].compact.join(".")
    File.open(File.join(Rails.root, "public", dest_filename), "w") do |file|
      path = File.join(Rails.root, "app", "views", "errors", "#{status}.rhtml")
      file.print ERB.new(File.read(path)).result
    end
  end

  def default_error_page_locale
    :en
  end

  desc 'Write public/404.html and public/500.html error pages'
  task :write_error_pages => :environment do
    [500, 404].each do |status|
      I18n.valid_locales.each do |locale| # I18n.available_locales except :root
        I18n.with_locale locale do
          write_error_page(status, locale)
          # We need a default error page for when Rails doesn't work
          write_error_page(status) if locale.to_sym == default_error_page_locale
        end
      end
    end
  end
end

# Two utility methods below that probably belong under config/initializers
module I18n
  def self.with_locale(locale, &block)
    orig_locale = self.locale
    self.locale = locale
    return_value = yield
    self.locale = orig_locale
    return_value
  end
end

def I18n.valid_locales
  I18n.available_locales.reject { |locale| locale == :root }
end
{% endhighlight2 %}

That Rake task can then be invoked by Capistrano in an after\_update\_code deploy hook to make the pages available in production.