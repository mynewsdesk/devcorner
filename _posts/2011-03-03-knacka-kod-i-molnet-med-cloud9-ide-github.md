---
title: 'Knacka kod i molnet med Cloud9 IDE &#038; GitHub'
author: Joakim Westerlund
layout: post
categories:
  - GUI
  - Javascript
tags:
  - cloud9ide
  - development tools
  - editor
  - git
  - github
  - gui
  - javascript
  - webapp
---
**Cloud9 IDE** är en editor som körs i webbläsaren, skapad för JavaScript utvecklare av JavaScript utvecklare som de själva marknadsför sig. Jag har haft möjligheten att titta lite på det tidigare då jag erhöll ett beta konto för en månad sedan men inte hunnit testa hur det är att arbeta med tills idag.

Så när vi idag hade vår andra dag av Dev Corner här på Mynewsdesk RnD så passade jag på att koppla mitt [github konto][1] med [Cloud9ide][2] och testa lite.

Att komma igång var väldigt smidigt, bara ange sin Git URL och klicka vidare

![Import projects from Git with Cloud9 IDE](/images/wp/2011/03/Screen-shot-2011-03-03-at-2.26.46-PM-600x347.png)

När man väl kopplat ihop sitt konto med GitHub så får man upp alla sina projekt på GitHub på sin dashboard och kan enkelt välja “clone” för att börja koda direkt i webbläsaren.

Då man clonat sitt GitHub projekt till Cloud9 IDE är det bara sätta igång och inte speciellt svårt att känna igen sig från andra utvecklingsverktyg.

![Cloud9 IDE JavaScript jQuery Plugin](/images/wp/2011/03/Screen-shot-2011-03-03-at-2.47.49-PM-600x231.png)

Målet för dagen var att testa att skapa en jQuery plugin (mer om denna kommer senare) som bara använder en testsida och JavaScript så var det enkelt att komma igång och det  jag gillade med JavaScript kodningen var att man direkt får upp varningar om semicolon eller fel finns i koden och det är enkelt att surfa på vanliga HTML sidor genom att bara använda preview knappen i verktyget. Andra trevliga features som finns redan nu är Themes, Syntax Highlighting och en extension manager som jag inte hunnit kolla närmare på.

Har aktivt följt Cloud9 IDE sedan jag hörde att de började arbeta på det och tycker de lyssnat och tagit in feedback bra från utvecklare vad de vill ha i en editor. Personligen så tycker jag den känns snabb för att vara en webbapplikation, den är snygg (viktigt) och känns smart. Mitt första intryck är att **jag gillar det**.

När man väl är klar med sitt kodade så hade jag lite problem att hitta hur jag skulle pusha filerna tillbaka till GitHub igen (är ingen git-guru direkt, speciellt inte i consolen) men fick hjälp av [Richard][3] ([Twitter][4]) att ställa in så att det fungerade till slut.

Det jag fick göra för att få det att fungera var att ändra lite i git config och när jag skulle pusha till GitHub fick jag följande problem.

{% highlight2 console %}
$ git push origin master
fatal: remote error:   You can’t push to git://github.com/jorkas/jCloud9Test.git  Use git@github.com:jorkas/Cloud9Test.git
{% endhighlight2 %}

{% highlight2 console %}
$ git config remote.origin.url git@github.com:jorkas/Cloud9Test.git
$ git config user.name \”Joakim Westerlund\”
$ git config user.email myemail@example.com
{% endhighlight2 %}

När jag sedan körde commandot:
{% highlight2 console %}
$ git config -l
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.fetch=+refs/heads/\*:refs/remotes/origin/\*
remote.origin.url=git@github.com:jorkas/Cloud9Test.git
branch.master.remote=origin
branch.master.merge=refs/heads/master
user.name=Joakim Westerlund
user.email=myemail@example.com*
{% endhighlight2 %}

Lite problem med commit meddelanden hittade jag också, var tvungen att skriva
{% highlight2 console %}
$ git commit -m \’My commit message goes here\’
{% endhighlight2 %}

Så när allt var konfigurerat så fungerade det som det ska.

{% highlight2 console %}
$ git add .
$ git commit -m \’This is a commit from Cloud9 IDE\’
$ git push origin master
{% endhighlight2 %}

En bild hur detta ser ut i den inbyggda consolen i Cloud9 IDE

![Console Cloud9 IDE](/images/wp/2011/03/Screen-shot-2011-03-03-at-3.28.07-PM-600x127.png)

Och på GitHub ser det ut som vanligt.

![GitHub commit/push](/images/wp/2011/03/Screen-shot-2011-03-03-at-3.37.40-PM.png)

Finns mycket att förbättra för att det ska vara ett fullvärdigt alternativ till TextMate för mig, men det är alltid roligt att testa nya verktyg. Tilläggas ska väl att Cloud9 IDE är gratis för open source projekt medan det finns alternativ som kostar för den som vill ha privata repositorier. Så min slutsats efter dagen är att jag nog kan tänka mig ge den några chanser till för open source projekt! Hoppas på mer trevligheter i denna applikation i framtiden.

Jag hoppas att denna post kan vara till nytta för den som vill prova koda med Cloud9 IDE och skriv gärna en kommentar med egna erfarenheter eller frågor.

 [1]: https://github.com/jorkas "Joakim Westerlund at Github"
 [2]: http://cloud9ide.com/ "Cloud9Ide for JavaScripter by JavaScripters"
 [3]: /author/richard/ "Richard at Mynewsdesk"
 [4]: http://twitter.com/richardjohansso "Richard på Twitter"