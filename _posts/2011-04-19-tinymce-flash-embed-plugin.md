---
title: TinyMCE Flash Embed Plugin
author: Joakim Westerlund
layout: post
categories:
  - Editor
  - Front End
  - GUI
tags:
  - '@media'
  - editor
  - flash embed
  - javascript
  - plugins
  - TinyMCE
---
[TinyMCE][1] är en **WYSIWYG Editor** som vi använder för de företag som publicerar sina nyheter via Mynewsdesk. **TinyMCE** är en populär editor med stor användarbas som bland annat används i grundutförandet av [WordPress][2]. Fördelen med editorn är att den går enkelt att konfigurera efter sina speciella behov genom att använda sig av inställningar eller lägga till olika plugins som följer med.

På Mynewsdesk har vi använt TinyMCE sedan några år tillbaka och har försökt att hänga med uppgraderingar av editorn så gott som möjligt. Då den medföljande **media pluginen** i TinyMCE är alldeles för komplex för våra användare så har vi haft en egen variant till att **infoga flash kod** genom editorn.

Tyvärr har vi märkt att det ibland har blivit eftersatt med uppdateringar då det bara var vi själva som använde vår specialdesignade **infoga flash media plugin för TinyMCE** som vi använde tidigare.

Efter en av de senare uppgraderingarna av TinyMCE vi gjorde i och med att stödet för Internet Exlorer 9 uppdaterades råkade vi ut för lite konstiga problem. Istället för att ändra i den gamla befintliga koden för vår plugin beslöt jag mig för att avsätta två **lab days** till att lära mig skriva plugins för TinyMCE och samtidigt dela med oss av en plugin till TinyMCE som även andra kunde ha nytta för.

Resultatet blev en plugin som fått namnet [TinyMCE Raw Embed Plugin][3] och är en väldigt enkel plugin som innehåller möjligheten att infoga en flash object/embed kod i en dialog med en textarea.

![Dialog Insert flash embed TinyMCE plugin](/images/wp/tinymce-flash-dialog.png)

Samtidigt finns möjligheten att direkt i HTML-läget infoga embed-kod och då visas det på samma sätt som det gör genom infogande via knappen och dialogen.

![TinyMCE Flash Raw Embed Plugin example view](/images/wp/tinymce-raw-embed.png)

För att använda sig av pluginen lägger man rawembed-mappen i sin TinyMCE plugins-katalog. Sedan konfigurerar man initieringen av editorn så att pluginen laddas och knappen visas i editorn.



Idén till denna plugin kom från den ursprungliga media plugin som finns med i TinyMCE och är en rejält nedbantad variant av den pluginen.

**TinyMCE Raw Embed Plugin** ligger på [Github][3] med mer information och ett exempel på initieringen av editorn, hur man konfigurerar knappen i verktygsfältet samt hur man lägger till rawembed pluginen i TinyMCE .

*Om du vill testa skapa din egen plugin till TinyMCE så finns ett plugin-template med instruktioner hur man går tillväga på [deras webbplats][4]. Värt att tänka på är att om man ska använda sig av egen design så är det snyggast enligt mig att ha det i en separat css i plugin katalogen och ladda denna genom plugin:en då det initieras. Detta löste jag genom att lägga till en extra CSS till editorn i pluginens init metod genom följande kod.*

 [1]: http://tinymce.moxiecode.com/
 [2]: http://codex.wordpress.org/TinyMCE "TinyMCE WordPress"
 [3]: https://github.com/jorkas/tinymce-rawembed-plugin "TinyMCE flash embed plugin"
 [4]: http://tinymce.moxiecode.com/wiki.php/Creating_a_plugin "Create a TinyMCE plugin"