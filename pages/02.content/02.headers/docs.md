---
title: Headers / Frontmatter
page-toc:
  active: true
taxonomy:
    category: docs
---

Die Kopfzeilen oder Headers (alternativ als Frontmatter bezeichnet), am oberen Rand einer Seite, sind völlig optional. Sie benötigen sie für die Darstellung einer Seite innerhalb von Grav gar nicht. Es gibt 3 Haupttypen von Seiten (**Standard**, **Listing** und **Modular**) innerhalb von Grav, und jede hat relevante Kopfzeilen.

! Headers sind auch als **Page Frontmatter** bekannt und werden allgemein als solche bezeichnet, um nicht mit HTTP-Headern verwechselt zu werden.

## Einfache Seiten-Headers

Es stehen eine Reihe von grundlegenden Kopfzeilen-Optionen zur Verfügung.

### Cache aktivieren

```yaml
cache_enable: false
```

Standardmäßig wird Grav den Inhalt der Seitendatei zwischenspeichern, um zu gewährleisten, dass alles so schnell wie möglich ausgeführt wird. Es gibt aber auch fortgeschrittene Szenarien, in denen die Seite nicht zwischengespeichert werden sollte.

Als Beispiel hierfür dient die Verwendung von dynamischen Twig-Variablen in Ihrem Inhalt. Die Variable `cache_enable` erlaubt es, dieses Verhalten zu übersteuern.  Wir werden die Twig-Inhaltsvariablen in einem späteren Kapitel behandeln. Gültige Werte sind `true` oder `false`.

### Datum

```yaml
date: 01/01/2020 3:14pm
```

Die Variable `date` erlaubt es, spezifisch ein mit dieser Seite verbundenes Datum festzulegen.  Dies wird häufig verwendet, um anzuzeigen, wann ein Beitrag erstellt wurde und kann zur Anzeige oder zum Sortieren verwendet werden.  Wenn diese Variable nicht gesetzt ist, wird die letzte **modifizierte Zeit** der Seite automatisch verwendet.

! Datumsangaben in den Formaten `m/d/y` oder `d-m-y` werden durch die Berücksichtigung des Trennzeichens zwischen den verschiedenen Komponenten eindeutig unterschieden: Ist das Trennzeichen ein Schrägstrich (`/`), so wird das **amerikanische** `m/d/y` Format angenommen; ist das Trennzeichen dagegen ein Bindestrich (`-`) oder ein Punkt (`.`), so wird das **europäische** `d-m-y` Format angenommen.

### Menü

```yaml
menu: My Page
```

Über die Variable `menu` können Sie den Text einstellen, der in der Navigation verwendet werden soll. Es gibt mehrere Ebenen von Rückblenden für das Menü, wenn also keine `menu`-Variable gesetzt ist, wird Grav versuchen, die Variable `title` zu verwenden.

### Veröffentlicht

```yaml
published: true
```

Standardmäßig wird eine Seite **veröffentlicht**, es sei denn, Sie setzen explizit `published: false` oder über ein in der Zukunft liegendes `publish_date` bzw. ein in der Vergangenheit liegendes `unpublish_date`. Gültige Werte sind `true` oder `false`.

### Slug

```yaml
slug: my-page-slug
```

Die Variable `slug` erlaubt es, gezielt den Teil der URL der Seite festzulegen. Zum Beispiel: `http://yoursite.com/my-page-slug` wäre die URL, wenn Sie `slug`, wie oben, setzen würden.  Wenn die Variable `slug` in der Seite nicht gesetzt ist, greift Grav auf den Ordnernamen zurück (ohne irgendwelche numerischen Präfixe).

[Slugs](http://en.wikipedia.org/wiki/Semantic_URL#Slug) werden generell komplett kleingeschrieben, wobei Akzentzeichen durch Buchstaben des englischen Alphabets und Leerzeichen durch einen Bindestrich oder einen Unterstrich ersetzt werden. Obwohl zukünftige Versionen von Grav Leerzeichen in Slugs unterstützen werden, wird von einer Verwendung von Leerzeichen oder Großbuchstaben dringend abgeraten.

Hier ein Beispiel: Wenn der Titel eines Blog-Beitrags `Blog Post Beispiel` lautet, dann wäre der empfohlene Slug `blog-post-beispiel`.

### Taxonomie

```yaml
taxonomy:
    category: blog
    tag: [sample, demo, grav]
```

`taxonomy` ist eine sehr nützliche Header-Variable. Damit können Sie der **Taxonomie** Werte zuweisen, die Sie in der [Site-Konfiguration](../../basics/grav-configuration#site-configuration) als gültige Typen definiert haben.

Wenn die Taxonomie in dieser Datei nicht definiert ist, wird sie ignoriert. In diesem Beispiel ist die Seite als in der Kategorie `blog` definiert und hat die Tags: `sample`, `demo`, und `grav`. Diese Taxonomien können verwendet werden, um andere Seiten, Plugins und sogar Themes zu suchen. Im Kapitel [Taxonomy](../taxonomy) wird dieses Konzept ausführlicher behandelt.

### Titel

Wenn Sie überhaupt keine Kopfzeilen verwenden, haben Sie keine Kontrolle über den Titel der Seite, wie er im Browser und in den Suchmaschinen angezeigt wird.  Aus diesem Grund wird empfohlen, _mindestens_ die Variable `title` in die Kopfzeile der Seite zu setzen:

```yaml
title: Title of my Page
```

Wenn die Variable `title` nicht gesetzt ist, hat Grav eine Fallback-Lösung und wird versuchen, die großgeschriebene Variable `slug` zu verwenden.

## Fortgeschrittene Header-Funktionen

Diese sind nicht weniger wichtig, werden aber seltener verwendet. Sie können verwendet werden, um erweiterte Funktionen innerhalb Ihrer Seite einzurichten.

### URL-Erweiterung anhängen

```yaml
append_url_extension: '.json'
```

Ermöglicht es der Seite, die Standarderweiterung zu überschreiben und stattdessen eine programmgesteuert festzulegen.  Sie setzt auch die entsprechenden Header-Attribute für die Rückmeldung.

### Cache-Kontrolle

```yaml
cache_control: max-age=604800
```

Kann leer sein für keine Einstellung oder einen [gültigen](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Cache-Control) Textwert für `cache-control` haben.

! Achten Sie darauf, dass Sie `no-cache` verwenden, falls die Seite Informationen enthält, die sich je nach Benutzer ändern kann. Andernfalls kann der Inhalt an andere Benutzer weitergegeben werden. Die Einstellung [Expires](/content/headers#expires) hat die gleiche Wirkung, falls Sie `expires: 0` eingestellt haben.

### Datum-Format

```yaml
dateformat: 'Y-m-d H:i:s'
```

Überschreibt die standardmäßige Grav-Konfiguration für Datumformate und lässt sich auf Seitenebene einstellen. Sie können jedes der verfügbaren [PHP-Datumformate](https://php.net/manual/en/datetime.formats.date.php) verwenden.

### Debugger

Wenn Sie den Debugger über die Konfigurationsdatei `system.yaml` aktivieren, wird der Debugger auf jeder Seite angezeigt.  Es gibt Fälle, in denen dies nicht erwünscht ist oder zu Konflikten mit der Darstellung führen kann.  Ein solches Beispiel ist, wenn Sie eine Seite anfordern, die gerendertes HTML an einen Ajax-Aufruf zurückgeben soll.  Dabei sollte der Debugger nicht in die resultierenden Daten injiziert werden.  Um den Debugger auf dieser Seite zu deaktivieren, können Sie den Seiten-Header `debugger` verwenden:

```yaml
debugger: false
```

### ETag

```yaml
etag: true
```

Aktivieren oder deaktivieren Sie auf Seitenebene die Anzeige einer ETag-Header-Variablen mit einem eindeutigen Wert. In der Voreinstellung ist dies `false`, es sei denn, der Wert wird in Ihrer `system.yaml` überschrieben.

### Expires (ungültig werden)

```yaml
expires: 604800
```

Verfallszeit der Seite in Sekunden (604800 Sekunden = 7 Tage) (`no cache` ist auch möglich).

! Achten Sie darauf, dass Sie die Einstellung `expires: 0` verwenden, falls die Seite Informationen enthält, die sich je nach Benutzer ändern können. Andernfalls kann der Inhalt an andere Benutzer weitergegeben werden.

### Externe Url

```yaml
external_url: https://www.mysite.com/foo/bar
```

Ermöglicht Ihnen, die dynamisch generierte URL mit einer von Ihnen explizit angegebenen URL zu überschreiben.


### HTTP Antwort-Code

```yaml
http_response_code: 404
```

Ermöglicht die dynamische Einstellung eines HTTP Response-Codes.

### Language (Sprache)

```yaml
language: fr
```

Damit können Sie die Sprache für eine bestimmte Seite überschreiben.

### LastModified

```yaml
last_modified: true
```

Aktivieren oder deaktivieren Sie auf Seitenebene, ob eine zuletzt geänderte Header-Variable mit Änderungsdatum angezeigt werden soll oder nicht. In der Voreinstellung ist diese Variable `false`, es sei denn, sie wird in Ihrer `system.yaml` überschrieben.

### Lightbox

```yaml
lightbox: true
```

Obwohl es sich dabei streng genommen nicht um einen Standard-Seitenheader handelt, ist es eine übliche Methode, um das Laden eines standardisierten Lightbox-JavaScripts und CSS für eine Seite zu ermöglichen.  Normalerweise lädt das Core Theme `antimatter` nicht die notwendigen Anforderungen, um die Lightbox-Fähigkeiten von Bildern zu aktivieren. Sie sollten daher ein Lightbox-Plugin wie **Featherlight** installieren, das über GPM verfügbar ist.

### Login Redirect Here

```yaml
login_redirect_here: false
```

Mit der Kopfzeile `login_redirect_here` können Sie bestimmen, ob jemand nach dem Einloggen über das [Grav Login Plugin](https://github.com/getgrav/grav-plugin-login) auf dieser Seite verbleibt oder nicht. Wenn Sie diesen Header auf `false` setzen, wird jemand nach einem erfolgreichen Login auf die vorherige Seite umgeleitet.

Eine `true` Einstellung hier ermöglicht es der Person, nach einem erfolgreichen Login auf der aktuellen Seite zu bleiben. Dies ist auch die Standardeinstellung, die gilt, wenn es keinen `login_redirect_here` Header in dem Frontmatter gibt.

Sie können dieses Standardverhalten außer Kraft setzen, indem Sie einen vorgegebenen Pfad erzwingen, indem Sie ausdrücklich eine Option in Ihrer Login-Konfiguration YAML angeben:

```yaml
redirect_after_login: '/profile'
```

Damit werden Sie nach einem erfolgreichen Login immer auf die Route `/profile` geleitet.

### Markdown

```yaml
  markdown:
    extra: false
    auto_line_breaks: false
    auto_url_links: false
    escape_markup: false
    special_chars:
      '>': 'gt'
      '<': 'lt'
```

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **extra:** | Unterstützung für Markdown-Extra aktivieren (GFM standardmäßig) |
| **auto_line_breaks:** | Aktiviert den automatischen Zeilenumbruch |
| **auto_url_links:** | Automatische HTML-Links aktivieren |
| **escape_markup:** | Escape-Markup-Tags in Entitäten |
| **special_chars:** | Liste der automatisch zu konvertierenden Sonderzeichen |
[/div]

Sie können diese global über Ihre Konfigurationsdatei `user/config/system.yaml` aktivieren, oder Sie können diese globale Einstellung _pro Seite_ mit dieser `markdown` Header-Option überschreiben.

### Twig nie cachen

```yaml
never_cache_twig: true
```

Wenn Sie dies aktivieren, können Sie eine Verarbeitungslogik hinzufügen, die sich bei jedem Seitenladevorgang dynamisch ändern kann, anstatt die Ergebnisse zwischenzuspeichern und für jeden Seitenladevorgang zu speichern. Dies kann site-weit in der **system.yaml** oder auf einer bestimmten Seite aktiviert/deaktiviert werden. Kann auf `true` oder `false` gesetzt werden.

Dies ist eine subtile Änderung, aber eine, die besonders bei modularen Seiten nützlich ist, da sie verhindert, dass Sie das Caching ständig deaktivieren müssen, wenn Sie damit arbeiten. Die Seite wird immer noch zwischengespeichert, aber nicht mehr Twig. Der Twig wird erst verarbeitet, nachdem der zwischengespeicherte Inhalt abgerufen wurde. Bei modularen Formularen arbeitet es jetzt mit genau dieser Einstellung, anstatt den modularen Seiten-Cache deaktivieren zu müssen.

!! Das ist derzeit nicht mit `twig_first: true` kompatibel, da die gesamte Verarbeitung in einem einzigen Twig-Aufruf erfolgt.

### Process

```yaml
process:
  markdown: false
  twig: true
```

Die Verarbeitung der Seite ist eine weitere fortgeschrittene Fähigkeit. Standardmäßig verarbeitet Grav `markdown`, aber **nicht** `twig` in einer Seite.  Die Entscheidung, "Twig" standardmäßig nicht zu verarbeiten, ist rein aus Leistungsgründen getroffen worden, da es sich hierbei nicht um eine häufig benötigte Funktion handelt.  Die Variable `process` erlaubt es Ihnen, dieses Verhalten zu überschreiben.

Möglicherweise möchten Sie `markdown` auf einer bestimmten Seite deaktivieren, wenn Sie 100% HTML in Ihrer Seite einsetzen und den Markdown-Prozess überhaupt nicht laufen lassen wollen.  Außerdem erlaubt es einem Plugin, Inhalte auf eine andere Art und Weise vollständig zu verarbeiten. Gültige Werte sind `true` oder `false`.

Es gibt Situationen, in denen Sie die Twig-Templating-Funktionalität in Ihrem Inhalt verwenden möchten. Dazu müssen Sie die Variable `twig` auf true setzen.

### Twig zuerst verarbeiten

```yaml
twig_first: false
```

Wenn auf `true` gesetzt, erfolgt die Twig-Verarbeitung vor der Markdown-Verarbeitung. Das kann dann sinnvoll sein, wenn Ihr Twig einen Markdown-Code erzeugt, der verfügbar sein muss, damit er vom Markdown-Compiler verarbeitet werden kann. Eines ist zu beachten: Wenn `cache_enable: false` **und** `twig_first: true` gesetzt ist, wird das Seiten-Caching effektiv deaktiviert.

### Datum der Veröffentlichung

```yaml
publish_date: 01/23/2020 13:00
```

Optionales Feld, man kann ein Datum angeben, um die Veröffentlichung automatisch auszulösen. Gültige Werte sind alle String-Datumswerte, die [strtotime()](https://www.php.net/manual/de/function.strtotime.php) unterstützt.

### Redirect

```yaml
redirect: '/some/custom/route'
```

oder

```yaml
redirect: 'http://someexternalsite.com'
```

Sie können direkt aus einem Seitenheader auf eine andere interne oder externe Seite umleiten.  Das bedeutet natürlich, dass diese Seite nicht angezeigt wird, aber die Seite kann sich immer noch in einer Kollektion, einem Menü usw. befinden, da sie als Seite innerhalb von Grav existiert.

Sie können auch einen Redirect-Code an eine URL anhängen, indem Sie eckige Klammern verwenden:

```yaml
redirect: '/some/custom/route[303]'
```

### Routen

```yaml
routes:
  default: '/my/example/page'
  canonical: '/canonical/url/alias'
  aliases:
    - '/some/other/route'
    - '/can-be-any-valid-slug'
```

Sie können jetzt eine **Standard-Route** (default) angeben, die die Standard-Route-Struktur überschreibt, wie sie durch die Ordnerstruktur definiert ist.

Sie können auch eine besondere **kanonische Route** festlegen, die in Themes verwendet werden kann, um einen kanonischen Link anzuzeigen:

```html
<link rel="canonical" href="https://yoursite/dresses/green-dresses-are-awesome" />
```

Schließlich können Sie eine Reihe von **Routen-Aliasen** angeben, die als alternative Routen für eine bestimmte Seite verwendet werden können.

### Routable (Routingfähig)

```yaml
routable: false
```

Normalerweise sind alle Seiten **routingfähig**.  Das bedeutet, dass sie erreicht werden können, indem Sie mit Ihrem Browser auf die URL der Seite zeigen.  Es kann jedoch sein, dass Sie eine Seite erstellen müssen, die für einen bestimmten Inhalt erstellt wurde, die aber direkt von einem Plugin, einem anderen Inhalt oder sogar direkt von einem Theme aufgerufen werden soll.  Ein gutes Beispiel hierfür ist eine `404-Fehler`-Seite.

Grav sucht automatisch nach einer Seite mit der Route `/error`, wenn eine andere Seite nicht gefunden werden kann.  Da es sich um eine tatsächliche Seite innerhalb von Grav handelt, hätten Sie die vollständige Kontrolle darüber, wie diese Seite aussieht.  Wahrscheinlich möchten Sie jedoch nicht, daß die Leute diese Seite direkt in ihrem Browser aufrufen, so daß die Variable `routable` für diese Seite üblicherweise auf false gesetzt ist. Gültige Werte sind `true` oder `false`.

### SSL

```yaml
ssl: true
```

Sie können jetzt zwangsweise eine bestimmte Seite mit SSL **on** oder **off**  aufrufen.  Das **funktioniert nur** mit der Option `absolute_urls: true`, die in der Konfiguration `system.yaml` gesetzt wird.  Das liegt daran, dass Sie vollständige URLs inklusive dem enthaltenen Protokoll und Host verwenden müssen, um zwischen SSL- und Nicht-SSL-Seiten hin- und herwechseln zu können.

### Summary (Zusammenfassung)

```yaml
summary:
  enabled: true
  format: short | long
  size: int
```

Die Option **summary** bestimmt, was die Methode `page.summary()` zurückgeben soll.  Diese Methode wird am häufigsten in einem Szenario vom Typ Blog-Listing verwendet, kann aber immer dann angewendet werden, wenn Sie eine Übersicht oder Zusammenfassung des Seiteninhalts benötigen.  Die Szenarien sind wie folgt:

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enabled:** | Ausschalten der Seitenübersicht (die Zusammenfassung zeigt das gleiche an wie der Seiteninhalt) |
| **format:** | <ul><li>`long` = Alle Zusammenfassungen, die den eigentlichen Inhalt einschränken, werden ignoriert.<li>`short` = Erkennen und Abschneiden von Inhalten bis zur Position des Zusammenfassung-Trennzeichens</ul> |
[/div]

Das Attribut `size` hat unterschiedliche Bedeutungen, wenn das Format auf `short` oder `long` gesetzt ist:

[div class="table-keycol"]
| Short/Size | Beschreibung |
| -------- | ----------- |
| **size: 0** | Wenn kein Trennzeichen für die Zusammenfassung gefunden wird, entspricht die Zusammenfassung dem gesamten Seiteninhalt, andernfalls wird der Inhalt bis zur Position des Trennzeichens verkürzt. |
| **size:** `int` | Der Inhalt wird immer nach der Anzahl von **int** Zeichen abgeschnitten. Wenn ein Trennzeichen für die Zusammenfassung gefunden wurde, dann kürzt es den Inhalt bis zur Position des Trennzeichens. |
[/div]

[div class="table-keycol"]
| Long/Size | Beschreibung |
| -------- | ----------- |
| **size: 0** | Die Zusammenfassung entspricht dem gesamten Seiteninhalt. |
| **size:** `int` | Der Inhalt wird immer nach der Anzahl von **int** Zeichen abgeschnitten, unabhängig von der Position des Trennzeichens. |
[/div]

### Template

```yaml
template: custom
```

Wie im [vorherigen Kapitel](../content-pages) erläutert, basiert das Template des Themes, das zum Rendern einer Seite verwendet wird, auf dem Dateinamen der `.md` Datei.

Daher wird eine Datei namens `default.md`, das Template `default` im aktiven Thema verwenden.  Sie können dieses Verhalten natürlich übersteuern, indem Sie einfach die Variable `template` in der Headerzeile setzen und ein anderes Template wählen.

Im obigen Beispiel wird die Seite das Template `custom` des Themes verwenden.  Diese Variable existiert, weil Sie das Template einer Seite möglicherweise programmtechnisch von einem Plugin aus ändern müssen.

### Template-Format

```yaml
template_format: xml
```

Wenn Sie möchten, dass eine Seite ein bestimmtes Format ausgibt (d.h.: xml, json usw.), müssen Sie das Format an die URL anhängen. Zum Beispiel würde die Eingabe von `http://example.com/sitemap.xml` den Browser anweisen, den Inhalt unter Verwendung des Twig-Templates `xml` mit der Endung `.xml.twig` zu rendern. Das ist gut so, denn wir mögen es, in Grav Dinge einfach zu machen.

Mit Hilfe des Seiten-Headers `template_format` können wir dem Browser sagen, wie er die Seite ohne notwendige URL-Extensions darstellen soll. Durch die Eingabe von `template_format: xml` in unsere Seite `sitemap` können wir `http://example.com/sitemap` für uns verwenden, ohne `.xml` an das Ende der Seite anhängen zu müssen.

[Diese Methode](https://github.com/getgrav/grav-plugin-sitemap/commit/00c23738bdbfe9683627bf0f99bda12eab9505d5#diff-190081f40350c0272970d9171f3437a2) haben wir mit dem [Grav Sitemap Plugin](https://github.com/getgrav/grav-plugin-sitemap) verwendet.

### Unpublish Date (Datum der Aufhebung der Veröffentlichung)

```yaml
unpublish_date: 05/17/2020 00:32
```

Optionales Feld, kann aber ein Datum angeben, um automatisch die Aufhebung der Veröffentlichung zu bewirken. Gültige Werte sind alle String-Datumswerte, die [strtotime()](https://php.net/manual/en/function.strtotime.php) unterstützt.

### Visible (Sichtbar)

```yaml
visible: false
```

Standardmäßig ist eine Seite in der **Navigation** dann **sichtbar**, wenn der umgebende Ordner ein numerisches Präfix hat, d.h. `/01.home` ist sichtbar, während `/error` **nicht sichtbar** ist. Dieses Verhalten kann überschrieben werden, indem die Variable `visible` in der Kopfzeile gesetzt wird. Gültige Werte sind `true` oder `false`.

## Eigene Seiten-Header

Selbstverständlich können Sie Ihre eigenen benutzerdefinierten Seiten-Kopfzeilen mit jeder gültigen YAML-Syntax erstellen. Diese wären seitenspezifisch und für jedes Plugin oder Theme verfügbar. Ein gutes Beispiel hierfür wäre das Setzen einer für ein Sitemap-Plugin bestimmte Variable, wie z.B:

[prism classes="language-yaml line-numbers"]
sitemap:
    changefreq: monthly
    priority: 1.03
[/prism]

Diese Header haben die Eigenschaft, dass Grav sie nicht standardmäßig verwendet. Sie werden nur vom **Sitemap-Plugin** gelesen, um festzustellen, wie oft diese spezielle Seite geändert wird und welche Priorität sie haben soll.

Jeder derartige Seiten-Header sollte dokumentiert werden. Im Allgemeinen sollte es auch einen Standardwert geben, der dann angewandt wird, wenn die Seite keinen Wert vorgibt.

Ein anderes Beispiel wäre die Speicherung seitenspezifischer Daten, die dann von Twig im Inhalt der Seite verwendet werden könnten.

So könnten Sie der Seite möglicherweise eine Autorenreferenz zuordnen wollen. Falls Sie diese YAML-Einstellungen in den Seitenkopf eingefügt haben:

[prism classes="language-yaml line-numbers"]
author:
    name: Sandy Johnson
    twitter: @sandyjohnson
    bio: Sandy is a freelance journalist and author of several publications on open source CMS platforms.
[/prism]

Von Twig aus könnten Sie dann auf diese Daten zugreifen:

[prism classes="language-twig line-numbers"]
<section id="author-details">
    <h2>{{ page.header.author.name|e }}</h2>
    <p>{{ page.header.author.bio|e }}</p>
    <span>Contact: <a href="https://twitter.com/{{ page.header.author.twitter|e }}"><i class="fa fa-twitter"></i></a></span>
</section>
[/prism]

## Meta-Seiten-Header

Meta-Header ermöglichen es Ihnen, den [Standardsatz von HTML **<meta> Tags**](http://www.w3schools.com/tags/tag_meta.asp) für jede Seite sowie [OpenGraph](http://ogp.me/), [Facebook](https://developers.facebook.com/docs/sharing/best-practices), und [Twitter](https://dev.twitter.com/cards/overview) festzulegen.

#### Beispiel für Standard-Metatags

[prism classes="language-yaml line-numbers"]
metadata:
    refresh: 30
    generator: 'Grav'
    description: 'Your page description goes here'
    keywords: 'HTML, CSS, XML, JavaScript'
    author: 'John Smith'
    robots: 'noindex, nofollow'
    my_key: 'my_value'
[/prism]

Das erzeugt das entsprechende HTML:

[prism classes="language-twig line-numbers"]
<meta name="generator" content="Grav" />
<meta name="description" content="Your page description goes here" />
<meta http-equiv="refresh" content="30" />
<meta name="keywords" content="HTML, CSS, XML, JavaScript" />
<meta name="author" content="John Smith" />
<meta name="robots" content="noindex, nofollow" />
<meta name="my_key" content="my_value" />
[/prism]

Alle HTML5-Metatags werden unterstützt.

#### Beispiel für OpenGraph-Metatags

[prism classes="language-yaml line-numbers"]
metadata:
    'og:title': The Rock
    'og:type': video.movie
    'og:url': http://www.imdb.com/title/tt0117500/
    'og:image': http://ia.media-imdb.com/images/rock.jpg
[/prism]

Das erzeugt das entsprechende HTML:

[prism classes="language-html line-numbers"]
<meta name="og:title" property="og:title" content="The Rock" />
<meta name="og:type" property="og:type" content="video.movie" />
<meta name="og:url" property="og:url" content="http://www.imdb.com/title/tt0117500/" />
<meta name="og:image" property="og:image" content="http://ia.media-imdb.com/images/rock.jpg" />
[/prism]

Eine vollständige Übersicht über alle OpenGraph-Metatags, die verwendet werden können, finden Sie in der [offiziellen Dokumentation](http://ogp.me/).

#### Beispiel für Facebook-Metatags

[prism classes="language-yaml line-numbers"]
metadata:
    'fb:app_id': your_facebook_app_id
[/prism]

Das erzeugt das entsprechende HTML:

[prism classes="language-html line-numbers"]
<meta name="fb:app_id" property="fb:app_id" content="your_facebook_app_id" />
[/prism]

Facebook verwendet hauptsächlich OpenGraph-Metatags, aber es gibt einige spezifische Facebook-Tags, die automatisch von Grav unterstützt werden.

#### Beispiel für Twitter-Metatags

[prism classes="language-yaml line-numbers"]
metadata:
    'twitter:card' : summary
    'twitter:site' : @flickr
    'twitter:title' : Your Page Title
    'twitter:description' : Your page description can contain summary information
    'twitter:image' : https://farm6.staticflickr.com/5510/14338202952_93595258ff_z.jpg
[/prism]

Das erzeugt das entsprechende HTML:

[prism classes="language-twig line-numbers"]
<meta name="twitter:card" property="twitter:card" content="summary" />
<meta name="twitter:site" property="twitter:site" content="@flickr" />
<meta name="twitter:title" property="twitter:title" content="Your Page Title" />
<meta name="twitter:description" property="twitter:description" content="Your page description can contain summary information" />
<meta name="twitter:image" property="twitter:image" content="https://farm6.staticflickr.com/5510/14338202952_93595258ff_z.jpg" />
[/prism]

Eine vollständige Übersicht über alle Twitter-Metatags, die verwendet werden können, finden Sie in der [offiziellen Dokumentation](https://dev.twitter.com/cards/overview).

Das bietet wirklich eine enorme Flexibilität und Leistungsstärke.

## Frontmatter.yaml

Eine erweiterte Funktion, die für einige Power-User nützlich sein kann, ist die Möglichkeit, gemeinsame Frontmatter-Werte über eine `frontmatter.yaml`-Datei im Seitenordner zu verwenden.  Besonders nützlich ist das bei der Arbeit mit mehrsprachigen Sites, bei denen Sie einen Teil der Frontmatter-Werte unter allen Sprachversionen einer bestimmten Seite austauschen möchten.

Um dies auszunutzen, erstellen Sie einfach eine `frontmatter.yaml`-Datei neben der `.md`-Datei Ihrer Seite und fügen Sie alle gültigen Frontmatter-Werte hinzu.  Zum Beispiel:

[prism classes="language-yaml line-numbers"]
metadata:
    generator: 'Super Grav'
    description: Give your page a powerup with Grav!
[/prism]

! Wenn eine Kopfzeile sowohl in frontmatter.yaml als auch im Seiten-Frontmatter definiert ist, werden die Werte aus der Seite verwendet, die Werte aus frontmatter.yaml werden überschrieben.

!!!! Die Verwendung von frontmatter.yaml ist ein Feature auf der Dateiseite und wird vom Admin-Plugin **nicht unterstützt**.
