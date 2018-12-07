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

Mit `$x("xxx")` wird eine XPath-Suche durchgeführt, `/` navigiert von Knoten zu Knoten, `html`, `head`, `title` sind die Knoten-Namen, `text()` wählt den Text-Knoten aus (d.h. den Text, der zwischen den Tags steht).

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


## Eigenarbeit

Extrahieren Sie alle Zeitschriftenartikel der Zs Glossa (https://www.glossa-journal.org/articles/).
Die Anzahl der angezeigten Artikel können Sie manuel einstellen: unten 100 auswählen. Dann URL anschauen und `app=300` am Ende er-/einsetzen.

1. Schreiben Sie ein Xpath, um sich alle Artikel auszuwählen
1. Schreiben Sie ein Xpath, um alle Autoren auszuwählen
1. Schreiben Sie ein Xpath, um alle Titel auszuwählen
1. Schreiben Sie ein Xpath, um alle Artikel-URLs auszuwählen
1. Schreiben Sie ein Xpath, um alle Publikationsdaten auszuwählen
1. Schreiben Sie ein Xpath, um alle Zeitschriftenhefte (volume, issue) auszuwählen.

>Seite öffnen, Inspector öffnen und Listenelement auswählen:
>1. Artikel: `$x("//li[@class='article-block']")` so auch in Scraper
>1. Autor: `$x("//p[@class='article-author']/text()")` bzw. in Scraper: `./div/div/a/p[@class='article-author']`
>1. Titel: `$x("//p[@class='article-author']/preceding-sibling::h5")` bzw. in Scraper: `./preceding-sibling::h5`  (Manchmal kann man ein Element nicht direkt auswählen, weil es keine hinreichende ID hat. Dann kann man z.B. das nachfolgende/vorangehende Element auswählen und `/preceding-sibling::h5` oder `/child::*` oder `/parent::*` angeben: Wähle jedes vorangehende Geschwister der Art h5 aus; wähle jedes direkte Kind aller Typen aus; wähle das Elternelement aus.)
>1. URL: `$x("//p[@class='article-author']/parent::*/@href")` bzw. in Scraper: `./parent::*/@href`  gibt nur die relative URL an: mit Concatencate kann die URL ergänzt werden:
`$x("concat('https://www.glossa-journal.org', //p[@class='article-author']/parent::*/@href)")` bzw.  in Scraper: `concat('https://www.glossa-journal.org', ./parent::*/@href)`
>1. Datum: `$x("//p[@class='article-date']/text()")` bzw. in Scraper: `//p[@class='article-date']`
>1. Volume/Issue: `$x("//div[@class='article-actions']/div[@class='aside']/a/text()")` bzw. in Scraper: `//div[@class='article-actions']/div[@class='aside']/a/text()`
 
 **Die letzten beiden funktionieren nicht in Scraper!!! Da wird nur der erste Wert ausgelesen und immer wiederholt!!**


## Webscraping mit Scraper 2
https://librarycarpentry.org/lc-webscraping/03-manual-scraping/index.html

Jetzt werden die Ergebnisse aus der Console mit Scraper wiederholt!
1. Auf der Webseite einen Artikel markieren und Scrape similar auswählen.
1. Den ersten XPath eingeben
1. Spalten hinzufügen mit dem grünen Plus
1. Spaltennamen hinzufügen
1. jetzt die anderen Befehle als jeweils neue Spalte einfügen
1. Ergebnisse in die Zwischenablage und exportieren



