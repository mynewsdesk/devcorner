---
title: acts_as_bitfield Plugin now Radio Button Compatible
author: peter
layout: post
categories:
  - Ruby on Rails
  - Uncategorized
---
I’ve [patched][1] the acts\_as\_bitfield plugin to recognize “true” as true so that now instead of having to write:

<%= f.radio\_button(:email\_activated, true, :checked => (“checked” if @item.email_activated?)) %> Yes

<%= f.radio\_button(:email\_activated, false, :checked => (“checked” unless @item.email_activated?)) %> No

you can omit the checked argument just like with ActiveRecord boolean attributes:

<%= f.radio\_button(:email\_activated, true) %> Yes

<div class="ii gt">
  <div id=":yn" class="ii gt">
    <%= f.radio_button(:email_activated, false) %> No
  </div>
</div>

 [1]: http://github.com/mynewsdesk/acts_as_bitfield/commit/4057b67470469ba2f0cef1a31cca2d1f5f6bbfee