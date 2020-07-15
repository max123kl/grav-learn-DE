---
title: Theme-Variablen
page-toc:
  active: true
taxonomy:
    category: docs
---

Wenn Sie ein Theme entwerfen, haben Sie mit Grav, über Ihre Twig-Templates, Zugriff auf alle Arten von Objekten und Variablen. Das Twig-Templating-Framework bietet leistungsstarke Funktionen zum Auslesen und Manipulieren dieser Objekte und Variablen. Dies wird in [dessen eigener Dokumentation](http://twig.sensiolabs.org/doc/templates.html) vollständig beschrieben und in unserer eigenen Dokumentation [kurz und knapp zusammengefasst](../twig-primer).

!!!! In Twig können Sie auch Methoden aufrufen, die keine Parameter benötigen, indem Sie einfach den Methodennamen aufrufen und die Klammern `()` weglassen.  Wenn Sie Parameter übergeben müssen, müssen Sie diese auch hinter dem Methodennamen angeben.  `page.content` ist gleichbedeutend mit `page.content()`

## Core-Objekte

Es gibt mehrere **Core-Objekte**, die in einem Twig-Template verfügbar sind, und jedes Objekt verfügt über eine Reihe von **Variablen** und **Funktionen**.

### Die Variable: base_dir

Die Variable `{{ base_dir }}` liefert das Basis-Dateiverzeichnis der Grav-Installation zurück.

### Die Variable: base_url

Die Variable `{{ base_url }}` gibt die Basis-URL an die Grav-Site zurück. Ob diese die vollständige URL anzeigt oder nicht, hängt von der Option `absolute_urls` in der [Systemkonfiguration](../../basics/grav-configuration#system-configuration) ab.

### Die Variable: base_url_relative

Die Variable `{{ base_url_relative }}` gibt die Basis-URL ohne die Host-Informationen an die Grav-Site zurück.

### Die Variable: base_url_absolute

Die Variable `{{ base_url_absolute }}` gibt die Basis-URL, einschließlich der Host-Informationen, an die Grav-Site zurück.

### Die Variable: base_url_simple

Die Variable `{{ base_url_simple }}` gibt die Basis-URL ohne den Sprachencode an die Grav-Site zurück.

### Die Variable: home_url

Die Variable `{{ home_url }}` ist besonders geeignet, um einen Link zurück auf die Startseite Ihrer Website zu setzen. Sie ist ähnlich wie `base_url`, berücksichtigt aber den Fall, dass diese leer ist.

### Die Variable: html_lang

Die Variable gibt, falls vorhanden, die aktuell aktive Sprache zurück, andernfalls verwendet sie die konfigurierte Option `site.default_lang` und fällt danach wieder auf `en` zurück.

### Die Variable: theme_dir

Die Variable `{{ theme_dir }}` gibt den Dateiordner für das aktuelle Theme zurück.

### Die Variable: theme_url

Die Variable `{{ theme_url }}` gibt die relative URL für das aktuelle Theme zurück.

### Die Variable: html_lang

Die Variable `{{ html_lang }}` gibt die aktive Sprache zurück.

### Die Variable: language_codes

Die Variable `{{ language_codes }}` returns list of available languages of the site.


!! Wenn Sie auf Assets wie Bilder oder JavaScript- und CSS-Dateien verlinken, empfiehlt es sich, die Funktion `url()` in Kombination mit dem Stream `theme://` zu verwenden, wie auf der Seite [Twig Filter und Funktionen](/themes/twig-filters-functions#url) beschrieben. Für JavaScript und CSS ist der [Asset Manager](/themes/asset-manager) noch einfacher zu bedienen, aber in einigen Fällen, wie beim dynamischen oder bedingten Laden von Assets, funktioniert er nicht.

### Das Objekt config

Auf jede in den YAML-Dateien unter `/user/config` gesetzte Grav-Konfigurationseinstellung können Sie über das Objekt `config` zugreifen. Zum Beispiel:

[prism classes="language-twig"]
{{ config.system.pages.theme }}{# returns the currently configured theme #}
[/prism]

### Das Objekt site

Ein Alias für das Objekt `config.site`. Es ist die in der Datei `site.yaml` festgelegte Konfiguration.

### Das Objekt system

Ein Alias für das Objekt `config.system`. Es ist die in der Datei `system.yaml` enthaltene Konfiguration.

### Das Objekt theme

Ein Alias für das Objekt `config.theme`. Es steht für die Konfiguration für das aktuell aktive Theme. Die Einstellungen des Plugins sind über `config.plugins` zugänglich.

### Das Objekt page

Aufgrund der Struktur von Grav, die im Ordner `pages/` definiert ist, wird jede Seite durch ein **Seiten-Objekt** abgebildet.

Das **Seiten-Objekt** ist wahrscheinlich _das_ wichtigste Objekt, mit dem Sie arbeiten werden, da es alle Informationen über die aktuelle Seite enthält, auf der Sie sich gerade befinden.

!! Die gesamte Liste der Page-Objektmethoden ist auf der [API-Website](https://learn.getgrav.org/api#class-gravcommonpagepage) zu finden. Hier sehen Sie eine Liste der Methoden, die Ihnen äußerst nützlich sein werden.

##### summary([size])

Dadurch wird eine beschnittene oder verkürzte Version Ihres Inhalts angezeigt.  Sie können den optionalen Parameter `size` angeben, um die maximale Länge der Zusammenfassung in Zeichen zu bestimmen.  Wenn keine Größe angegeben wird, kann der Wert alternativ über die site-weite Variable `summary.size` aus Ihrer `site.yaml`-Konfiguration abgerufen werden.

[prism classes="language-twig"]
{{ page.summary }}
[/prism]

or

[prism classes="language-twig"]
{{ page.summary(50) }}
[/prism]

Eine dritte Möglichkeit ist die Verwendung eines manuellen Trennzeichens `===` in Ihrem Dokument.  Alles, was vor dem Trennzeichen steht, wird für die Zusammenfassung verwendet.

##### content()

Dadurch wird der gesamte HTML-Inhalt Ihrer Seite angezeigt.

[prism classes="language-twig"]
{{ page.content }}
[/prism]

##### header()

Hiermit werden die Kopfzeilen der Seite zurückgegeben, wie sie im YAML-Frontmatter der Seite definiert sind. Zum Beispiel eine Seite mit den folgenden Kopfzeilen:

[prism classes="language-yaml line-numbers"]
title: My Page
author: Joe Bloggs
[/prism]

könnte so verwendet werden:

[prism classes="language-twig"]
The author of this page is: {{ page.header.author }}
[/prism]

##### media()

Damit wird ein Array zurückgegeben, das alle mit einer Seite verbundenen Medien enthält. Dazu gehören **Bilder**, **Videos** und andere **Dateien**. Sie können auf Medien-Methoden zugreifen, wie in der [Medien-Dokumentation](../../content/media) zum Inhalt beschrieben. Da es sich um ein Array handelt, können Twig-Filter und -Funktionen eingesetzt werden. Hinweis: `.svg` wird als Datei und nicht als Bild behandelt, da sie nicht mit Twig-Bildfiltern manipuliert werden kann.

Eine bestimmte Datei oder ein bestimmtes Bild aufrufen:

[prism classes="language-twig"]
{% set my_pdf = page.media['myfile.pdf'] %}
[/prism]

Das erste Bild aufrufen:

[prism classes="language-twig"]
{% set first_image = page.media.images|first %}
[/prism]

Alle Bilder in eine Schleife legen und den HTML-Tag ausgeben, um sie anzuzeigen:

[prism classes="language-twig"]
{% for image in page.media.images %}
   {{ image.html }}
{% endfor %}
[/prism]

##### title()

Dies liefert den Titel der Seite, wie er in der Variable `title` der YAML-Kopfzeilen für die Seite gesetzt ist.

[prism classes="language-yaml"]
title: My Page
[/prism]

##### menu()

Dies liefert den Wert der Variablen `menu` in den YAML-Kopfzeilen der Seite.  Wenn nichts angegeben ist, wird standardmäßig `title` zurückgegeben.

[prism classes="language-yaml line-numbers"]
title: My Page
menu: my-page
[/prism]

##### visible()

Hier wird geprüft, ob die Seite sichtbar ist oder nicht.  Standardmäßig sind Seiten mit einem numerischen Wert gefolgt von einem Punkt sichtbar (`01.somefolder1`), während Seiten ohne diesen Wert (`Unterordner2`) als unsichtbar angesehen werden. Diese Einstellung kann in den Headerzeilen der Seite überschrieben werden:

[prism classes="language-yaml line-numbers"]
title: My Page
visible: true
[/prism]

##### routable()

Dadurch wird angezeigt, ob eine Seite mit Grav routingfähig ist oder nicht.  Das heißt, ob Sie mit Ihrem Browser auf die Seite verweisen und den Inhalt zurückerhalten können. Nicht routbare Seiten können in Templates, Plugins usw. verwendet werden, sind aber nicht direkt erreichbar. Dies kann in den Kopfzeilen der Seite eingestellt werden:

[prism classes="language-yaml line-numbers"]
title: My Page
routable: true
[/prism]

##### slug()

Dies gibt den Namen zurück, wie er in der URL für diese Seite angezeigt wird, z.B. `my-blog-post`.

##### url([include_host = false])

Dadurch wird z.B. die URL zur Seite zurückgegeben:

[prism classes="language-twig"]
{{ page.url }} {# could return /my-section/my-category/my-blog-post #}
[/prism]

oder

[prism classes="language-twig"]
{{ page.url(true) }} {# could return http://mysite.com/my-section/my-category/my-blog-post #}
[/prism]

##### permalink()

Dies gibt die URL mit Host-Informationen zurück. Diese Funktion ist besonders dann interessant, wenn ein kurzer Link benötigt wird, auf den von überall aus zugegriffen werden kann.

##### canonical()

Damit wird die URL der „gewählten“ Variante oder der Link zu einer bestimmten Seite zurückgegeben. Dieser Wert wird standardmäßig auf die reguläre URL gesetzt, es sei denn, die Seite hat die Page-Header-Option `canonical:` überschrieben.

##### route()

Hiermit wird das interne Routing für eine Seite angegeben. In erster Linie wird diese Funktion für das interne Routing und das Ausliefern von Seiten verwendet.

##### home()

Dies gibt `true` oder `false` zurück, je nachdem, ob diese Seite als **Home**-Page konfiguriert ist oder nicht.  Die Einstellung ist in der Datei `system.yaml` zu finden.

##### root()

Das Ergebnis ist entweder `true` oder `false`, je nachdem, ob diese Seite die Root-Seite der Baumhierarchie ist oder nicht.
Wird so verwendet: {{ page.parent.root() }}

##### active()

Es wird `true` oder `false` angezeigt, je nachdem, ob diese Seite die Seite ist, auf die Ihr Browser gerade zugreift oder nicht. Dies ist besonders nützlich bei der Navigation, um festzustellen, ob die Seite, auf der Sie sich gerade befinden, die aktive Seite ist.

##### modular()

Dadurch wird `true` oder `false` zurückgegeben, je nachdem, ob diese Seite modular aufgebaut ist oder nicht.

##### activeChild()

Hiermit erhalten Sie die Information, ob die URL aus diesen URIs auch die URL der aktiven Seite enthält oder nicht. Mit anderen Worten, ist die URL dieser Seite in der aktuellen URL enthalten? Auch dies ist beim Aufbau Ihrer Navigation nützlich und Sie möchten wissen, ob die Seite, über die Sie die Iteration durchführen, die übergeordnete Seite einer aktiven untergeordneten Seite ist.

##### find(url)

Dies gibt ein Seiten-Objekt zurück, wie es durch eine Route-URL spezifiziert ist.

[prism classes="language-twig"]
{% include 'modular/author-detail.html.twig' with {'page': page.find('/authors/billy-bloggs')} %}
[/prism]

##### collection()

Dadurch wird die Kollektion aus Seiten zu diesem Kontext zurückgegeben, wie sie durch die [Kopfzeilen der Kollektionsseiten](../../content/collections) bestimmt wird.

[prism classes="language-twig line-numbers"]
{% for child in page.collection %}
    {% include 'partials/blog_item.html.twig' with {'page':child, 'truncate':true} %}
{% endfor %}
[/prism]

##### currentPosition()

Gibt den Index der aktuellen Seite im Verhältnis zu ihren Schwester-Seiten zurück.

##### isFirst()

Daraus ergibt sich `true` oder `false`, je nachdem, ob diese Seite die erste ihrer Geschwister ist.

##### isLast()

Daraus ergibt sich `true` oder `false`, je nachdem, ob diese Seite die letzte ihrer Geschwister ist.

##### nextSibling()

Hiermit wird die nächste Seite aus dem Array der Schwester-Seiten, basierend auf der aktuellen Position, zurückgegeben.

##### prevSibling()

Hiermit wird die vorherige Seite aus dem Array der Schwester-Seiten, basierend auf der aktuellen Position, zurückgegeben.

!! nextSibling() und prevSibling() ordnen Seiten in einer stack-ähnlichen Struktur an. Am besten funktioniert das in einer Blog-Situation, in der der erste Blog-Eintrag nextSibling null ist, während prevSibling der vorherige Blog-Eintrag ist. Wenn Sie durch diese Reihenfolge verwirrt werden, schlagen wir vor, dass Sie page.adjacentSibling(-1) verwenden, um auf die nächste Seite zu verweisen, anstatt page.nextSibling(), um die mögliche Verwirrung durch die Terminologie zu verringern. Sie können auch eine Konstante im Theme definieren und diese zur besseren Lesbarkeit verwenden, wie z.B. page.adjacentSibling(NEXT_PAGE)

##### children()

Dies gibt ein Array von untergeordneten Seiten für die Seite zurück, wie sie in der Inhaltsstruktur der Seite definiert ist.

##### orderBy()

Dies liefert den Auftragstyp für jede sortierte Unterseite der Seite zurück. Die Werte umfassen in der Regel: `default`, `title`, `date` und `folder`. Dieser Wert wird typischerweise in den Kopfzeilen der Seite konfiguriert.

##### orderDir()

Damit wird die Reihenfolge für alle sortierten Nachkommen der Seite zurückgegeben.  Die Werte können entweder `asc` für aufsteigend oder `desc` für absteigend sein. Dieser Wert wird typischerweise in den Kopfzeilen der Seite konfiguriert.

##### orderManual()

Dadurch wird ein Array zur manuellen Seitenanordnung für alle Nachfolger der Seite zurückgegeben.  Dieser Wert wird typischerweise in den Kopfzeilen der Seite konfiguriert.

##### maxCount()

Dies gibt die maximale Anzahl von Child-Seiten zurück, die maximal ausgegeben werden dürfen.  Dieser Wert wird typischerweise in den Kopfzeilen der Seite konfiguriert.

##### children.count()

Gibt die Anzahl der untergeordneten Seiten der Seite zurück.

##### children.current()

Gibt das aktuelle untergeordnete Element zurück.  Kann während der Iteration der Unterobjekte verwendet werden.

##### children.next()

Dadurch wird das jeweils nächste Mitglied in der Reihe der Unterobjekte zurückgegeben.

##### children.prev()

Gibt das vorherige Mitglied im Array der Unterobjekte zurück.

##### children.nth(position)

Dies gibt das Kind-Element zurück, das durch `position` identifiziert wird, eine ganze Zahl von `0` bis `children.count() - 1` im Array der Kinder ist.

##### children.sort(orderBy, orderDir)

Ordnet die Kind-Elemente basierend auf **orderBy** (`default`, `title`, `date` und `folder`) und **orderDir** (`asc` oder `desc`)

##### parent()

Gibt das übergeordnete Seiten-Objekt für diese Seite zurück. Dies ist sehr nützlich, wenn Sie in der ineinander verschachtelten Baumstruktur mehrerer Seiten zurück navigieren müssen.



##### isPage()

Gibt `true` oder `false` zurück, je nachdem, ob dieser Seite eine konkrete `.md`-Datei zugeordnet ist und nicht nur ein Ordner für das Routing.

##### isDir()

Gibt `true` oder `false` zurück, je nachdem, ob dieser Seite nur ein Ordner für das Routing zugeordnet ist.

##### id()

Gibt eine eindeutige Identifikationsnummer für die Seite zurück.

##### modified()

Dies gibt einen Zeitstempel zurück, der angibt, wann die Seite zuletzt geändert wurde.

##### date()

Zeigt den Datums-Zeitstempel für die Seite an.  Normalerweise wird dieser in den Kopfzeilen gesetzt, um das Datum einer Seite oder eines Beitrags wiederzugeben.  Wenn nicht explizit ein Wert definiert wird, gilt der in der Datei geänderte Zeitstempel.

##### template()

Gibt den Namen des Seiten-Templates ohne die Erweiterung `.md` zurück. Zum Beispiel `default`

##### filePath()

Gibt den vollständigen Dateipfad der Seite zurück. Zum Beispiel `/Users/yourname/sites/grav/user/pages/01.home/default.md`

##### filePathClean()

Diese Funktion gibt den relativen Pfad ab der Grav-Root zurück.  Zum Beispiel `user/pages/01.home/default.md`

##### path()

Hierdurch wird der vollständige Pfad zu dem Verzeichnis zurückgegeben, das die Seite enthält. Zum Beispiel `/Users/yourname/sites/grav/user/pages/01.home`

##### folder()

Gibt den Namen des Ordners für die Seite zurück. Zum Beispiel `01.home`

##### taxonomy()

Dadurch wird ein Array der mit einer Seite verbundenen Taxonomie ausgegeben.  Das kann durch Iteration bearbeitet werden. Besonders nützlich ist das für die Anzeige von Elementen wie Tags:

[prism classes="language-twig line-numbers"]
{% for tag in page.taxonomy.tag %}
    <a href="search/tag:{{ tag }}">{{ tag }}</a>
{% endfor %}
[/prism]

### Das Objekt pages

!! Die gesamte Liste der Pages-Objekt-Methoden ist auf der [API-Seite](https://learn.getgrav.org/api#class-gravcommonpagepages) zu finden. Hier ist eine Übersicht der Methoden, die Sie besonders hilfreich finden werden.

Das **Pages-Objekt** stellt einen ineinander verschachtelten Baum jedes **Page-Objekts** dar, von dem Grav Kenntnis hat.  Das ist besonders nützlich für die Erstellung einer **Sitemap**, einer **Navigation** oder wenn Sie eine bestimmte **Seite** finden wollen.

##### children method

Gibt die unmittelbar untergeordneten Seiten als ein Array von **Page-Objekten** zurück. Da das pages-Objekt den gesamten Baum repräsentiert, können Sie jede Seite im Ordner Grav pages/ komplett rekursiv aufrufen.

So erhalten Sie die Top-Level-Seiten für ein einfaches Menü:

[prism classes="language-twig line-numbers"]
<ul class="navigation">
    {% for page in pages.children %}
        {% if page.visible %}
            <li><a href="{{ page.url }}">{{ page.menu }}</a></li>
        {% endif %}
    {% endfor %}
</ul>
[/prism]

### Das Objekt media

Es gibt ein neues Objekt, mit dem Sie über PHP-Streams von Twig auf [Medien](../../content/media) zugreifen können, die sich außerhalb der Page-Objekte befinden. Dies funktioniert ähnlich wie die [Bildverlinkung im Inhalt](../../content/image-linking#php-streams), bei der Streams verwendet werden, um auf Bilder zuzugreifen und die Medienbearbeitung, um das Theme zu modifizieren.

`media['user://media/bird.png'].resize(50, 50).rotate(90).html()`

### Das Objekt uri

!! Die gesamte Liste der Uri-Objekt-Methoden ist auf der [API-Seite](https://learn.getgrav.org/api#class-gravcommonuri) zu finden. Hier ist eine Übersicht der Methoden, die Sie besonders hilfreich finden werden.

Das Uri-Objekt verfügt über mehrere Methoden, um auf Teile des aktuellen URI zuzugreifen. Für die vollständige URL `http://mysite.com/grav/section/category/page.json/param1:foo/param2:bar/?query1=baz&query2=qux`:

##### path()

Gibt den Pfad-Anteil der URL zurück: (z.B. `uri.path` = `/section/category/page`)

##### paths()

Das gibt das Array der Pfad-Elemente zurück: (z.B. `uri.paths` = `[section, category, page]`)

##### route([absolute = false][, domain = false])

Dies gibt die Route entweder als absolute oder relative URL zurück. (z.B. `uri.route(true)` = `http://mysite.com/grav/section/category/page` oder `uri.route()` = `/section/category/page`)

##### params()

Gibt den Parameter-Teil der URL zurück: (z.B. `uri.params` = `/param1:foo/param2:bar`)

##### param(id)

Gibt den Wert eines bestimmten Parameters zurück.  (z.B. `uri.param('param1')` = `foo`)

##### query()

Gibt den Query-Teil der URL zurück: (z.B. `uri.query` = `query1=bar&query2=qux`)

##### query(id)

Sie können auch gezielt bestimmte Query-Elemente abrufen: (z.B. `uri.query('query1')` = `bar`)

##### url([include_host = true])

Gibt die vollständige URL mit oder ohne Angabe des Hosts zurück. (z.B. `uri.url(false)` = `grav/section/category/page/param:foo?query=bar`)

##### extension()

Gibt die Dateierweiterung zurück, oder liefert `html`, falls nichts übergeben wurde: (z.B. `uri.extension` = `json`)

##### host()

Gibt den Host-Anteil der URL zurück. (z.B. `uri.host` = `mysite.com`)

##### base()

Gibt den Basisteil der URL zurück. (z.B. `uri.base` = `http://mysite.com`)

##### rootUrl([include_host = false])

Gibt die Root-URL an die Grav-Instanz zurück. (z.B. `uri.rootUrl()` = `http://mysite.com/grav`)

##### referrer()

Damit werden die Referrer-Informationen für diese Seite angezeigt.

### Das Objekt header

Das Header-Objekt ist ein Alias für `page.header()` auf der Originalseite. Es bietet Ihnen eine bequeme Möglichkeit, auf die Header der Originalseiten zuzugreifen, wenn Sie in einer Schleife durch andere `page`-Objekte von untergeordneten Seiten oder Kollektionen blättern.

### Das Objekt content

Das Inhalts-Objekt ist ein Alias für `page.content()` der Originalseite.

### Das Objekt taxonomy

Das ist das globale Taxonomie-Objekt, das alle Taxonomie-Information der Website enthält.

### Das Objekt browser

!! Die gesamte Liste der Browser-Objekt-Methoden ist auf der [API-Seite](https://learn.getgrav.org/api#class-grav-common-browser) verfügbar. Hier ist eine Übersicht der Methoden, die Sie besonders hilfreich finden werden.

Grav verfügt über integrierte Funktionen mit denen die Plattform des Benutzers, sein Browser und die Version programmatisch bestimmt werden können.

[prism classes="language-twig"]
{{ browser.platform}}   # macintosh
{{ browser.browser}}    # chrome
{{ browser.version}}    # 41
[/prism]

### Das Objekt user

Sie können indirekt über das Grav-Objekt auf das aktuell angemeldete Benutzer-Objekt zugreifen.  Dies erlaubt Ihnen den Zugriff auf Daten wie `username`, `fullname`, `title`, und `email`:

[prism classes="language-twig"]
{{ grav.user.username }}  # admin
{{ grav.user.fullname }}  # Billy Bloggs
{{ grav.user.title }}     # Administrator
{{ grav.user.email }}     # billy@bloggs.com
[/prism]

## Eigene Variablen hinzufügen

Sie können auf einfache Weise benutzerdefinierte Variablen auf unterschiedliche Weise hinzufügen.  Wenn es sich um eine site-weite Variable handelt, können Sie die Variable in Ihre Datei `user/config/site.yaml` einfügen und darüber darauf zugreifen:

[prism classes="language-twig"]
{{ site.my_variable }}
[/prism]

Wird die Variable nur für eine bestimmte Seite benötigt, können Sie die Variable alternativ auch in den YAML-Frontmatter Ihrer Seite einfügen und über das Objekt `page.header` darauf zugreifen.  Zum Beispiel:

[prism classes="language-twig"]
title: My Page
author: Joe Bloggs
[/prism]

könnte verwendet werden als:

[prism classes="language-twig"]
The author of this page is: {{ page.header.author }}
[/prism]

## Eigene Objekte hinzufügen

Eine verbesserte Möglichkeit, benutzerdefinierte Objekte hinzuzufügen, ist das Hinzufügen von Objekten zum Twig-Objekt mit Hilfe eines Plugins. Das ist ein fortgeschrittenes Thema und wird im [Kapitel Plugins](../../plugins/event-hooks) ausführlicher behandelt.
