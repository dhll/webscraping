# Teaching Manual

## Voraussetzungen:

- Chrome
- Chrome-Erweiterung Scraper

## Intro Webscraping

Webscraping ist eine Technik, um Informationen aus Webseiten zu extrahieren ("kratzen"), anstatt diese manuell per Copy & Paste zu sammeln.
In diesem Workshop lernen Sie die Grundlagen von Webscraping kennen mithilfe der Chrome-Erweiterung Scraper sowie einige notwendige Basics von 
* HTML und
* XPath

In dem Workshop soll 
* die Liste der 250 beliebtesten Filme der IMDb gescrapt werden (https://www.imdb.com/chart/top?ref_=nv_mv_250) und
* Informationen zu den in der Zeitschrift Glossa (https://www.glossa-journal.org/articles/) erschienenen Artikel aus der Webseite extrahiert werden.

## Intro HTML
Um eine Einführung in HTML zu bekommen, ist folgende Seite sehr hilfreich:

> (https://www.w3schools.com/html/html_intro.asp) aufrufen. Zur Illustration am Seitenende scrollen ("HTML Page Structure")

Alles auf einer Webseite steht zwischen Tags, die das Element, das sie umfassen definieren.
Das, was Sie auf einer Webseite sehen, steht zwischen den sog. `<body>-Tags`. Body definiert, dass alles, was dazwischen steht, auf der Webseite (in irgendeiner Form) erscheint.  
Vor dem Body steht der `<head>-Tag`. Im Head sind sog. Metadaten enthalten, z.B. der Titel der Webseite (das was Sie in der Kopfzeile des Browsers sehen), aber noch viel mehr.  
Ganz am Anfang einer html-Seite wird der Dokumententyp definiert, so dass der Webbrowser weiß, um was für ein Dokument es sich handelt (z.B. html, oder pdf, oder Bild, ...)

> (https://www.w3schools.com/html/html_basic.asp) aufrufen.

Alle Elemente einer Webseite, sind in Tags eingeschlossen, die definieren, um was es sich handelt. Z.B. **Überschriften** in versch. Ebenen, oder **Absätze**, **Links** oder **Bilder**.

> Jeweils Click auf `Try it Yourself`

* `<hx>` **Überschriften**: Strukturieren das Dokument.
* `<p>` **Absätze**: Es kann einen Unterschied geben zwischen der Darstellung auf der Webseite und den Zeichen im Quelltext: HTML unterdrückt extra Leerzeichen oder Zeilenumbrüche.
* `<a>` **Links**
* `<img>` **Bilder**

Bei **Bilder** und **Links** sehen Sie, dass die Tags auch noch _Attribute_ haben (können), z.B. beim Link ist die URL angegeben, die durch Klick aufgerufen wird. Der dargestellte Text steht (wie üblich) zwischen den Tags. Beim `<img>`-Tag sind mehrere Attribute aufgeführt: die Quelle/die Datei; ein Alternativtext, der von Screenreadern gelesen wird oder das Bild in Suchmaschinen finden lässt; sowie die Größenangaben. 

Im "realen Leben" sind die Webseiten sehr komplex aufgebaut und die Tags vielfach ineinander geschachtelt. In den Beispielen ist das alles noch sehr übersichtlich.

Einen ersten Eindruck, wie solche Verschachtelungen aussehen, gibt die Formatierung von **Tabellen**:

* `<table>`
  * `<tr>` Table row
    * `<th>` Table Heading
    * `<td>` Table data (= Zelle)

Tabellen sind in HTML zeilenweise aufgebaut; d.h. dass es nicht möglich ist, eine Spalte direkt auszuwählen.

Mithilfe der Tags ist es möglich, spezifische Elemente einer Webseite abzufragen, um sie für das Datamining/die Datenanalyse zu nutzen.

Genau das machen wir jetzt mit der Liste aus der IMDb, diese Liste ist als Tabelle strukturiert.

## Webscraping mit Scraper 1

Liste der beliebtesten Filme mit Scraper auslesen und nach Excel kopieren.

> (https://www.imdb.com/chart/top?ref_=nv_mv_250) aufrufen.

Tabelle und HTML-Code nebeneinander und anhand des ersten Eintrags "Shawshank Redemption" zeigen, von wo bis wo das `<tr>` und `<td>` geht, dass es zahlreiche weitere Informationen gibt, die nicht sichtbar sind.

> In den Tabellenkopf mit Rechtsclick "Scrape Similar" auswählen.
Scraper sollte so alle Spaltenüberschriften extrahiert haben.

> In der Titelzeile mit Rechtsclick "Scrape Similar" auswählen. 
Scraper sollte so eine komplette Zeile extrahiert haben.

Um alle 250 Titel zu extrahieren, muss eine komplette Zeile markiert werden:

> Mit der Maus eine komplette `<tr>` markieren und dann per Rechtsklick scrapen.

Oben links stehen die Tags (im XPath-Feld).
Rechts im Scraper-Hauptfenster sehen Sie die extrahierten Elemente als Spalten angezeigt. Einzelne Spalten können über das Columns-Menü links gelöscht werden.

>Löschen Sie alle Spalten bis auf Rank&Title. Führen Sie Scrape erneut aus (Button unten).

Copy to clipboard, wenn alle 250 Einträge erfasst sind. Einfügen in Excel/LibreOffice. Aus welchem Jahr gibt es die meisten Filme in der Liste?


## Intro XPath

In Scraper stehen oben links im Selector-Menü nicht die HTML-Tags, die ausgewählt werden, sondern der XPath-Ausdruck, um die betreffenden Tags auszuwählen.

XPath (XML Path Language) ist eine Ausdruckssprache, um spezifische Ausdrücke/Elemente von XML-Dokumenten anzusteuern und auszuwählen.

> XML, eXtended Markup Language, ist wie HTML, Hypertext Markup Language, eine Auszeichnungssprache, 
mit denen Dokumente über sog. Tags in Elemente strukturiert sind. HTML ist gewissermaßen ein Dialekt von XML.

Da HTML und XML gleich strukturiert sind, kann XPath sowohl auf HTML- als auch XML-Dokumente angewendet werden.

XML/HTML-Dokumente sind streng hierarchisch als Baumstruktur organisiert, das sog. DOM, Document Object Model. 
Demnach ist jedes Element ein *node*/*Knoten*. Der Text innerhalb eines Knoten ist ein "Text-Knoten".
![DOM](https://www.w3schools.com/js/pic_htmltree.gif)

Jeder Knoten hat 
* genau ein Elternteil/*parent* (bis auf das Wurzelelement/*root*),
* 0 - n Kinder/*children*
* 0 - n Geschwister/*siblings* (Siblings sind Knoten mit dem gleichen Elternteil).

Mit XPath kann jeder Knotenpunkt angesteuert werden und der Inhalt des Knotens ausgewählt werden. Die Knoten sind über Beziehungen miteinander verbunden, sog. *path*:

![Node Relationships](https://www.w3schools.com/js/pic_navigate.gif)

### Beispiel
1. webseite aufrufen: https://www.imdb.com/chart/top?ref_=nv_mv_250
1. Quelltext anzeigen lassen (rechtsklick)
1. Struktur: html-head-title
1. Console öffnen und unten in den Editor eingeben: `$x("/html/head/title/text()")`

Mit `$x("xxx")` wird eine XPath-Suche durchgeführt, `/` navigiert von Knoten zu Knoten, `html`, `head`, `title` sind die Knoten-Namen, `text()` wählt den Text-Knoten aus (d.h. den Text, der zwischen den Tags steht).

Es werden zwei Ergebnisse ausgegeben, da im Head 2 Title-Tags vorhanden sind. 
In Chrome ist die Anzeige in der Console etwas unübersichtlich. Click auf die Index-Nummern springt zur HTML-Zeile. (Die Anzeige der Index-Nummern in Chrome fängt mit 0 an, die Auswahl jedoch fängt mit `[1]` an!)

Um nur ein spezifisches Element anzusteuern, kann die Index-Nummer mit angegeben werden: `$x("/html/head/title[1]/text()")`

Um den gesamten Knoten auszuwählen (und nicht nur das Textelement) können Sie `$x("/html/head/title[1]")` eingeben.

Um nicht nur den title-Knoten unter html/head zu suchen, sondern alle title-Knoten auf einer Seite, können sie `//`voranstellen. Suchen Sie auf der IMDb-Seite alle Tabellenzeilen (tr):

`$x("//tr")`

>Damit bekommen Sie mehr als die 250 Einträge der Tabelle, warum? (Schauen Sie sich die ersten und letzten Einträge an!)
>Die Tabellenüberschriftzeile ist enthalten und der footer ist auch als Tabelle formatiert!

Um die Tabellenüberschrift auszuschließen, können wir noch den den `tr` übergeordneten Knoten auswählen: Eine Tabelle ist (normalierweise) strukturiert table-thead-tbody. Die Überschrift ist unterhalb des `thead`-Knotens, der Tabelleninhalt unterhalb des `tbody`-Knotens: `$x("//tbody/tr")`

Damit bekommen wir aber immer noch die Tabellenzeiten aus dem Footer. Das Table-Tag der Filmliste hat noch zusätzlich eine *class*-Angabe: `<tbody class="lister-list">`. Die class können wir nutzen, um die Ergebnisse einzuschränken. Dazu geben wir die Class an, wie auch die Indexnummern (in eckigen Klammern): `$x("//tbody[@class='lister-list']/tr")`

>Weitere Vertiefung von XPath nach dem Tutorial, oder reicht das als Intro? Noch Auswahl Parent-Element? Operators, Booleans?
>Wozu überhaupt Parent/Child/Siblings-Differenzierung?

## Cheat Sheat


| XPath | Erläuterung  |
| --- | ---  |
| `nodename` | Elementname |
| / | Pfad zwischen zwei Elementen |
| // | wählt alle Elemente aus |
| . | aktueller Node |
| .. | wechselt zum Parentnode |
| `text()` | wählt nur den Text eines Elementes aus |
| `[1]` | Indexnummer: Wählt das erste/zweite/... Element aus |
| `[@class]` | wählt den Node mit dem Attribut `class` aus |
| `[@class='Name']` | wählt den Node mit dem Attribut `class` und dem Attribut-Wert "Name" aus |
| `::` | Relationship-Auswahl |


## Eigenarbeit

Extrahieren Sie alle Zeitschriftenartikel der Zs Glossa (https://www.glossa-journal.org/articles/).
Die Anzahl der angezeigten Artikel können Sie manuel einstellen: unten 100 auswählen. Dann URL anschauen und `app=400` am Ende er-/einsetzen (bzw. bis unten keine weitere Seite mehr angezeigt wird).

1. Schreiben Sie ein Xpath, um sich alle Artikel auszuwählen
1. Schreiben Sie ein Xpath, um alle Autoren auszuwählen
1. Schreiben Sie ein Xpath, um alle Titel auszuwählen
1. Schreiben Sie ein Xpath, um alle Artikel-URLs auszuwählen
1. Schreiben Sie ein Xpath, um alle Publikationsdaten auszuwählen
1. Schreiben Sie ein Xpath, um alle Zeitschriftenhefte (volume, issue) auszuwählen.

Es kann helfen, sich die DOM-Struktur (für die relevanten Elemente) zu skizzieren.

>Seite öffnen, Inspector öffnen und Listenelement auswählen:
>1. **Artikel**: `$x("//li[@class='article-block']")`
>1. **Autor**: `$x("//p[@class='article-author']/text()")`
>1. **Titel**: `$x("//p[@class='article-author']/preceding-sibling::h5")`  (Manchmal kann man ein Element nicht direkt auswählen, weil es keine hinreichende ID hat. Dann kann man z.B. das nachfolgende/vorangehende Element auswählen und `/preceding-sibling::h5` oder `/child::*` oder `/parent::*` angeben: Wähle jedes vorangehende Geschwister der Art h5 aus; wähle jedes direkte Kind aller Typen aus; wähle das Elternelement aus.)
>1. **URL**: `$x("//p[@class='article-author']/parent::*/@href")` Eine Kurzform ist: `../@href` (`..` wählt parent-node aus, `.` wählt aktuellen node aus.)
Gibt nur die relative URL an: mit Concatencate kann die URL ergänzt werden:
`$x("concat('https://www.glossa-journal.org', //p[@class='article-author']/parent::*/@href)")`
>1. **Datum**: `$x("//p[@class='article-date']/text()")`
>1. **Volume/Issue**: `$x("//div[@class='article-actions']/div[@class='aside']/a/text()")`
 
 Zur Visualisierung der "Familienstrukturen" in XPath:
 ![XPath axes](https://kimpham54.github.io/library-webscraping/fig/xpath-axes.jpg)
 
 **Bug: es werden nicht alle Artikel-URLs gefunden!!**

## Webscraping mit Scraper 2
https://librarycarpentry.org/lc-webscraping/03-manual-scraping/index.html

Jetzt werden die Ergebnisse aus der Console mit Scraper wiederholt!
1. Auf der Webseite einen Artikel markieren und Scrape similar auswählen.
1. Den ersten XPath eingeben
1. Spalten hinzufügen mit dem grünen Plus
1. Spaltennamen hinzufügen
1. jetzt die anderen Befehle als jeweils neue Spalte einfügen
1. Ergebnisse in die Zwischenablage und exportieren

>1. **Artikel**: in Scraper `//div[@class='caption-text']`
>1. **Autor**: in Scraper: `./*/p[@class='article-author']`
>1. **Titel**: in Scraper: `./*/h5`  (`*` = Wildcard: alle beliebigen nodes, die ein Unterelement `h5` haben.)
>1. **URL**: in Scraper: `./a/@href`. Gibt nur die relative URL an: mit Concatenate kann die URL ergänzt werden: `concat('https://www.glossa-journal.org', ./a/@href)`
>1. **Datum**: in Scraper: `./following-sibling::div/p[@class='article-date']`
>1. **Volume/Issue**: in Scraper: `./following-sibling::div/div[@class='aside']/a/text()` oder: `../*/div[@class='aside']/a/text()` oder: `./parent::*/*/div[@class='aside']/a/text()` (`..` wählt parent-node aus, `.` wählt aktuellen node aus.)

## Weitere Tuturials:
* https://librarycarpentry.org/lc-webscraping/
* https://www.w3schools.com/xml/xpath_intro.asp
