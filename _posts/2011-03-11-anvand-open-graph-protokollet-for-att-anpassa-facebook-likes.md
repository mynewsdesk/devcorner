---
title: Använd Open Graph-protokollet för att anpassa Facebook Likes
author: Richard
layout: post
categories:
  - Front End
tags:
  - Facebook
  - Microformats
  - Open Graph
---
Vi lade nyligen till <a href="http://ogp.me/" target="_blank">Open Graph</a>-funktioner på Mynewsdesk, med vilka vi kan styra hur våra Facebook Likes ser ut på Facebook. Open Graph är ett protokoll ursprungligen framtaget av Facebook, och har inspirerats av Microformats och liknande protokoll. På hemsidan kan man läsa följande:

> “The Open Graph protocol enables any web page to become a rich object in a social graph. For instance, this is used on Facebook to enable any web page to have the same functionality as a Facebook Page.”

Det låter ju väldigt komplext. Tur då att det är väldigt enkelt att bara definera ett antal nya meta-taggar och få bra resultat snabbt:

<pre id="line10">&lt;meta property="og:site_name"
  content="Mynewsdesk" /&gt;
&lt;meta property="og:title"
  content="Restplatser till fjällen under sportloven" /&gt;
&lt;meta property="og:type"
  content="article" /&gt;
&lt;meta property="og:url"
  content="http://www.mynewsdesk.com/se/...sportloven-577030" /&gt;
&lt;meta property="og:image"
  content="http://resources1.mynewsdesk.com/...skistar_small.jpg" /&gt;
&lt;meta property="og:description"
  content="Snart är sportlovet här ... restplatser till bra priser. " /&gt;</pre>

De olika attributen vi använder:

*   **site_name: **Namnet på tjänsten, alltså inte kundens pressrums namn.
*   **title: ** Titeln på länken som visas på Facebook.
*   **type:** Typ av object vi länkar till. Här finns ett antal alternativ, <a href="http://ogp.me/#types" target="_blank">se listan hos Open Graph</a>. *article*, *blog* eller *website* är de tre som rekommenderas för webbplatser. Vi använder *article *för vårt material. För andra objekt, som exempelvis “platser”, “organisationer” etc finns andra värden att använda.
*   **url: **Permalänken. Bra att definera här för att tex undvika att fel campaign-tracking följer med till Facebook.
*   **image:** Anledningen till varför vi först implementerade Open Graph. Här tillåts vi själva välja vilken bild som ska höra till artikeln på Facebook, istället för att användaren ska välja ut en bild. Det är också bra att kunna definera “fallbacks”, om en artikel inte har en bild associerad, men kunden har en logotyp som vi kan använda istället.
*   **description: **Texten till artikeln på Facebook. “En till två meningar” är rekommendationen här.

Nu ser våra Facebook Likes ut såhär, snyggt och prydligt!

<a rel="attachment wp-att-496" href="http://devcorner.mynewsdesk.com/2011/03/11/anvand-open-graph-protokollet-for-att-anpassa-facebook-likes/screen-shot-2011-03-11-at-16-04-10/"><img class="alignnone size-full wp-image-496" title="Screen shot 2011-03-11 at 16.04.10" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/03/Screen-shot-2011-03-11-at-16.04.10.png" alt="" width="551" height="146" /></a>

Vi definerar några fler media-taggar för våra videor, och dom förklarar sig själv:

<pre>&lt;meta property="og:video"
  content="http://csp.picsearch.com/...F2anRT.flv" /&gt;
&lt;meta property="og:video:type"
  content="application/x-shockwave-flash" /&gt;
&lt;meta property="og:video:height" content="276" /&gt;
&lt;meta property="og:video:width" content="454" /&gt;</pre>

Det ska göra att det går att se våra videor direkt på Facebook:

<a rel="attachment wp-att-520" href="http://devcorner.mynewsdesk.com/2011/03/11/anvand-open-graph-protokollet-for-att-anpassa-facebook-likes/screen-shot-2011-03-11-at-16-52-02/"><img class="alignnone size-full wp-image-520" title="Screen shot 2011-03-11 at 16.52.02" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/03/Screen-shot-2011-03-11-at-16.52.02.png" alt="" width="529" height="167" /></a>

Hur använder du själv Open Graph på din webbplats?