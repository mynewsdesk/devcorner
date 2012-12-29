---
title: 'Rails Tip: Safe Usage of URI.parse'
author: peter
layout: post
categories:
  - Uncategorized
---
As [documented already by Doug][1] URI.parse will thrown an exception if your URL has a trailing space. It also throws an exception on invalid URLs in general. To avoid having URI.parse bomb on your pages you can use a construct like this:

 [1]: http://www.practicalguile.com/2007/09/15/raising-uriinvalidurierror-from-a-perfectly-valid-uri/