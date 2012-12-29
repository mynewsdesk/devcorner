---
title: Hårdvaruhacking i en post-PC-värld
author: Jan Andersson
layout: post
categories:
  - Arduino
  - Hardware
---
**Vägen hit**

I begynnelsen (även känt som 90-talet) fanns idén om att en central dator skulle styra och övervaka hemmet. Genom den skulle du kunna hyra film, kontrollera mjölken i kylen och sätta på larmet.

Sedan kom genombrottet för bärbara datorer, netbooks, smarta mobiler och surfplattor. Den centrala datorn kändes allt mer avlägsen och Apple började prata om en “Post-PC world” där PC:n inte längre står i centrum utan många små enheter löser allt mer specialiserade problem i våra liv.

Open source-rörelsen har hela tiden legat i framkant av utvecklingen och utan den skulle våra datorer och enheter skulle se väldigt annorlunda ut. Det faller sig därför naturligt att en open-source-plattform tar plats i vår Post-PC-värld, på att sätt som [Arduino][1] har gjort. Det är en plattform för att vi som inte är civilingenjörer i elektroteknik ska kunna ta fram objekt som interagerar med sin omgivning.

<a rel="attachment wp-att-853" href="http://devcorner.mynewsdesk.com/2011/06/15/hardvaruhacking-i-en-post-pc-varld/narbild/"><img class="alignright size-medium wp-image-853" title="närbild" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/06/närbild-300x199.jpg" alt="" width="300" height="199" /></a>

Arduino startades i Ivrea, Italien av Massimo Banzi och David Cuartielles 2005. Då universitetet hotades av nedläggning och de var rädda att allt skulle läggas i malpåse, släpptes såväl kod som hårdvarudesign under öppna licenser. Sedan dess har projektet rönt stor uppmärksamhet. Att även hårdvarudesignen har öppen licens gör att vem som helst kan producera egna kort, vilket gör det svårt att veta hur många som har släppts, men förra året hade 120 000 officiella Arduino producerats, med målet att nå en miljon i år. På Googles senaste utvecklarkonferens presenterades deras planer på att släppa en egen version, Google Accessory Development Toolkit, specialiserad för Android-utvecklare.

<span style="color: #000000;"><strong>Skapandet av en timer</strong></span>

<a rel="attachment wp-att-854" href="http://devcorner.mynewsdesk.com/2011/06/15/hardvaruhacking-i-en-post-pc-varld/oversikt/"><img class="alignleft size-medium wp-image-854" title="översikt" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/06/versikt-300x199.jpg" alt="" width="300" height="199" /></a>Jag är normalt utvecklare av webbapplikationer men Arduino känns alltför intressant för att inte prova på. Jag beställde därför en officiell Arduino Uno från leverantören Seeed Studio, baserad i Shenzhen, Kina. Bortsett från att det var billigare att ta varvet runt jorden så erbjöd de också en prisvärt sidekick, ett kit med en uppsättning komponenter att experimentera med. Totalt kostade allt ringa 320 kr, fritt porto.

Själva Arduinon levereras i en liten ask som är ungefär hälften så stor som en iPhone. Hjärtat i Arduino är en enchipsdator med namnet Atmel ATmega328. Den har 32 kB flashminne och bara 2 kb RAM. Med dessa begränsade resurser har folk byggt allt från [musikinstrument][2] till [quadcopters][3].

<a rel="attachment wp-att-856" href="http://devcorner.mynewsdesk.com/2011/06/15/hardvaruhacking-i-en-post-pc-varld/sidekickbk1_01/"><img class="alignleft size-medium wp-image-856" title="sidekickbk1_01" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/06/sidekickbk1_01-300x225.jpg" alt="" width="300" height="225" /></a>Bland tillbehören fanns en liten breadboard, det vill säga en platta med små hål att sticka komponenter i, en handfull sladdar några sensorer (ljus, lutning, värme, tryck, vrid) och några enheter att påverka (servomotor, lysdioder, högtalare), motstånd, kondensatorer etc. Tillbehören är utmärkt dokumenterade på [Seeed Studios wiki][4].

Jag tänkte begränsa mitt första projekt till en timer, fast inställd på 25 minuter enligt projektmetoden Pomodoro. Den kan alltså användas som alternativ till äggklocka för att se när det är dags att ta paus. Tanken var att fem lysdioder ska blinka, en i taget, för att sedan hissa en flagga. En knapptryckning skall starta om timern.

Det är slående hur pass enkelt det är att få ihop ett fungerande projekt som detta. Elektronik brukade vara komplicerat och tidskrävande, som jag minns det från min begränsade erfarenhet under gymnasietiden. Logiken består till största del av programkod. Arduino tillhandahåller ett litet men kompetent IDE. Själva programkoden skrivs i en förenklad form av C som sedan prekompileras till ett fullfjädrat C-projekt som kompileras och via USB förs över till Arduinon. Det tar bara någon sekund eller två att genom ett knapptryckning kompilera, ladda upp och driftsätta koden. Sedan kan man helt koppla bort datorn och Arduinon är självgående, förutsatt man man ger ström via något annat än USB, exempelvis ett 9V-batteri.

<a rel="attachment wp-att-848" href="http://devcorner.mynewsdesk.com/2011/06/15/hardvaruhacking-i-en-post-pc-varld/schema/"><img title="schema" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/06/schema-600x462.png" alt="" width="600" height="462" /></a>

Jag blev nöjd med resultatet! Den kanske inte är så praktisk att använda men att paketera den i en låda ingår inte i projektet. Jag blev klar så pass snabbt att jag la till en högtalare som spelar Gubben Noak när tiden gått ut.

**Dokumentation**

Till skillnad från rena mjukvaruprojekt som man kan arkivera eller driftsätta när man är klar så är det inte lika enkelt med hårdvara. För att inte den biten skulle vara förgäves efter att jag plockat tillbaka alla komponenter i lådan ville jag dokumentera allt. Jag hittade den eminenta applikationen [Fritzing][5] som tagit fram i samma anda av open source och lekfullhet som Arduino. Med den kan man med ett snyggt och effektivt GUI.

Fritzing erbjuder tre vyer:

*   *Breadboard* där man drar ut komponenter på en breadboard och en Arduino på ett realistiskt sätt
*   *Schematic* som ger en schematisk överblick av projektet
*   *PCB* som ger en kretskortsvy

Om man vill serieproducera sin produkt, eller åtminstone göra en mer stabil produkt som inte kräver lösa komponenter på en breadboard, kan man använda PCB-vyn för att producera ett kretskort att löda fast komponenterna på.

<a rel="attachment wp-att-855" href="http://devcorner.mynewsdesk.com/2011/06/15/hardvaruhacking-i-en-post-pc-varld/skarmavbild-2011-06-15-kl-11-38-23/"><img class="alignnone size-large wp-image-855" title="Skärmavbild 2011-06-15 kl. 11.38.23" src="http://devcorner.mynewsdesk.com/wp-content/uploads/2011/06/Skärmavbild-2011-06-15-kl.-11.38.23-600x213.png" alt="" width="600" height="213" /></a>

Du kan [ladda ned dokumentation och kod här][6], open source under MIT-licens.

**Framtiden**

Arduino lär fortsätta att ta världen med storm. Det öppnar upp för helt nya typer av lösningar. Det finns gott om tillbehör i form av så kallade sköldar som staplar ovanpå Arduinon. Till dem hör Ethernet Shield som ger en fullfjädrad TCP-IP-stack. Den finns stor potential att utveckla produkter med, exempelvis en Rails-baserad server på Heroku som kommunicerar med ett antal Arduinos som analyserar och påverkar världen runt omkring.

 [1]: http://arduino.cc/
 [2]: http://www.critterandguitari.com/home/store/arduino-piano.php
 [3]: http://aeroquad.com/
 [4]: http://garden.seeedstudio.com/index.php?title=Arduino_Sidekick_Basic_Kit
 [5]: http://fritzing.org/
 [6]: https://github.com/janne/arduino_pomodoro