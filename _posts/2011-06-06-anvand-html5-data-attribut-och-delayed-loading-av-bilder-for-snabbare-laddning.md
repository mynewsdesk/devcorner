---
title: Använd HTML5 data attribut och delayed loading av bilder för snabbare laddning
author: Joakim Westerlund
layout: post
categories:
  - Front End
  - Javascript
  - jQuery
  - Uncategorized
tags:
  - data attributes
  - delayed loding
  - html5
  - images
  - javascript
  - jQuery
  - loading
---
Om man har väldigt många bilder som laddas från externa tjänster på en webbsida så kan man inte alltid vara säker på att den externa tjänsten svarar så snabbt som man vill.

Då även antalet bilder på sidan slöar ner laddningstiden så vill man ibland ladda bilder som inte är kritiska och ej är själva huvudinnehållet på sidan efter att hela DOM:en är laddad för att på så sätt inte påverka den upplevda laddningstiden.

För att ta ett konkret exempel på när och hur man skulle kunna använda detta är när man exempelvis har bilder av flera kontakter på en sida, alternativt kommentarer för en artikel där man visar bilder genom tjänsten [gravatar][1].

I och med HTML5 och data attribut så kan man använda dessa för att spara url:en till den bild man vill ladda och sedan skriva ut ett image element med JavaScript efter att DOM:en är laddad.

För att anropa scriptet efter att DOM:en är laddad så använder man sig av eventet window.onload som skickas då hela trädet är laddat, som bilder, frames osv.

**Exempel med jQuery 1.6.1**
För att göra detta ännu smidigare så kommer här ett exempel med jQuery 1.6.1. Det använder sig av `$(window).load(function(){...});` som gör att det vi kör scriptet först efter att DOM:en är klar.

Där itererar vi över alla länk element som har data-attributet **data-gravatar-url**. För varje element så ersätter scriptet innehållet i länk elementet med en bild som använder **src** från **data-gravatar-url** och lägger till bildens **alt** från den text som står i elementet sedan tidigare.

**Nedan är ett exempel på hur detta kan användas** ([gist på github][2]).



Om man sedan tittar hur laddningen ser ut genom firebug så ser man följande resultat

**Laddning av gravatar med delayed loading**
![Delayed image loading example with firebug](/images/wp/delayed-image-loading.png "Delayed image loading example with firebug")

Om man inte använder sig av onload eventet och laddar bilderna direkt blir resultatet istället enligt nedan:

**Laddning av gravatar utan delayed loading**
![Loading of gravatar images thats not delayed](/images/wp/non-delayed-image-loading.png "Loading of gravatar images thats not delayed")

Av dessa två skärmdumpar kan man se att den utan delayed loading flyttar fram load eventet (röda linjen i Firebug) markant. Den totala laddningstiden är nästan identisk, men man ska ju komma ihåg att detta bara är ett exempel och inte speciellt många bilder som laddas.

 [1]: http://en.gravatar.com/
 [2]: https://gist.github.com/1009784