---
title: Lär dig mer om din site med Google Analytics Event Tracking
author: Richard
layout: post
categories:
  - Analytics
tags:
  - Analytics
  - javascript
---
Google Analytics är fullt av möjligheter! Många funktioner tror jag aldrig används för ditt genomsnittliga projekt. Och det är synd, eftersom funktionerna är gratis, ger massor av bra information, och ofta väldigt enkla att utnyttja om man bara vet hur man ska använda dom. Under en Lab Day nyligen var jag nyfiken på att se hur vi kunde använda [Event Tracking][1] för att lära oss mer om hur vår site används.

Kort sammanfattat kan man säga att Event Tracking är som en form av intern Campaign Tracking. Vi kan mäta hur många gånger utvalda element klickas på, och markera dom med olika parametrar. Som ni kanske känner till kan man bara använda Campaign Tracking från ingående länkar, och jag misstänker att det är en mycket mer använd tjänst än Event Tracking. Kanske för att den är mer uppenbar och upplevs enklare att implementera.

Vi har byggt ett Javascript för Event Tracking som innebär att vi kan mäta klick på alla element på siten som har en bestämd klass. Lika enkelt som att mäta Campaign Tracking! Vi sätter helt enkelt data-attribut på elementen med de olika parametrarna som Event Tracking behöver, ungefär som man sätter GET-parametrar i länkar för Campaign Tracking. Här är Javascript-klassen (skriven för Prototype) som gör allt jobb:



Scriptet kommer alltså initieras när sidans DOM-träd laddats, sätta upp ett click-event på alla element (oftast länkar) med klassen *event-tracking*, samt läsa in de olika data-attributen: *category*, *action*, *label* och *value*. Sedan skickas värdena till Analytics genom dess globala objekt asynkront: *_gaq.push(). *

Ett problem vi stötte på var att den asynkrona trackingen avbryts när sidan laddas om genom att användaren klickar på länken. Därför har vi en timeout för att hinna med laddningen av Googles tracker-gif. Normalt ska laddningen inte ta mer än ~50 ms, men för att vara på den säkra sidan väntar vi 100 ms. Om det är en Ajax-länk som ska trackas behöver vi dock ingen timeout eftersom laddningen av tracker-gifen kan ske parallellt med Ajax-requesten. En fördröjning på 100 ms tror jag också är mer påtaglig för den typen av länkar som normalt ska vara snabba och responsiva. För dessa länkar sätter vi ytterligare en klass: *ajax-tracking*. * *

## Parametrar och namngivning

Det svåraste med Event Tracking har ändå varit att komma fram till strukturen för de olika parametrarna som går att ange. För oss betyder de olika parametrarna följande:

### ***category *- krävs**

Den övergripande kategorin, och det som är målet med vad du mäter. Tex “follow” för att mäta hur många som följer ett företags nyhetsflöde på Mynewsdesk, eller “image-download” för att mäta hur mycket våra bilder laddas ner.

### ***action *- krävs**

Den unika länken/knappen som skiljer olika mätpunkter. Vi har bestämt oss för att sätta kategori-namet först i action-namnet för att kunna skilja på olika actions under sidan i Analytics som listar alla actions oberoende av kategori. Formatet blir oftast: *“category-page-unique_link”* eller *“category-group-page”*.

Exempelvis blir action *“follow-pressroom*” för att mäta klick på “Följ oss”-länken i kundens pressrum, eller *“follow-search”* för att mäta “Följ”-länken intill företaget i resultatlistan på Sök företag.

*“follow-recommended-dashboard”* anges för att mäta de som väljer att följa företag som vi [rekommenderar för våra användare][2] från deras Dashboard, och *“follow-recommended-network”* för att mäta klicken på rekommenderade företag från sidan *“network”*.

### ***label *- valfri**

Här kan man ange namnet eller titeln på det objekt som mäts. Vi anger här exempelvis namnet på det företag som användaren följer i exemplet ovan. Eller rubriken på den bild som laddas ner för kategorin “image-download”. Då kan vi alltså se vilka bilder som laddats ner flest gånger, något som var svårt att göra tidigare eftersom vi inte parsar våra logg-filer.

*Glöm inte att Javascript-escape:a strängen!*

### ***value *- valfri**

Ett numeriskt värde som ska ange någon kvantitet, tex hur länge en användare tittade på en video innan han klickade på Stop-knappen, eller laddningstiden på en sida. I rapporten i Analytics kommer man, om parametern anges, få lite nyttig info såsom genomsnitts-värden samt hur mycket laddningstider påverkar användarens beteenden. Ingenting som vi använder, ännu.

Det är såklart bra att gå igenom Googles egna genomgång av parametrarna, i deras [Event Tracker Guide][3].

En färdig länk kan se ut såhär:

<pre>&lt;a href="/follow/872" class="event-tracking"
data-event-category="follow" data-event-action="follow-search"
data-event-label="Scania AB"&gt;Följ oss på MyNewsdesk&lt;/a&gt;</pre>

Såhär kan sedan rapporten se ut för vår kategori “follow”, för den senaste veckan:

![Event Tracking Category: follow overview](http://devcorner.mynewsdesk.com/wp-content/uploads/2011/02/Event-Tracking-Category_-Google-Analytics.jpg)

Här lär vi oss att Följ-länken i kunders pressrum används mest, medans de rekommenderade företagen under sidan “network” används minst.

Nu kan vi enkelt följa upp alla justeringar vi gör på dessa länkar, och verkligen se om ett designbeslut var bra eller dåligt. Inte illa!

 [1]: http://code.google.com/apis/analytics/docs/tracking/eventTrackerGuide.html
 [2]: http://devcorner.mynewsdesk.com/2010/10/06/item-based-recommendations-with-mahout/
 [3]: http://code.google.com/apis/analytics/docs/tracking/eventTrackerGuide.html#Anatomy