---
title: Hantera profiler i olika versioner av Firefox
author: Joakim Westerlund
layout: post
categories:
  - Browsers
  - Front End
tags:
  - firefox
  - profilemanager
  - profiles
---
Som webbutvecklare så vill jag gärna kunna testa nya versioner av webbläsare i ett tidigt skede och på samma gång enkelt kunna falla tillbaka och använda den version av webbläsaren som majoriteten av besökaren av den webbplats jag utvecklar för använder.

Då jag är en flitig användare av Extensions i **Firefox** så är det tidskrävande och irriterande att när man byter mellan olika versioner av webbläsaren så har extensions en tendens att uppgradera sig alternativt sluta fungera med den aktuella versionen som man för tillfället använder.

För att lösa detta problem så kan man använda sig av olika profiler för de olika Firefox versionerna man har installerade. Jag stötte nyligen på ett program som man kan använda sig av om man vill slippa sig av att använda terminalen som heter **Profile Manager** som fungerar ypperligt för **hantera profiler i Firefox** och finns som en beta 2.

<a rel="attachment wp-att-766" href="http://devcorner.mynewsdesk.com/2011/05/21/hantera-profiler-i-olika-versioner-av-firefox/profile-manager/"><img class="size-large wp-image-766" title="Profile Manager Firefox Versions" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/05/profile-manager-600x517.png" alt="" width="600" height="517" /></a>  
Så här kan det se ut i **Profile Manager beta 2** för att hantera Firefox profilerna

Ladda ner Profile Manager  
<ftp://ftp.mozilla.org/pub/utilities/profilemanager/1.0_beta2/>

Eller läs mer från en av skaparna av Profile Manager  
<http://jagriffin.wordpress.com/2011/01/11/profilemanager-1-0_beta1/>

Vill man mot förmodan hellre använda sig av terminalen för att hantera sina profiler så gör man det så här:

**Mac OS X**

*   /Applications/Firefox.app/Contents/MacOS/firefox-bin -profilemanager

**Linux**

*   ./firefox -profilemanager

**Windows**

*   firefox.exe -profilemanager
*   firefox.exe -P

Vill du lära dig mer om profiler så finns bra läsning på <http://kb.mozillazine.org/Profile_manager>