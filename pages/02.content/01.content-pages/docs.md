---
title: Pages
taxonomy:
    category: docs
process:
    twig: true
---

In der Grav-Sprache sind **Pages** die fundamentalen Bausteine Ihrer Website. Sie stellen die Grundlage für das Schreiben von Inhalten und die Navigation im Grav-System dar.

Die Kombination von Content und Navigation stellt sicher, dass das System auch für die unerfahrensten Autoren von Inhalt-Seiten intuitiv zu bedienen ist. In Verbindung mit leistungsstarken Taxonomie-Funktionen ist dieses System trotzdem mächtig genug, um komplexe Anforderungen an die Darstellung von Inhalten zu erfüllen.

Grav unterstützt von Haus aus **3 Seitentypen**, mit denen Sie eine umfangreiche Palette von Web-Inhalten erstellen können. Diese Typen sind:

![Grav Page Types](page-types.png)

#### Reguläre Seite

![Standard Page](content-standard.png)

Eine reguläre Seite ist allgemein eine einzelne Seite wie z.B. ein **Blogbeitrag**, **Kontaktformular**, **Fehlerseite** usw. Das ist die häufigste Seitenart, die Sie erstellen werden. Eine Seite wird standardmäßig als reguläre Seite betrachtet, sofern Sie Grav nichts anderes vorgeben.

Wenn Sie das **Core Grav-Paket** heruntergeladen und installiert haben, werden Sie von einer Standardseite begrüßt. Wir haben die Erstellung einer einfachen regulären Seite im Kapitel [Erste Schritte](/basics/basic-tutorial) behandelt.

#### Listing Seite

![Listing Page](content-listing.png)

Hierbei handelt es sich um eine Erweiterung einer regulären Seite. Die Seite verweist auf eine Kollektion von Einzelseiten.

Der einfachste Weg, diese einzurichten, ist die Erstellung von **„Kinderseiten“** unterhalb der Listing-Seite. Ein Beispiel dafür wäre eine **Blog-Listing-Seite**, auf der Sie eine Liste mit einer Zusammenfassung der Blog-Beiträge anzeigen würden, die als untergeordnete Seiten existieren.

Außerdem finden Sie einige Einstellungen zur **Kontrolle der Reihenfolge** der Einträge sowie ein **Limit für die Anzahl der Einträge** und ob die **Seitennummerierung** aktiviert werden soll oder nicht.

!! Ein Beispiel für ein **Blog-Skeleton** unter Verwendung einer **Listing-Seite** finden Sie bei den [Grav Downloads](https://getgrav.org/downloads/skeletons).

#### Modulare Seite

![Modular Page](content-modular.png)

Eine Modulare Seite ist eine besondere Art von Listing-Seite, weil sie eine **einzelne modulare Seite** aus ihren **nachgeordneten modularen Unterseiten** aufbaut. Dies erlaubt die Erstellung sehr komplexer **Einseiten-Layouts** aus den Modulen. Dazu wird die **modulare Seite** aus mehreren **modularen Unterseiten-Ordnern** aufgebaut, die sich im Hauptordner der modularen Seite befinden.

!! Ein Beispiel für ein **One-Page Skeleton** unter Verwendung einer **modularen Seite** finden Sie in den [Grav Downloads](https://getgrav.org/downloads/skeletons).

Jeder dieser Seitentypen folgt der gleichen Grundstruktur. Bevor wir uns also mit den Einzelheiten der einzelnen Typen befassen können, müssen wir den Aufbau der Seiten in Grav näher betrachten.

!! Ein Modul ist, da es als Teil einer anderen Seite konzipiert ist, von Natur aus keine Seite, die Sie direkt über eine URL erreichen können. Aus diesem Grund sind alle modularen Seiten standardmäßig als **nicht routingfähig** gekennzeichnet.

## Ordner

Alle Inhaltsseiten befinden sich im Ordner `/user/pages`. Jede **Page** sollte in einen eigenen Ordner gelegt werden.

!! Ordnernamen sollten ebenfalls gültige [**Slugs**](https://de.ryte.com/wiki/Slug) sein. Die Slugs werden vollständig kleingeschrieben, wobei Akzentzeichen durch Buchstaben des lateinischen Alphabets und Leerzeichen durch einen Bindestrich oder einen Unterstrich ersetzt werden, um zu verhindern, dass sie kodiert werden.

Grav begreift jeden ganzzahligen Wert, dem ein Punkt folgt, als reine Sortierfunktion und der systemintern entfernt wird. Wenn Sie beispielsweise einen Ordner mit dem Namen `01.home` haben, behandelt Grav diesen Ordner als `home`, stellt aber sicher, dass er bei der Standardreihenfolge vor `02.blog` liegt.

[prism classes="language-txt line-numbers"]
/user
└── /pages
    ├── /01.home
    │   ├── /_header
    │   ├── /_features
    │   ├── /_body
    ├── /02.blog
    │   ├── /blog-item-1
    │   ├── /blog-item-2
    │   ├── /blog-item-3
    │   ├── /blog-item-4
    │   └── /blog-item-5
    ├── /03.about-us
    └── /error
[/prism]

Ihre Website muss einen Startpunkt haben, damit sie weiß, wohin sie springen muss, wenn Sie Ihren Browser auf das Stammverzeichnis Ihrer Website richten. Wenn Sie zum Beispiel `http://yoursite.com` in Ihren Browser eingeben, erwartet Grav standardmäßig einen Alias `home/`. Sie können jedoch den Home-Punkt überschreiben, indem Sie die Option `home.alias` in der [Grav-Konfigurationsdatei](/basics/grav-configuration) ändern.

**Modulare Unterseiten** sind durch einen Unterstrich (`_`) vor dem Ordnernamen gekennzeichnet. Dies ist ein spezieller Ordnertyp, der nur für die Verwendung mit **modularem Inhalt** vorgesehen ist.  Diese sind **nicht routingfähig** und in der Navigation **nicht sichtbar**. Ein Beispiel für einen modularen Seitenaufbau wäre ein Ordner wie z.B. `user/pages/01.home`. Home ist als **modulare Seite** konfiguriert, die eine Kollektion von Unterseiten enthalten würde und aus den modularen Unterseiten `_header`, `_features` und `_body` aufgebaut wäre.

Der textliche Namensteil jedes Ordners wird standardmäßig auf den _Slug_ gesetzt, den das System als Teil der URL verwendet. Wenn Sie z.B. einen Ordner wie `/user/pages/02.blog` haben, würde der Slug für diese Seite standardmäßig auf `blog` stehen, und die vollständige URL wäre `http://yoursite.com/blog`. Eine Seite mit einem Blog-Eintrag, die sich in `/user/pages/02.blog/blog-item-5` befindet, wäre über `http://yoursite.com/blog/blog-item-5` zugänglich.

Wenn keine Nummer als Präfix des Ordnernamens angegeben wird, gilt die Seite als **unsichtbar** und wird in der Navigation nicht angezeigt. Ein Beispiel hierfür wäre die `error` Seite in der obigen Ordnerstruktur.

!! Das lässt sich übrigens auf der Seite selbst überschreiben, indem der [Parameter visible](/content/headers#visible) in den Headerzeilen gesetzt wird.

## Sortierung

Wenn Sie mit Kollektionen arbeiten, stehen Ihnen mehrere Optionen zur Verfügung, um die Reihenfolge der Ordner zu steuern. Die wichtigste Option wird mit `content.order.by` in den Einstellungen der Seitenkonfiguration festgelegt. Die Optionen sind:

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **default**  | Die Reihenfolge richtet sich nach dem Dateisystem, d.h. `01.home` vor `02.advark` |
| **title**    | Die Reihenfolge richtet sich nach dem Titel, wie er auf der jeweiligen Seite definiert ist. |
| **basename** | Die Sortierung beruht auf dem Alphabet ohne den numerischen Präfix. |
| **date**     | Die Reihenfolge basiert auf dem Datum, wie es auf jeder Seite zu finden ist. |
| **modified** | Die Reihenfolge basiert auf dem Zeitstempel der modifizierten Seite |
| **folder**   | Die Reihenfolge basiert auf dem Ordnernamen mit einem beliebigen numerischen Präfix, d.h. `01.`, entfernt |
| **header.x** | Die Reihenfolge basiert auf einem beliebigen Kopfzeilenfeld der Seite,z.B. `header.taxonomy.year`.  Auch eine Voreinstellung kann über eine Pipe hinzugefügt werden. z.B. `header.taxonomy.year|2015` |
| **manual**   | Die Reihenfolge basiert auf der Variablen `order_manual` |
| **random**   | Die Reihenfolge wird nach dem Zufallsprinzip ermittelt. |
[/div]

Sie können eine manuelle Reihenfolge ausdrücklich definieren, indem Sie eine Liste von Optionen für die Konfigurationseinstellung `content.order.custom` zur Verfügung stellen. Das funktioniert in Verbindung mit `content.order.by`, weil sie zuerst versucht, die Seiten manuell zu sortieren, aber alle Seiten, die nicht in der manuellen Reihenfolge angegeben sind, fallen durch und werden nach der angegebenen Reihenfolge angeordnet.

!! Das **Standardverhalten** für die Sortierung der Ordner und die Richtung, in der die Reihenfolge eingehalten wird, können Sie durch Festlegen der Optionen `pages.order.dir` und `pages.order.by` in der [Grav-System-Konfigurationsdatei](/basics/grav-configuration) überschreiben.

## Seitendatei

Innerhalb des Page-Ordners erstellen wir die eigentliche Seitendatei. Der Dateiname sollte mit `.md` enden, um zu signalisieren, dass es sich um eine Markdown-formatierte Datei handelt. Technisch gesehen handelt es sich um Markdown mit YAML FrontMatter, was beeindruckend klingt, aber in Wirklichkeit gar keine große Sache ist. Auf die Details der Dateistruktur werden wir in Kürze eingehen.

Wichtig zu verstehen ist, dass der Name der Datei direkt auf den Namen der Template-Datei des Themes verweist, die zum Rendern verwendet wird. Der Standardname für die Haupt-Template-Datei ist **default**, so dass die Datei `default.md` heißen müsste.

Sie können Ihre Datei natürlich nach Belieben benennen, z.B. `Dokument.md`, was Grav dazu veranlassen würde, nach einer passenden Template-Datei im Theme zu suchen, wie z.B. dem Twig-Template **document.html.twig**.

!! Dieses Verhalten kann innerhalb der Seiten-Datei übersteuert werden, indem der [Template-Parameter](/content/headers#template) in den Kopfzeilen gesetzt wird.

Eine Seiten-Datei könnte beispielsweise wie folgt aussehen:

[div class="no-margin-bottom"]
[prism classes="language-yaml line-numbers"]
---
title: Page Title
taxonomy:
    category: blog
---
[/prism]
[/div]
[div class="no-margin-top"]
[prism classes="language-markdown line-numbers" ln-start="6"]
# Page Title

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque porttitor eu
felis sed ornare. Sed a mauris venenatis, pulvinar velit vel, dictum enim. Phasellus
ac rutrum velit. **Nunc lorem** purus, hendrerit sit amet augue aliquet, iaculis
ultricies nisl. Suspendisse tincidunt euismod risus, _quis feugiat_ arcu tincidunt
eget. Nulla eros mi, commodo vel ipsum vel, aliquet congue odio. Class aptent taciti
sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Pellentesque
velit orci, laoreet at adipiscing eu, interdum quis nibh. Nunc a accumsan purus.
[/prism]
[/div]

Die Einstellungen zwischen den beiden `---` Markierungslinien (3-facher Bindestrich) werden als YAML-FrontMatter bezeichnet und umfassen grundlegende YAML-Einstellungen für die Seite.

In diesem Beispiel legen wir den Titel ausdrücklich fest, ebenso wie die Taxonomie auf **blog**, damit wir sie später filtern können. Der Inhalt nach dem zweiten `---` ist der eigentliche Inhalt, der auf Ihrer Website kompiliert und als HTML gerendert wird. Dieser ist in [Markdown](/content/markdown) geschrieben, das in einem späteren Kapitel ausführlich besprochen wird. Sie müssen nur wissen, dass die Marker `#`, `**`, und `_` für die **Überschrift 1**, **fett** und **kursiv** stehen.

!! Vergewissern Sie sich, dass Sie Ihre `.md`-Dateien im `UTF-8` Zeichenkode speichern. Dadurch wird gesichert, dass sie mit sprachspezifischen Sonderzeichen arbeiten können.

### Länge und Trennzeichen der Kurzfassung

Es gibt eine Einstellung in der Datei `site.yaml`, mit der Sie eine Standardgröße (in Zeichen) der Zusammenfassung definieren können, die über `page.summary()` verwendet werden kann, um eine Zusammenfassung oder Exposé der Seite anzuzeigen. Das ist besonders für Blogs nützlich, bei denen Sie eine Übersicht haben möchten, die nur zusammengefasste Informationen und nicht den gesamten Seiteninhalt enthält.

Standardmäßig beträgt dieser Wert `300` Zeichen. Sie können diesen Wert in Ihrer `user/config/site.yaml`-Datei ändern, aber ein noch besserer Weg ist die Verwendung des manuellen **Trennzeichens**, auch bekannt als **summary delimiter** (Begrenzung der Zusammenfassung): `===`.

Sie müssen darauf achten, dass Sie dieses Trennzeichen in Ihrem Inhalt mit Leerzeilen **davor** und **dahinter** einfügen. Zum Beispiel:

[prism classes="language-markdown line-numbers"]
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat.

===

Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
[/prism]

Dabei wird der Text oberhalb des Trennzeichens verwendet, wenn er durch `page.summary()` referenziert wird und der gesamte Seiteninhalt, wenn er durch `page.content()` referenziert wird.

!! Bei der Verwendung von `page.summary()` wird die Einstellung für die Länge der Zusammenfassung verwendet, wenn das Trennzeichen nicht im Seiteninhalt gefunden wird.

### Auffinden anderer Seiten

Grav hat eine nützliche Funktion, die es Ihnen ermöglicht, eine andere Seite zu finden und dort Aktionen durchzuführen. Dies kann mit der `find()`-Methode erreicht werden, die einfach die **Route** benutzt und ein neues Page-Objekt zurückgibt.

Dadurch können Sie eine große Palette von Funktionen von jeder Seite Ihrer Grav-Website aus aufrufen. Beispielsweise könnten Sie eine Liste aller aktuellen Projekte auf einer bestimmten Projektseite anzeigen lassen wollen:

{% verbatim %}
[prism classes="language-twig line-numbers"]
# All Projects
<ul>
{% for p in pages.find('/projects').children if p != page %}
<li><a href="{{ p.url|e }}">{{ p.title|e }}</a></li>
{% endfor %}
</ul>
[/prism]
{% endverbatim %}

Im nächsten Abschnitt werden wir weiter auf die Besonderheiten einer Seite im Detail eingehen.

### contentMeta

Das Verweisen auf Seiten und Inhalte ist unkompliziert, aber was ist mit dem Inhalt, der nicht zusammen mit dem Rest der Seite auf dem Frontend gerendert wird?

Wenn Grav den Seiteninhalt liest, speichert es diesen Inhalt im Cache. Auf diese Weise muss beim nächsten Rendern der Seite nicht der gesamte Inhalt aus der `.md`-Datei gelesen werden. Im Allgemeinen wird der gesamte Inhalt für das Frontend gerendert. Es gibt jedoch Fälle, in denen es sinnvoll ist, einige zusätzliche Daten neben der Seite im Cache zu speichern.

Hier kommt `contentMeta()` ins Spiel. Wir verwenden ContentMeta in unserem [Shortcode-Plugin](https://github.com/getgrav/grav-plugin-shortcode-core), um mit Hilfe von Shortcodes [Teile von anderen Seiten abzurufen](https://github.com/getgrav/grav-plugin-shortcode-core#sections-from-other-pages). Zum Beispiel:

{% verbatim %}
[prism classes="language-twig line-numbers"]
<div id="author">{{ page.find('/my/custom/page').contentMeta.shortcodeMeta.shortcode.section.author|e }}</div>
[/prism]
{% endverbatim %}

Wir haben diese Funktion in Shortcode Core verwendet, um CSS- und JS-Assets zu speichern, die für den Shortcode auf der Seite erforderlich sind. Diese Funktion kann auch verwendet werden, um nahezu jede Datenstruktur zu speichern, die Sie benötigen.
