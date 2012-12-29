---
title: 'Microformats hCard and Rich Snippets &#8211; get started'
author: Joakim Westerlund
layout: post
categories:
  - GUI
  - SEO
tags:
  - hCard
  - Microformats
  - RDFa
  - Rich snippets
  - Semantic web
---
As the semantic information of the Web keeps getting more important, the Web needs more structured information and machine-readable information to describe the content of the HTML markup in a better way. A big step forward for the Semantic Web is the recently announcement from Google with the [release of “rich snippets”][1] for reviews (**hReview**) and contacts (**hCard**) marked-up with either [Microformats][2] or RDFa. Yahoo in turn have indexed Microformats and RDFa for over a year, so now two large search engines have pushed the semantic web forward.

### Microformats at Newsdesk

At Newsdesk we recently deployed our first Microformats implementation for more than 2800 contact people. The Microformats approach is an easy way to mark-up the existing HTML elements with specific class names and element attributes that still are valid HTML.

### How can you see this in action?

The best tool I’ve found so far is the [Operator add-on][3] for [Mozilla Firefox][4], simply install it and browse a contact at [Newsdesk][5] and you will get the possibility to download contact information to your computer, or simply add the contact to your Yahoo contacts.

**Microformats hCard example with Firefox Operator add-on**

[<img class="alignnone size-medium wp-image-73" title="Microformats contact list with Operator add-on" src="/images/wp/2009/05/microformats-list-300x230.png" alt="" width="300" height="230" />][6]

Microformats at [pressroom contacts listing][7] displays a list of contacts marked up with Microformats hCard.

**Microformats contact example**

[<img class="alignnone size-medium wp-image-74" title="Microformats Peter Ingman" src="/images/wp/2009/05/microformats-peter-300x253.png" alt="" width="300" height="253" />][8]

[Microformats hCard contact][9] with detailed information about the contact and possibility to download contact.

### Example of Microformats hCard mark-up

{% highlight2 html %}

<div id="hcard-Joakim-Westerlund" class="vcard"><img class="photo" src="http://newsdesk.se/files/123d405e0dfaf99b9c3cae178ca0e124/resources/ResourceWebImage/thumbnails/joakimwesterlund_small.jpg?1237881798" alt="photo of Joakim Westerlund" />
<a class="url fn" href="http://www.newsdesk.se">Joakim Westerlund</a>
<div class="org">Newsdesk</div>
<a class="email" href="mailto:joakim.westerlund@newsdesk.se">joakim.westerlund@newsdesk.se</a>
<div class="adr">
<div class="street-address">Bondegatan 21</div>
<span class="locality">Stockholm</span>,
<span class="postal-code">116 33</span>
<span class="country-name">Sweden</span>

</div>
</div>
{% endhighlight2 %}

And in HTML the <strong>Microformats hCard</strong> will be displayed like this:

<div style="background-color: #f9f9f9; border: 1px dotted #D1D1D1; padding: 1em;">
<div id="hcard-Joakim-Westerlund" class="vcard"><img class="photo" src="http://newsdesk.se/files/123d405e0dfaf99b9c3cae178ca0e124/resources/ResourceWebImage/thumbnails/joakimwesterlund_small.jpg?1237881798" alt="photo of Joakim Westerlund" />
<a class="url fn" href="http://www.newsdesk.se">Joakim Westerlund</a>
<div class="org">Newsdesk</div>
<a class="email" href="mailto:joakim.westerlund@newsdesk.se">joakim.westerlund@newsdesk.se</a>
<div class="adr">
<div class="street-address">Bondegatan 21</div>
<span class="locality">Stockholm</span>,
<span class="postal-code">116 33</span>
<span class="country-name">Sweden</span>

</div>
</div>
</div>

See more examples at the <a title="Microformats Wiki" href="http://microformats.org/wiki/hcard">Microformats Wiki</a> or use the <a title="Microformats creator" href="http://microformats.org/code/hcard/creator">Microformats creator</a> to define your own hCard markup or visit for more information. Are you interested in using RDFa, have a look at <a title="RDFa for HTML authors" href="http://rdfa.info/2009/05/14/rdfa-for-html-authors-start-here/">RDFa for HTML authors</a>.
### Google Rich snippet

Google will not automatically detect your Microformats. When you have deployed an Microformats hCard or RDFa implementation to your site, you have to <a title="Submit rich snippets to Google" href="http://www.google.com/support/webmasters/bin/request.py?contact_type=rich_snippets_feedback">fill out a form at Google</a> and submit examples of pages that have implemented the hCard or hReview to let them know you are interested in <strong>Rich snippet</strong> on Google with your Microformats/RDFa.

 [1]: http://googlewebmastercentral.blogspot.com/2009/05/introducing-rich-snippets.html "Google introducing rich snippets"
 [2]: http://microformats.org/
 [3]: https://addons.mozilla.org/en-US/firefox/addon/4106 "Download Operator add-on for Mozilla Firefox"
 [4]: http://www.mozilla.com/en-US/firefox/firefox.html "Mozilla Firefox"
 [5]: http://www.Newsdesk.se "Publish pressreleases at Newsdesk "
 [6]: /images/wp/2009/05/microformats-list.png
 [7]: http://newsdesk.se/pressroom/newsdesk/contact_person/list "Newsdesk contacts with Microformats"
 [8]: /images/wp/2009/05/microformats-peter.png
 [9]: http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14 "Peter Ingman at Newsdesk hCard marked up with Microformats"