---
title: Rake Tasks for the Translate Plugin
author: peter
layout: post
categories:
  - Ruby on Rails
tags:
  - i18n
  - plugin
  - Ruby on Rails
---
We’ve [added two Rake tasks][1] to the [Translate plugin][2]. The lost\_in\_translation rake task helps find I18n keys in lookups in your code that are missing from your default locale YAML file (i.e. config/locales/sv.yml in our case). The merge_keys Rake task is used in conjunction with [Sven Fuchs’s Rails I18n Textmate bundle][3] to move extracted keys and texts into their config/locales YAML file. Those two rake tasks have been quite helpful in our I18n work here at Newsdesk so we figured that we should share them with the community.

 [1]: http://github.com/newsdesk/translate/commit/034ff2526e9e5b00ed62b252e28abc70540ef310
 [2]: http://github.com/newsdesk/translate/tree/master
 [3]: http://github.com/svenfuchs/rails-i18n/tree/master