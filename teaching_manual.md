# Teaching Manual

## Voraussetzungen:

- Chrome
- Chrome-Erweiterung Scraper
- ein Tabellenbearbeitungsprogramm (Libre Office, Excel,...)

Basierend auf https://librarycarpentry.org/lc-webscraping/

## Intro Webscraping

Web scraping is a technique for extracting information from websites. 
This can be done manually but it is usually faster, more efficient and less error-prone if it can be automated.

Web scraping allows you to convert non-tabular or poorly structured data into a usable, 
structured format, such as a .csv file or spreadsheet.

Web scraping is also increasingly being used by scholars to create data sets for text mining projects, 
say, collections of journal articles or digitised texts.

**Key Points:**
- Humans are good at categorizing information, computers not so much.
- Often, data on a web site is not properly structured, making its extraction difficult.
- Web scraping is the process of automating the extraction of data from web sites.

## Intro HTML
Wie in DH-Intro MedWiss

## Webscraping mit Scraper 1
Wie in DH-Intro MedWiss:

Liste der beliebtesten Filme mit Scraper auslesen und nach Excel kopieren.

## Intro XPath
XPath (XML Path Language) ist eine Ausdruckssprache, um spezifische Ausdrücke/Elemente von XML-Dokumenten anzusteuern und auszuwählen.

> XML, eXtended Markup Language, ist wie HTML, Hypertext Markup Language, eine Auszeichnungssprache, 
mit denen Dokumente über sog. Tags in Elemente strukturiert sind. HTML ist gewissermaßen ein Dialekt von XML.

Da HTML und XML gleich strukturiert sind, kann  XPath sowohl auf HTML- als auch XML-Dokumente angewendet werden.

XML/HTML-Dokumente sind streng hierarchisch als Baumstruktur organisiert, das sog. DOM, Document Object Model. 
Demnach ist jedes Element ein *node*/*Knoten*. Der Text innerhalb eines Knoten ist ein "Text-Knoten".
![DOM](https://www.w3schools.com/js/pic_htmltree.gif)

Jeder Knoten hat 
* genau ein Elternteil/*parent* (bis auf das Wurzelelement/*root*),
* 0 - n Kinder/*child*
* 0 - n Geschwister/*siblings* (Siblings sind Knoten mit dem gleichen Elternteil).

Mit XPath kann jeder Knotenpunkt angesteuert werden und der Inhalt des Knotens ausgewählt werden. Die Knoten sind über Beziehungen miteinander verbunden, sog. *path*:

![Node Relationships](https://www.w3schools.com/js/pic_navigate.gif)

### Beispiel
1. webseite aufrufen: https://www.imdb.com/chart/top?ref_=nv_mv_250
1. Quelltext anzeigen lassen (rechtsklick)
1. Struktur: html-head-title
1. Console öffnen und unten in den Editor eingeben: `$x("/html/head/title/text()")`

Mit $x("xxx") wird eine XPath-Suche durchgeführt, `/` navigiert von Knoten zu Knoten, `html`, `head`, `title` sind die Knoten-Namen, `text()` wählt den Text-Knoten aus (d.h. den Text, der zwischen den Tags steht).

Es werden zwei Ergebnisse ausgegeben, da im Head 2 Title-Tags vorhanden sind. Um nur ein spezifisches Element anzusteuern, kann die Index-Nummer mit angegeben werden: `$x("/html/head/title[1]/text()")`

Um den gesamten Knoten auszuwählen (und nicht nur das Textelement) können Sie `$x("/html/head/title[1]")` eingeben.

Um nicht nur den title-Knoten unter html/head zu suchen, sondern alle title-Knoten auf einer Seite, können sie `//`voranstellen. Suchen Sie auf der IMDb-Seite alle Tabellenzeilen (tr):

`$x("//tr")`

>Damit bekommen Sie mehr als die 250 Einträge der Tabelle, warum? (Schauen Sie sich die ersten und letzten Einträge an!)
>Die Tabellenüberschriftzeile ist enthalten und der footer ist auch als Tabelle formatiert!

Um die Tabellenüberschrift auszuschließen, können wir noch den den `tr` übergeordneten Knoten auswählen: Eine Tabelle ist (normalierweise) strukturiert table-thead-tbody. Die Überschrift ist unterhalb des `thead`-Knotens, der Tabelleninhalt unterhalb des `tbody`-Knotens: `$x("//tbody/tr")`

Damit bekommen wir aber immer noch die Tabellenzeiten aus dem Footer. Das Table-Tag der Filmliste hat noch zusätzlich eine *class*-Angabe: `<tbody class="lister-list">`. Die class können wir nutzen, um die Ergebnisse einzuschränken: `$x("//tbody[@class='lister-list']/tr")`

>Weitere Vertiefung von XPath nach dem Tutorial, oder reicht das als Intro? Noch Auswahl Parent-Element? Operators, Booleans?
>Wozu überhaupt Parent/Child/Siblings-Differenzierung?

## Webscraping mit Scraper 2
https://librarycarpentry.org/lc-webscraping/03-manual-scraping/index.html

Vllt. Ergebnisliste des K+ nehmen (https://hds.hebis.de/ubmr/Search/Results?lookfor=digital+humanities&type=allfields&filters=on&view=list)
Da dann auch or, id, ... möglich?
