---
title: Microformats validator/transformator
author: Joakim Westerlund
layout: post
categories:
  - GUI
tags:
  - JSON
  - Microformats
  - Operator
  - Optimus
  - Semantic web
  - Transformator
  - Validator
  - XML
---
Today I found an interesting service for Microformats validation/transforming that I found very useful. If you read my previous post about [Newsdesk implementing Microformats][1] for contact people you know that my favorite Microformats “tool/validator” so far is the Operator add-on for Firefox.

In a blog post from the [Microformats team][2] I read about an interesting service named [Optimus][3] that validates and transform your Microformats at a public URI and get the the data back as **XML**, **JSON** and **JSON-P**.

Choose an URI with a contact mark up at [Newsdesk][4]:  
[http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14][5]

and just pass it as parameter to Optimus like this:

[/?uri=http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14][6]

As result you get a nicely formatted XML view with all Microformat information displayed.

[sourcecode language='xml']  
<?xml version="1.0" encoding="UTF-8"?>

  
<microformats from="http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14" title="Newsdesk - Peter Ingman (Administration/företagsledning) - Newsdesk">  
<description>Peter Ingman är en av grundarna och VD för Newsdesk.</description>  
<hcard>  
<email>pingman@newsdesk.se</email>  
<fn>
  Peter Ingman
</fn>

  
<org>Newsdesk</org>  
<role>VD</role>  
<tel>  
<type>Work</type>  
<value>08 644 89 51</value>  
</tel>  
</hcard>  
<rel-nofollow>  
<nofollow href="http://newsdesk.se/search/news">Sök i medier</nofollow>  
<nofollow href="http://newsdesk.se/publish">Publicera</nofollow>  
</rel-nofollow>  
</microformats>  
[/sourcecode]

You can also easily get the response data as **JSON** and define an **Callback function** with just adding the parameters **format=json** and **function=cbFunction**.

[/?uri=http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14&**format=json**&**function=cbFunction**][7]

[sourcecode language='javascript']  
cbFunction({  
“from”: “http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14″,  
“title”: “Newsdesk – Peter Ingman (Administration/företagsledning) – Newsdesk”,  
“hcard”: {  
“email”: ["pingman@newsdesk.se"],  
“fn”: “Peter Ingman”,  
“org”: ["Newsdesk"],  
“role”: “VD”,  
“tel”: [{  
"type": ["Work"],  
“value”: “08 644 89 51″  
}]  
},  
“rel-nofollow”: {  
“nofollow”: [{  
"href": "http://newsdesk.se/search/news",  
"value": "Sök i medier"  
},  
{  
"href": "http://newsdesk.se/publish",  
"value": "Publicera"  
}]  
}  
});  
[/sourcecode]

### Microformats Validation

The Optimus service also supports validation of Microformats. Pass your URI that includes Microformats mark-up and add the parameter **format=validate** and you will get a validator page with information about possible errors.

[See Example here (/?format=validate&uri=http://newsdesk.se/pressroom/newsdesk/contact_person/list)][8]

 [1]: http://developer.newsdesk.se/2009/05/26/microformats-hcard-and-rich-snippets-get-started/ "Microformats hCard and Rich Snippets"
 [2]: http://microformats.org/blog/2009/05/27/placemaker-optimus-validator/
 [3]: http://microformatique.com/optimus/ "Optimus Transformator and Validator"
 [4]: http://www.newsdesk.se
 [5]: http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14 "Peter Ingman Newsdesk"
 [6]: http://microformatique.com/optimus/?uri=http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14 "Peter Ingman XML view of Microformats"
 [7]: http://microformatique.com/optimus/?uri=http://newsdesk.se/pressroom/newsdesk/contact_person/view/peter-ingman-administration-foeretagsledning-14&format=json&function=cbFunction "Peter Ingman JSON view of Microformats"
 [8]: http://microformatique.com/optimus/?format=validate&uri=http://newsdesk.se/pressroom/newsdesk/contact_person/list