---
title: 'Translate: New Rails I18n Plugin with a Nice Web UI'
author: peter
layout: post
categories:
  - Ruby on Rails
tags:
  - i18n
  - plugin
---
Here at [Mynewsdesk][1] we are in the midst of internationalizing a fairly big Ruby on Rails application. Sven Fuchs’s I18n Textmate bundle has been a great help in extracting thousands of texts away from our source code and into YAML files. As we were getting ready to start the actual translation work we figured that a nice web interface would be helpful. Since we couldn’t find a web UI out there we decided to roll our own and thus the [translate plugin][2] was born.

The plugin requires that you run Rails 2.2 or later. It lets you mount a web UI at /translate where you can view and translate all your I18n texts. You can select which locales you are translating from and to, search for keys and texts, and choose to view only untranslated texts. The plugin reads and writes all translations to YAML files under config/locales. For more details, please refer to the [README][3] file.

**Screenshot of the User Interface**

<div id="attachment_30" class="wp-caption alignnone" style="width: 510px">
  <a href="http://developer.newsdesk.se/wp-content/uploads/2009/01/newsdesk-translate.png"><img class="size-full wp-image-30" title="Newsdesk translate I18n screenshot" src="http://developer.newsdesk.se/wp-content/uploads/2009/01/newsdesk-translate.png" alt="Newsdesk translate I18n screenshot" width="500" height="360" /></a><p class="wp-caption-text">
    Newsdesk translate I18n screenshot
  </p>
</div>

 [1]: http://www.mynewsdesk.com
 [2]: http://github.com/mynewsdesk/translate
 [3]: http://github.com/mynewsdesk/translate/tree/master