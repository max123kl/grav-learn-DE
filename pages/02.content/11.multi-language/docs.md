---
title: Mehrspachige Sites
page-toc:
  active: true
taxonomy:
    category: docs
---

Die mehrsprachige Unterstützung in Grav ist das direkte Ergebnis einer großartigen [Diskussion in der Community](https://github.com/getgrav/grav/issues/170) zu diesem Thema. Wir werden diese nun zusammenfassen und anhand von Beispielen erläutern, wie Sie Ihre Grav-Site mit mehrsprachigen Inhalten einrichten können.

## Eine einzelne Sprache ausser Englisch

Wenn Sie nur eine Sprache verwenden, aktivieren Sie Übersetzungen und fügen Sie Ihren Sprachcode in der Datei `user/config/system.yaml` hinzu:

[prism classes="language-yaml line-numbers"]
languages:
  supported:
    - fr
[/prism]

oder in der Systemkonfiguration im Admin-Plugin:

![Admin Translations Settings](translations-settings.png)

Dadurch wird in Grav die korrekte Verwendung der Sprachstrings im Frontend ermöglicht.
Wenn das Theme dafür geeignet ist, wird auch Ihr Sprachcode dem HTML-Tag hinzugefügt.

## Basiswissen über Mehrsprachigkeit

Sie sollten bereits wissen, wie Grav Markdown-Dateien in Ordnern für die Definition des strukturellen Aufbaus sowie für die Einstellung wichtiger Seitenoptionen und Inhalte einsetzt. Daher werden wir hier nicht mehr näher auf diese Mechanismen eingehen. Sie sollten sich jedoch darüber im Klaren sein, dass Grav standardmäßig nach einer **einzigen** `.md`-Datei in einem Ordner sucht, um diese Seite zu präsentieren.
Falls Sie unsicher sind, ob Sie dieses Prinzip ausreichend verstanden haben, lesen SIe bitte noch einmal im Abschnitt [Erste Schritte]((../../basics/basic-tutorial) nach, bevor Sie hier fortfahren.
Ist die Mehrsprachen-Unterstützung aktiviert, sucht Grav nach der entsprechenden sprachbasierten Datei, zum Beispiel `default.de.md` oder `default.fr.md`.

### Sprach-Konfiguration

Zunächst müssen Sie einige grundlegende Einstellungen für die Sprache in Ihrer Datei `user/config/system.yaml` vornehmen (zur besseren Verständlichkeit mit Kommentaren).

[prism classes="language-yaml line-numbers"]
languages:
  supported: # Supported languages:
    - en # English language
    - fr # French language
  default_lang: en # Set default language to English
  include_default_lang: true # If true, use /en/path instead of /path for default English language.
[version=17]  include_default_lang_file_extension: true # If true, use .en.md file extension instead of .md for default langauge.
  content_fallback:
    en: ['en'] # No fallback for English.
    fr: ['fr', 'en'] #  French falls back to English version of the page.
[/version]
[/prism]

Durch die Einrichtung eines `languages` Blocks mit einer Liste der „unterstützten“ (`supported`) Sprachen haben Sie die Mehrsprachen-Unterstützung innerhalb von Grav wirksam aktiviert.

In diesem Beispiel können Sie sehen, dass zwei unterstützte Sprachen angegeben werden (`en` und `fr`). Dadurch können Sie die Sprachen **Englisch** und **Französisch** bedienen.

Wenn keine Sprache explizit angefordert wird (über die URL oder per Code), verwendet Grav die angegebene Sprachreihenfolge zur Auswahl der richtigen Sprache.  Im vorstehenden Beispiel ist die **Standardsprache** also `en` oder Englisch. Wenn Sie `fr` zuerst angeben hätten, wäre Französisch die Standardsprache.

[version=17]
Standardmäßig fallen alle Sprachen auf die Standardsprache zurück. Wenn Sie dies nicht wünschen, können Sie die Sprach-Fallbacks mit Hilfe von `content_fallback` überschreiben, wobei der Schlüssel die Sprache und der Wert das Array mit den Sprachen ist.
[/version]

!! Sie können natürlich so viele Sprachen zur Auswahl anbieten, wie Sie möchten und sogar Gebietsschema-Codes wie `en-GB`, `en-US` und `fr-FR` verwenden.  Wenn Sie diese Gebietsschema-basierte Benennung verwenden, müssen Sie _alle_ kurzen Sprachcodes durch die Gebietsschema-Versionen ersetzen.

### Seiten in mehreren Sprachen

Normalerweise wird in Grav jede Seite durch eine einzelne Markdown-Datei wiedergegeben, zum Beispiel `default.md`. Wenn Sie die Mehrsprachen-Unterstützung aktivieren, sucht Grav nach der passenden Markdown-Datei mit dem entsprechenden Dateinamen. Da, im Beispiel, Englisch unsere Standardsprache ist, wird zuerst die Datei `default.en.md` erwartet.

[version=15]
Wenn diese Datei nicht gefunden wird, wird die nächste Sprache getestet und nach `default.fr.md` gesucht. Erst wenn auch diese Datei nicht gefunden wird, greift Grav auf den Grav-Standard zurück und sucht nach `default.md`, um die Informationen der Seite anzuzeigen.
[/version]
[version=16]
Wenn diese Datei nicht gefunden wird, wird die nächste Sprache getestet und nach `default.fr.md` gesucht. Erst wenn auch diese Datei nicht gefunden wird, greift Grav auf den Grav-Standard zurück und sucht nach `default.md`, um die Informationen der Seite anzuzeigen.
[/version]
[version=17]
Wenn diese Datei nicht gefunden wird, greift Grav auf den Grav-Standard zurück und sucht nach `default.md`, um die Informationen der Seite anzuzeigen.

!! Dieses Standardverhalten hat sich in **Grav 1.7** geändert. In der Vergangenheit zeigte Grav nicht existierende englische Seiten in Französisch an, jetzt fallen alle Sprachen nur noch auf die Standardsprache zurück, wenn nicht anderes in `content_fallback` angegeben ist. Wenn die Seite also in keiner der Fallback-Sprachen gefunden werden kann, wird stattdessen die Fehlerseite **404 Error Page** angezeigt.
[/version]

Wenn wir die einfachsten Grav-Sites mit einer einzigen `01.home/default.md` Datei haben sollten, dann könnten wir damit beginnen die `default.md` in `default.en.md` umbenennen. Der Inhalt könnte dann so aussehen:

[prism classes="language-markdown line-numbers"]
---
title: Homepage
---

This is my Grav-powered homepage!
[/prism]

Sie könnten anschließend ,im gleichen Ordner `01.home/`, eine neue Datei mit dem Namen `default.fr.md` erstellen, die den folgenden Inhalt enthält:

[prism classes="language-markdown line-numbers"]
---
title: Page d'accueil
---

Ceci est ma page d'accueil générée par Grav !
[/prism]

Damit haben Sie zwei Seiten für Ihre aktuelle Homepage in verschiedenen Sprachen eingerichtet.

[version=17]
! Wenn Sie eine bestehende Website für die Verwendung mehrerer Sprachen konvertieren, können Sie alternativ `include_default_lang_file_extension: false` setzen, um die einfache Dateierweiterung `.md` weiterhin für Ihre Hauptsprache verwenden zu können. [Mehr Informationen...](/content/multi-language#default-file-extension).
[/version]

### Aktive Sprache über die URL

Da Englisch die Standardsprache ist, würden Sie mit Ihrem Browser, ohne eine Sprache anzugeben, den Inhalt wie in der Datei `default.en.md` beschrieben erhalten. Sie könnten auch explizit Englisch anfordern, indem Sie Ihren Browser auf folgendes richten:

[prism classes="language-text line-numbers"]
http://yoursite.com/en
[/prism]

Um die französische Version aufzurufen, würden Sie folgende Methode verwenden:

[prism classes="language-text line-numbers"]
http://yoursite.com/fr
[/prism]

! Wenn Sie es vorziehen, kein Sprachpräfix für die Standardsprache zu verwenden, setzen Sie `include_default_lang: false`. [Mehr Informationen...](/content/multi-language#default-language-prefix).

### Aktive Sprache über den Browser

Bei den meisten Browsern können Sie konfigurieren, in welchen Sprachen Sie die Inhalte bevorzugt sehen möchten. Grav hat die Fähigkeit, diese Werte für `http_accept_language` zu lesen und sie mit den aktuell unterstützten Sprachen für die Website zu vergleichen. Falls keine bestimmte Sprache erkannt wurde, zeigt Grav Ihnen den Inhalt in der von Ihnen bevorzugten Sprache an.

Damit diese Funktion einwandfrei arbeitet, müssten Sie im Abschnitt `languages:` die Option in Ihrer Datei `user/system.yaml` aktivieren:

[prism classes="language-yaml line-numbers"]
languages:
  http_accept_language: true
[/prism]


### Session-Based Active Language

If you wish to remember the active language independently from the URL, you can activate **session-based** storage of the active language.  To enable this, you must ensure you have `session: enabled: true` in [the system.yaml](../../basics/grav-configuration).  Then you need to enable the language setting:

[prism classes="language-yaml line-numbers"]
languages:
  session_store_active: true
[/prism]

Damit wird die aktive Sprache in der Sitzung gespeichert.

### Gebietsschema auf die aktive Sprache einstellen

Die boolean-Einstellung setzt die PHP-Funktion `setlocale()` auf die der aktiven Sprache. Diese Funktion kontrolliert die Anzeige der regionalen  Währungswerte, Datumsangaben, String-Vergleiche, Zeichenklassen und andere länderspezifische Einstellungen. Die Voreinstellung ist `false`. Dann wird die System-Locale verwendet, wenn Sie diesen Wert auf `true` setzen, wird die Locale mit der gerade aktiven Sprache überschrieben.

[prism classes="language-yaml line-numbers"]
languages:
   override_locale: false
[/prism]

### Vorgabe des Präfixes für die Sprache

In allen URLs wird automatisch der Standard-Sprachencode vorangestellt. Im Beispiel oben werden Englisch und Französisch (`en` und `fr`) unterstützt und die Voreinstellung ist Englisch. Eine Seitenroute könnte so aussehen: `/en/my-page` für Englisch und `/fr/ma-page` für Französisch. Es ist allerdings oft besser, die Standardsprache ohne das Präfix anzugeben, so dass Sie diese Option einfach auf `false` setzen können und die englische Seite wird als `/my-page` erscheinen.

[prism classes="language-yaml line-numbers"]
languages:
    include_default_lang: false
[/prism]

[version=17]
### Standard-Dateierweiterung

Wenn Sie eine bestehende Website für die Verwendung mehrerer Sprachen konvertieren, kann es eine gewaltige Aufgabe sein, alle bestehenden Seiten so zu konvertieren, dass sie die neue Sprachdateierweiterung `.en.md` verwenden (falls Sie Englisch verwenden wollen). Sie sollten in dieser Situation eventuell die Spracherweiterung für Ihre Originalsprache deaktivieren.

[prism classes="language-yaml line-numbers"]
languages:
    include_default_lang_file_extension: false
[/prism]
[/version]

### Routing für mehrere Sprachen

Grav verwendet normalerweise die Namen der Ordner, um eine URL-Route für eine bestimmte Seite zu erzeugen.  Auf diese Weise kann die Architektur der Site leicht verstanden und als verschachtelter Satz von Ordnern umgesetzt werden.  Bei einer mehrsprachigen Website kann es jedoch sinnvoll sein, eine URL zu verwenden, die in der jeweiligen Sprache verständlicher ist.

Angenommen, wir haben die folgende Ordnerstruktur:

[prism classes="language-yaml line-numbers"]
- 01.animals
  - 01.mammals
    - 01.bats
    - 02.bears
    - 03.foxes
    - 04.cats
  - 02.reptiles
  - 03.birds
  - 04.insets
  - 05.aquatic
[/prism]

This would produce URLs such as `http://yoursite.com/animals/mammals/bears`.  This is great for an English site, but if you wished to have a French version you would prefer these to be translated appropriately. The easiest way to achieve this is to add a custom [slug](../headers#slug) for each of the `fr.md` page files.  for example, the mammal page might look something like:

[prism classes="language-markdown line-numbers"]
---
title: Mammifères
slug: mammiferes
---

Les mammifères (classe des Mammalia) forment un taxon inclus dans les vertébrés, traditionnellement une classe, définie dès la classification de Linné. Ce taxon est considéré comme monophylétique...
[/prism]

Zusammen mit den entsprechenden **Slug-Overrides** in den anderen Dateien sollte dies zu der wesentlich „französischer“ aussehenden URL `http://yoursite.com/animaux/mammiferes/ours` führen!

Eine weitere Möglichkeit besteht darin, die [Routen-Unterstützung auf Seitenebene](../headers#routes) zu nutzen und einen vollständigen Route-Alias für die Seite anzugeben.

### Sprachbasierte Homepage

Falls Sie die Route bzw. den Slug für die Homepage überschreiben, kann Grav die Homepage nicht finden, wie sie durch die Option `home.alias` in Ihrer `system.yaml` definiert ist. Es wird nach `/homepage` suchen und Ihre französische Homepage hat möglicherweise eine Route für `/page-d-accueil`.

Um mehrsprachige Homepages zu unterstützen, hat Grav eine neue Option eingeführt. Anstelle von `home.alias` kann man nun auch einfach `home.aliases` verwenden. Sie könnte in etwa so aussehen:

[prism classes="language-yaml line-numbers"]
home:
  aliases:
    en: /homepage
    fr: /page-d-accueil
[/prism]

Auf diese Weise weiß Grav, wie man auf die Homepage gelangt, je nachdem, welche Sprache, Englisch oder Französisch, aktiv ist.

### Sprachbasierte Twig-Templates

Standardmäßig verwendet Grav den Markdown-Dateinamen, um das zum Rendern zu verwendende Twig-Template zu bestimmen.  Das funktioniert bei mehrsprachigen Dokumenten auf die gleiche Weise.  Beispielsweise würde `default.fr.md` nach einer Twig-Datei namens `default.html.twig` in den entsprechenden Twig-Template-Pfaden des aktuellen Themes und aller Plugins suchen, die Twig-Template-Pfade erfassen.  Bei mehrsprachigen Inhalten fügt Grav auch die aktuelle aktive Sprache in die Pfadstruktur ein.  Das bedeutet, dass Sie, wenn Sie eine sprachspezifische Twig-Datei benötigen, diese einfach in einen Sprachordner auf Root-Ebene legen können.  Wenn Ihr aktuelles Thema beispielsweise eine Vorlage verwendet, die sich unter `templates/default.html.twig` befindet, könnten Sie einen Ordner `templates/fr/` erstellen und Ihre französisch-spezifische Twig-Datei dort ablegen: `templates/fr/default.html.twig`.

Eine weitere Option, die eine manuelle Konfiguration erfordert, besteht darin, die Einstellung `template:` in den Kopfzeilen der Seiten zu überschreiben. Zum Beispiel:

[prism classes="language-yaml line-numbers"]
template: default.fr
[/prism]

Diese Option sucht nach einem Template, das sich unter `templates/default.fr.html.twig` befindet.

Damit haben Sie zwei Möglichkeiten, sprachspezifische Twig-Overrides zur Verfügung zu stellen.

!! Wenn kein sprachspezifisches Twig-Template vorhanden ist, wird das Standard-Twig-Template verwendet.



### Übersetzung über Twig

Der einfachste Weg, diese Übersetzungs-Strings in Ihren Twig-Templates zu verwenden, ist die Verwendung des Twig-Filters `|t`.  Sie können auch die Twig-Funktion `t()` verwenden, aber offen gesagt ist der Filter klarer und macht dasselbe:

[prism classes="language-twig line-numbers"]
<h1 id="site-name">{{ "SITE_NAME"|t|e }}</h1>
<section id="header">
    <h2>{{ "HEADER.MAIN_TEXT"|t|e }}</h2>
    <h3>{{ "HEADER.SUB_TEXT"|t|e }}</h3>
</section>
[/prism]

Mit Hilfe der Twig-Funktion `t()` ist die Lösung ähnlich:

[prism classes="language-twig line-numbers"]
<h1 id="site-name">{{ t("SITE_NAME")|e }}</h1>
<section id="header">
    <h2>{{ t("HEADER.MAIN_TEXT")|e }}</h2>
    <h3>{{ t("HEADER.SUB_TEXT")|e }}</h3>
</section>
[/prism]

Eine weitere neue Twig-Filter/Funktion ermöglicht es Ihnen, aus einem Array heraus zu übersetzen.  Dies ist besonders nützlich, wenn Sie eine Liste von Werten wie z.B. Monate oder Wochentage haben.  Nehmen wir zum Beispiel an, Sie haben diese Übersetzung:

[prism classes="language-yaml line-numbers"]
en:
  GRAV:
    MONTHS_OF_THE_YEAR: [January, February, March, April, May, June, July, August, September, October, November, December]
[/prism]

Sie könnten die passende Übersetzung für den Monat eines Beitrags mit den folgenden Angaben erhalten:

[prism classes="language-twig line-numbers"]
{{ 'GRAV.MONTHS_OF_THE_YEAR'|ta(post.date|date('n') - 1)|e }}
[/prism]

Man kann auch die Twig-Funktion mit `ta()` verwenden.

### Übersetzung mit Variablen

Darüber hinaus können Sie Variablen in Ihren Twig-Übersetzungen verwenden, wobei Sie die [PHP-Syntax sprintf](https://php.net/sprintf) verwenden:

[prism classes="language-yaml line-numbers"]
SIMPLE_TEXT: There are %d monkeys in the %s
[/prism]

Und dann können Sie diese Variablen mit Twig ergänzen:

[prism classes="language-twig line-numbers"]
{{ "SIMPLE_TEXT"|t(12, "London Zoo")|e }}
[/prism]

die zur eigentlichen Übersetzung führen:

[prism classes="language-text line-numbers"]
There are 12 monkeys in the London Zoo
[/prism]

### Komplexe Übersetzungen

Manchmal ist es erforderlich, komplexe Übersetzungen mit Ersetzungen in den einzelnen Sprachen durchzuführen.  Sie können die volle Leistung der `translate()` Methode für Sprachobjekte mit dem `tl` Filter/Funktion ausnutzen.  Zum Beispiel:

[prism classes="language-twig line-numbers"]
{{ ["SIMPLE_TEXT", 12, 'London Zoo']|tl(['fr'])|e }}
[/prism]

Wird den String `SIMPLE_TEXT` übersetzen und die Platzhalter durch `12` bzw. `London Zoo` ersetzen.  Es wird außerdem ein Array mit Sprachübersetzungen übergeben, das in der Reihenfolge „first-find-first-used“ durchlaufen wird.  Das Ergebnis wird in französischer Sprache ausgegeben:


[prism classes="language-text line-numbers"]
Il y a 12 singes dans le Zoo de Londres
[/prism]

### PHP-Übersetzungen

Neben dem Twig-Filter und den Funktionen können Sie den gleichen Ansatz auch in Ihrem Grav-Plugin verwenden:

[prism classes="language-php line-numbers"]
$translation = $this->grav['language']->translate(['HEADER.MAIN_TEXT']);
[/prism]

Sie können auch eine Sprache festlegen:

[prism classes="language-php line-numbers"]
$translation = $this->grav['language']->translate(['HEADER.MAIN_TEXT'], 'fr');
[/prism]

Zum Übersetzen eines bestimmten Elements in einem Array verwenden Sie:

[prism classes="language-php line-numbers"]
$translation = $this->grav['language']->translateArray('GRAV.MONTHS_OF_THE_YEAR', 3);
[/prism]

### Plugin- und Theme-Übersetzungen

In Plugins und Themes können Sie auch Ihre eigenen Übersetzungen bereitstellen. Dazu erstellen Sie eine Datei `languages.yaml` im Stammverzeichnis Ihres Plugins oder Themes (z.B. `/user/plugins/error/languages.yaml` oder `user/themes/antimatter/languages.yaml`), die alle unterstützten Sprachen mit dem vorangestellten Sprach- oder Ländercodes enthalten sollte:

[prism classes="language-yaml line-numbers"]
en:
  PLUGIN_ERROR:
    TITLE: Error Plugin
    DESCRIPTION: The error plugin provides a simple mechanism for handling error pages within Grav.
fr:
  PLUGIN_ERROR:
    TITLE: Plugin d'Erreur
    DESCRIPTION: Le plugin d'erreur fournit un mécanisme simple de manipulation des pages d'erreur au sein de Grav.
[/prism]

! Die Konvention für Plugins ist, PLUGIN_PLUGINNAME.* als Präfix für alle Sprachstrings zu verwenden, um Namenskonflikte zu vermeiden. Es ist weniger wahrscheinlich, dass Themes Konflikte mit Sprachstrings verursachen. Es ist eine gute Wahl, in Themes hinzugefügte Strings mit dem Präfix THEME_THEMENAME.* zu versehen.

### Übersetzung-Overrides

Wenn Sie eine bestimmte Übersetzung übersteuern möchten, legen Sie einfach das modifizierte Schlüssel/Wert-Paar in einer entsprechenden Sprachdatei in Ihrem Ordner `user/languages/` ab. Zum Beispiel könnte eine Datei namens `user/languages/de.yaml` enthalten:

[prism classes="language-yaml line-numbers"]
PLUGIN_ERROR:
  TITLE: My Error Plugin
[/prism]

Dies stellt sicher, dass Sie immer einen Übersetzungs-String überschreiben können, ohne an den Plugins oder Themes selbst herumzupfuschen. Außerdem wird so vermieden, dass eine benutzerdefinierte Übersetzung beim Aktualisieren überschrieben wird.

## Fortgeschrittene Anwendung

### Behandlung von Sprache in der System-Umgebung

Sie können die Vorteile der [Grav-Umgebungskonfiguration](../../advanced/environment-config) nutzen, um Benutzer anhand der URL automatisch zur richtigen Version Ihrer Website zu leiten. Wenn Sie z.B. eine URL wie `http://french.mysite.com` haben, die ein Alias für Ihre Standard-URL `http://www.mysite.com` ist, könnten Sie eine Umgebungs-Konfiguration einrichten:

`/user/french.mysite.com/config/system.yaml`

[prism classes="language-yaml line-numbers"]
languages:
  supported:
    - fr
    - en
[/prism]

Dieses Beispiel verwendet eine **invertierte Sprachreihenfolge**, so dass die Standard-Sprache jetzt `fr` ist und die französische Sprache automatisch angezeigt wird.

### Sprach-Alias-Routen

Da jede Seite ihre eigene benutzerdefinierte Route haben kann, wäre es schwierig, zwischen verschiedenen Sprachversionen derselben Seite zu wechseln.  Es gibt jedoch eine neue Funktion **Page.rawRoute()** im Page-Objekt, die die gleiche einfache Route für jede der verschiedenen Sprachversionen einer einzelnen Seite liefert.  Alles, was Sie tun müssten, wäre, den Sprach-Code voranzustellen, um die richtige Route zu einer bestimmten Sprachversion einer Seite zu erhalten.

Nehmen wir zum Beispiel an, Sie befinden sich auf einer Seite in englischer Sprache mit einer benutzerdefinierten Route:

[prism classes="language-text line-numbers"]
/my-custom-english-page
[/prism]

Die französische Seite hat folgende benutzerdefinierte Route:

[prism classes="language-text line-numbers"]
/ma-page-francaise-personnalisee
[/prism]

Sie könnten die Rohfassung der englischen Seite verwenden und das wäre:

[prism classes="language-text line-numbers"]
/blog/custom/my-page
[/prism]

Dann fügen Sie einfach die gewünschte Sprache hinzu und schon ist das Ihre neue URL:

[prism classes="language-text line-numbers"]
/fr/blog/custom/my-page
[/prism]

Damit wird dieselbe Seite wie `/ma-page-francaise-personnalisee` aufgerufen.

## Unterstützung von Übersetzungen

Grav bietet einen einfachen, aber effektiven Mechanismus zur Erstellung von Übersetzungen in Twig und außerdem über PHP für den Einsatz in Themes und Plugins. Dieser Mechanismus ist standardmäßig aktiviert und verwendet die Sprache `en`, wenn keine Sprachen definiert sind. Um Übersetzungen manuell zu aktivieren oder zu deaktivieren, gibt es eine Einstellung in Ihrer `system.yaml`:

[prism classes="language-yaml line-numbers"]
languages:
  translations: true
[/prism]

Die Übersetzungen verwenden die gleiche Sprachen-Liste, wie sie durch die `languages: supported:` in Ihrer `system.yaml` festgelegt wird.

Das Übersetzungssystem funktioniert auf ähnliche Weise wie die Grav-Konfiguration, und es gibt mehrere Orte und Wege, wie Sie Übersetzungen erstellen können.

Der erste Ort, an dem Grav nach Übersetzungsdateien sucht, befindet sich im Ordner `system/languages`. Die Dateien werden vorzugsweise in dem angegebenen Format erstellt: `en.yaml`, `fr.yaml`, usw.  Jede Yaml-Datei sollte ein Array oder verschachtelte Arrays von Schlüssel-/Werte-Paaren enthalten:

[prism classes="language-yaml line-numbers"]
SITE_NAME: My Blog Site
HEADER:
    MAIN_TEXT: Welcome to my new blog site
    SUB_TEXT: Check back daily for the latest news
[/prism]

Zur einfacheren Identifizierung bevorzugt Grav die Verwendung von Zeichenketten in Großbuchstaben, da dies bei der Erkennung unübersetzter Strings hilft. Außerdem wird dadurch die Darstellung deutlicher, wenn sie in Twig-Vorlagen verwendet werden.

Grav hat die Fähigkeit, auf die unterstützten Sprachen zurückzugreifen, um eine Übersetzung zu finden, falls eine Übersetzung für die aktive Sprache nicht gefunden wird.  Dies ist standardmäßig aktiviert, kann aber über die Option `translations_fallback` deaktiviert werden:

[prism classes="language-yaml line-numbers"]
languages:
  translations_fallback: true
[/prism]

!!! Helfen Sie Grav, einer größeren Anzahl von Benutzern zugänglich zu werden und stellen Sie Übersetzungen in **Ihrer Sprache** zur Verfügung. Wir verwenden die [Crowdin-Übersetzungsplattform](https://crowdin.com/), um die Übersetzung des [Grav-Kerns](https://crowdin.com/project/grav-core) und des [Grav-Admin-Plugins](https://crowdin.com/project/grav-admin) zu ermöglichen. [Melden Sie sich an](https://crowdin.com/join) und beginnen Sie noch heute mit dem Übersetzen!

### Language-Switcher für die Sprachumschaltung

Sie können ein einfaches Plugin zur Sprachumschaltung **Language Switching**  über das Admin-Plugin oder über den GPM herunterladen:

[prism classes="language-bash command-line"]
bin/gpm install langswitcher
[/prism]

Die [Dokumentation](https://github.com/getgrav/grav-plugin-langswitcher) zur Konfiguration und Implementierung finden Sie auf GitHub.


### Setup with Language Specific Domains

Mit dem [umgebungsbasierten Sprach-Handling](#environment-based-language-handling) können Sie Ihre Website so konfigurieren, dass den Domains Standardsprachen (die erste Sprache) zugewiesen werden.


Vergewissern Sie sich, dass die Option ...

[prism classes="language-yaml line-numbers"]
pages.redirect_default_route: true
[/prism]

... in Ihrer `system.yaml` auf `true` gesetzt ist.

Ergänzen Sie Ihre **.htaccess**-Datei und passen Sie die Sprach-Slugs und Domain-Namen Ihren Bedürfnissen an:

[prism classes="language-htaccess line-numbers"]
# http://www.cheat-sheets.org/saved-copy/mod_rewrite_cheat_sheet.pdf
# http://www.workingwith.me.uk/articles/scripting/mod_rewrite

# handle top level e.g. http://grav-site.com/de
RewriteRule ^en/?$ "http://grav-site.com" [R=302,L]
RewriteRule ^de/?$ "http://grav-site.de" [R=302,L]

# handle sub pages, exclude admin path
RewriteCond %{REQUEST_URI} !(admin) [NC]
RewriteRule ^en/(.*)$ "http://grav-site.com/$1" [R=302,L]
RewriteCond %{REQUEST_URI} !(admin) [NC]
RewriteRule ^de/(.*)$ "http://grav-site.de/$1" [R=302,L]
[/prism]

Falls Sie wissen, wie man die Rewrite-Regeln vereinfachen kann, bearbeiten Sie bitte diese Seite auf GitHub, indem Sie, oben auf der Seite, auf den Link **Edit** klicken.

### Sprachlogik in Twig-Templates

Häufig besteht die Notwendigkeit, auf den Sprachstatus und die Logik von Twig-Templates aus zuzugreifen.  Wenn Sie beispielsweise auf eine bestimmte Bilddatei zugreifen müssen, die für pro Sprache unterschiedlich ist und einen anderen Namen hat (`myimage.en.jpg` und `myimage.fr.jpg`).

Um die richtige Version des Bildes anzeigen zu können, müssten Sie die aktuelle, aktive Sprache kennen.  Unter Grav geschieht das durch Zugriff auf das Objekt `Language` über das Objekt `Grav` und den Aufruf des entsprechenden Verfahrens. Im Beispiel oben könnte dies mit dem folgenden Twig-Code erreicht werden:

[prism classes="language-twig line-numbers"]
{{ page.media.images['myimage.'~grav.language.getActive~'.jpg'].html()|raw }}
[/prism]

Der Aufruf `getActive` in Twig bewirkt, dass `Language->getActive()` aufgefordert wird, den aktuellen aktiven Sprachcode zurückzuliefern. Ein paar weitere nützliche Language-Methoden sind:

* `getLanguages()` – Gibt ein Array mit allen unterstützten Sprachen zurück
* `getLanguage()` – Gibt die aktuell aktive Sprache zurück, andernfalls die Standardsprache
* `getActive()` – Gibt die aktuell aktive Sprache zurück
* `getDefault()` – Gibt die Standardsprache (erste Sprache) zurück

Für eine vollständige Liste der verfügbaren Methoden können Sie in der Datei `<grav root>/system/src/Grav/Common/Language/Language.php` nachschauen.
