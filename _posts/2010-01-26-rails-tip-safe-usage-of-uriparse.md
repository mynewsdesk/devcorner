---
title: 'Rails Tip: Safe Usage of URI.parse'
author: peter
layout: post
categories:
  - Uncategorized
---
As [documented already by Doug][1] URI.parse will thrown an exception if your URL has a trailing space. It also throws an exception on invalid URLs in general. To avoid having URI.parse bomb on your pages you can use a construct like this:

{% highlight2 ruby %}
module URI
  # Safe alternative to URI.parse(uri_string).host to avoid exception being thrown
  # on URLs with trailing spaces or invalid URLs. See also:
  # http://www.practicalguile.com/2007/09/15/raising-uriinvalidurierror-from-a-perfectly-valid-uri/
  def self.host(uri_string)
    begin
      URI.parse(uri_string.try(:strip)).host
    rescue URI::InvalidURIError
      nil
    end
  end
end

describe URI do
  describe "host method" do
    it "can parse valid URLs" do
      URI.host("http://www.dn.se/ekonomi/").should == "www.dn.se"
      URI.host("http://dn.se/ekonomi/").should == "dn.se"
      URI.host("http://192.0.0.1/ekonomi/").should == "192.0.0.1"
      URI.host("http://www.newsdesk.se").should == "www.newsdesk.se"
    end

    it "returns nil for trailing space and invalid URLs" do
      URI.host("  http://www.dn.se/ekonomi/  ").should == "www.dn.se"
      URI.host("f o o b a r").should be_nil
      URI.host("").should be_nil
      URI.host("   ").should be_nil
    end
  end
end
{% endhighlight2 %}

 [1]: http://www.practicalguile.com/2007/09/15/raising-uriinvalidurierror-from-a-perfectly-valid-uri/