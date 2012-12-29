---
title: 'New rake task for the Tranlsate plugin: Google Translate'
author: Richard
layout: post
categories:
  - Ruby on Rails
tags:
  - i18n
  - Ruby on Rails
  - translate
---
We just pushed the latest addition to the <a href="http://github.com/newsdesk/translate/tree/master" target="_blank">Translate plugin</a> which adds <a href="http://github.com/newsdesk/translate/commit/6957c62a427bf4a1a001d2ddaaf8b1e794436628" target="_blank">a new rake task</a>: “rake translate:google FROM=sv TO=en”. This will translate all keys in sv.yaml using Google Translate into english and store them in en.yaml.

This is a very convinient way of testing your application in a translated state, and look for strings that were left un-extracted. Actually, I would say 80% of our application’s strings were translated correctly by Google, so less translation work for us!

The only drawback by this solution is the interpolated strings in yaml (“{{variable}}”) that is handled quite dodgy by Google. Sometimes the returned value would be “variable (())”, sometimes “(()) variable” and sometimes even “((var iable))”, with a broken variable name. Therefore we skip those strings for now. If you have a better solution for this, feel free to fork our repository over at Github and submit a patch!

Have a look at the massive language support at: <a href="http://translate.google.com/" target="_blank">http://translate.google.com/</a>

This rake task depends on the <a href="http://railstips.org/2008/7/29/it-s-an-httparty-and-everyone-is-invited" target="_blank">HTTParty gem</a> by John Nunemaker. Thank you John for making HTTP requests sexy!