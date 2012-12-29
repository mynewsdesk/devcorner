---
title: Keep Your I18n Translation Files Tidy
author: peter
layout: post
categories:
  - Ruby on Rails
---
I added the Rake task [translate:remove\_obsolete\_keys][1] that lets you remove all obsolete translations from your YAML files. How is a translation rendered obsolete? Suppose you are translating from english to a number of other languages. Over time you may end up removing a number of I18n keys from the english translation file as your application evolves. The Rake task lets you remove those keys for all locales so you don’t have to do that manually.

 [1]: http://github.com/mynewsdesk/translate/commit/79a4ef7ea7d6aff1914a859f7066f71352dd3dbe