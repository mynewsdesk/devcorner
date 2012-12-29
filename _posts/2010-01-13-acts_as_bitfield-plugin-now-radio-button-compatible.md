---
title: acts_as_bitfield Plugin now Radio Button Compatible
author: peter
layout: post
categories:
  - Ruby on Rails
  - Uncategorized
---
I’ve [patched][1] the acts_as_bitfield plugin to recognize “true” as true so that now instead of having to write:
{% highlight2 rhtml %}
<%= f.radio_button(:email_activated, true, :checked => (“checked” if @item.email_activated?)) %> Yes

<%= f.radio_button(:email_activated, false, :checked => (“checked” unless @item.email_activated?)) %> No
{% endhighlight2 %}

you can omit the checked argument just like with ActiveRecord boolean attributes:

{% highlight2 rhtml %}
<%= f.radio_button(:email_activated, true) %> Yes

<div class="ii gt">
  <div id=":yn" class="ii gt">
    <%= f.radio_button(:email_activated, false) %> No
  </div>
</div>
{% endhighlight2 %}

 [1]: http://github.com/mynewsdesk/acts_as_bitfield/commit/4057b67470469ba2f0cef1a31cca2d1f5f6bbfee