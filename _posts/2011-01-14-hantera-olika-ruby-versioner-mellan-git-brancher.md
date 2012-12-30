---
title: Hantera olika ruby-versioner mellan git-brancher
author: Jonas
layout: post
categories:
  - Workflow
tags:
  - bash
  - git
  - rvm
---
Vi använder självklart [RVM][1] för att hålla isär olika versioner av Ruby och uppsättningar av gems. Vi har vår [.rvmrc-fil][2] incheckad i git-repot för att alla utvecklare ska köra med samma inställningar.

Detta fungerar jättesmidigt så länge som man inte vill byta version av Ruby eller uppsättning av gems mellan olika brancher i Git. Varken Git eller RVM har koll på att .rvmrc-filen ändras när en annan branch checkas ut, och detta leder till att utvecklaren själv måste hålla koll på när det behövs byta version och/eller uppsättning av gems.

För att underlätta detta så skrev jag ihop en enkel git-hook som körs vid varje checkout:

Koden läggs i filen
{% highlight2 console %}REPO/.git/hooks/post-checkout{% endhighlight2 %} och ges körrättigheter:

{% highlight2 console %}
$ chmod +x REPO/.git/hooks/post-checkout
{% endhighlight2 %}

Scriptet avgör först om man byter mellan två brancher, och kör i sådana fall en diff på `.rvmrc` mellan de olika brancherna. Är det någon diff skrivs en tydlig röd ruta ut i terminalen för att påminna utvecklaren om att ladda om sin rvm.

 [1]: http://rvm.beginrescueend.com/
 [2]: http://rvm.beginrescueend.com/workflow/rvmrc/