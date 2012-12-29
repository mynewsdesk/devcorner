---
title: Simplest bit.ly API implementation possible?
author: Richard
layout: post
categories:
  - Ruby on Rails
  - Services
tags:
  - 'bit.ly'
  - Ruby on Rails
  - twitter
---
<div>
  To use an URL shortener in your project, I would strongly recommend bit.ly. It let you set a custom URL (name) and track traffic, clicks and conversations on micro blogs. But best of all, it should be super-fast and super-stable, since Twitter recently chose bit.ly as their default URL shortening service.
</div>

<div>
  Using the bit.ly REST API is also super-easy! Here is how you do it in Rails:
</div>

1.  [Sign up to get your API username/password here. ][1]
2.  Get HTTParty if you don’t already use it: **sudo gem install httparty**
3.  [][1]Copy the code: 
    <pre>require 'httparty'
class Bitly
  include HTTParty
  base_uri 'api.bit.ly'
  basic_auth 'username', 'password'
  format :json
  def self.shorten(url)
    response = get('/shorten', :query =&gt; required_params.merge(:longUrl =&gt; url))
    response['results'][url]['shortUrl']
  end
  def self.required_params
    {:version =&gt; "2.0.1"}
  end
end</pre>

4.  Paste code into lib/bitly.rb. Usage: Bitly.shorten(“longurl”)

<div>
  This is perfect for, as an example Tweeting your URL’s. (I will get back to our Twitter API implementation at Newsdesk.)
</div>

<div>
  The complete bit.ly API docs are here: <a href="http://code.google.com/p/bitly-api/wiki/ApiDocumentation">http://code.google.com/p/bitly-api/wiki/ApiDocumentation</a>
</div>

<div>
  To extend and share this class – use my Gist: <a href="http://gist.github.com/112191">http://gist.github.com/112191</a>
</div>

 [1]: http://bit.ly/account/register?rd=/