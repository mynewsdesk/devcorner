---
title: Google Analytics Event Tracking jQuery Plugin
author: Joakim Westerlund
layout: post
categories:
  - Analytics
  - Front End
  - Javascript
  - jQuery
  - plugins
tags:
  - data attributes
  - event-tracking
  - front-end
  - google analytics
  - javascript
  - jQuery
  - plugins
---
Som jag skrev i ett tidigare inlägg om att [utveckla med Cloud9 IDE][1] så testade jag att skriva en jQuery plugin med Cloud9 IDE och lovade att återkomma med en blogpost med resultatet av plugin:en.

Efter att ha varit på [Geek Meet med Jake Archibald][2] som anordnades i Stockholm av [Robert Nyman][3] i februari där Jake Archibald bland annat tog upp vikten av ### återanvändbar kod så tänkte jag att det börjar bli läge att utveckla min första jQuery plugin.

[Richard][4] har tidigare skrivit om [Analytics Event Tracking med Prototype][5] som vi i dagsläget använder. Då vi använder oss flitigt av Google Analytics och Event Tracking här på Mynewsdesk tyckte jag att det kändes aktuellt att skriva en plugin som även vi har användning för och dela med oss av de möjligheter som finns med Event Tracking i Google Analytics.

Vi har olika behov av spårning hos oss och behövde en plugin som fungerade lite olika vid olika tillfällen. I det flesta fall använder vi oss av ### data-attribut på elementen som vi vill spåra i Google Analytics men ibland vill vi kunna sätta värden dynamiskt och mer generellt.

Lösningen i denna [jQuery Plugin för Google Analytics Event Tracking][6] är att det i grundutförandet använder data attributen på elementen för spårning och sparar det värdet i Analytics.

Data-attributen som används är:

*   data-jaet-report-category
*   data-jaet-report-action
*   data-jaet-report-label
*   data-jaet-report-value

### Spårning med Data-attribut

I det enklaste utförandet kan man göra på följande sätt för att börja använda plugin:en.

{% highlight2 html %}
<a href="/signup" data-jaet-report-category="signup" data-jaet-report-action="signup-click">Sign up</a>
{% endhighlight2 %}

{% highlight2 javascript %}
$(“a”).analyticsEventTracking();
{% endhighlight2 %}
_Ovanstående kod skulle spara en Category som är **signup** och en Action som är **signup-click**._

### Spårning med definierade värden

Om man istället önskar att definera spårnings värden direkt vid initieringen så kan man göra det också. Säg att vi vill spåra alla användare som klickar på länkar i ett visst område på sajten med vissa värden.

{% highlight2 html %}

<div id="contact">
  <a href="/contact">Contact</a><br /> <a href="http://twitter.com/mynewsdesk">Twitter</a>
</div>

{% endhighlight2 %}

{% highlight2 javascript %}
$(“#contact a”).analyticsEventTracking({
category : “contact”,
label: “contact-link-click”
});
{% endhighlight2 %}

_Ovanstående kod skulle spara en Category som är **contact** och en Action som är **contact-link-click** för samtliga länkar i arean som användaren klickar på._

### Mer avancerad spårning

Vill man göra mer avancerad spårning kan man göra genom att ange en JavaScript-funktion som ska köras för att hämta värdet innan det sparas till Google Analytics. På detta sätt så kan man styra mer vad som ska hända och vad som ska spåras. Jag har testat detta några veckor med följande JavaScript kod.

{% highlight2 javascript %}
$(“div.box a”).analyticsEventTracking({
category : “Beta1″,
label: “link-events”,
action: googleEventTrackingBetaTest
});
function googleEventTrackingBetaTest(){
return $(this).attr(“href”);
}
{% endhighlight2 %}

Med denna kod så blir resultatet enligt följande i Google Analytics:

### Beta1 som event category i Google Analytics

![Beta 1 Event Tracking Category](/images/wp/2011/04/Screen-shot-2011-04-11-at-11.06.09-AM-600x65.png)

Event category Beta1 visas i Google Analytics

### Link-events som event label i Google Analytics

![Link-events label i google analytics](/images/wp/2011/04/Screen-shot-2011-04-11-at-11.08.38-AM-600x67.png)

Event label link-events visas i Google Analytics

### Link-action som som event action i Google Analytics

![Saved Actions with Google Event Tracking](/images/wp/2011/04/Screen-shot-2011-04-11-at-11.11.16-AM1-600x252.png)

I bilden ovan ser man värdet under action då länkarna som användare klickat på genom att funktionen ### *googleEventTrackingBetaTest* har körts för att hämta ut värdet på länken som besökaren klickade på.

Rapporterna i Google Analytics hittar man under *Content -> Event Tracking*

Detta var lite exempel på hur man gör, finns många saker att applicera detta på. Exempelvis så kan man enkelt analysera utfallet av olika länkar i förhållande till varandra och se ifall det ger något resultat av att flytta länkar/ändra design när man använder sig av spårning med Google Analytics Event Tracking.

Denna jQuery plugin ligger på [Github][7] med mer information om vad som krävs och ett enkelt exempel på implementation. Givetvis så måste man ha [Google Analytics][8] på sin sajt för att spårningen ska fungera.

**Uppdatering 21/5-2011, uppdaterade ändringen av data-attributen med “namespacing” så de avspeglar Version 1.0 av pluginen**

 [1]: /2011/03/03/knacka-kod-i-molnet-med-cloud9-ide-github/
 [2]: http://robertnyman.com/2011/01/12/geek-meet-february-2011-with-jake-archibald/
 [3]: http://twitter.com/robertnyman
 [4]: http://twitter.com/richardjohansso
 [5]: /2011/02/10/lar-dig-mer-om-din-site-med-google-analytics-event-tracking/
 [6]: https://github.com/jorkas/jquery-analyticseventtracking-plugin "jQuery Plugin for Google Analytics Event Tracking"
 [7]: https://github.com/jorkas/jquery-analyticseventtracking-plugin "jQuery plugin - Google Event Tracking with Google Analytics"
 [8]: http://code.google.com/apis/analytics/docs/tracking/asyncTracking.html