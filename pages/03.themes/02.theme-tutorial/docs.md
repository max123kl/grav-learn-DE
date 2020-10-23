---
title: Theme-Tutorial
page-toc:
  active: true
taxonomy:
    category: docs
---

Der beste Weg, etwas Neues zu lernen, ist oft ein konkretes Beispiel. Dann versucht man, seine eigene Entwicklung darauf aufzubauen. Wir werden dieselbe Technik bei der Gestaltung eines neuen Grav-Themes anwenden.

## Quark

Grav wird mit dem einfachen und modernen Theme **Quark** ausgeliefert, das das [Spectre.css-Framework](https://picturepan2.github.io/spectre/) verwendet.

Spectre.css ist ein schlankes, responsives und modernes CSS-Framework für eine zügige und erweiterbare Programmierung.

Spectre bietet elementare Styles für Typografie und Elemente, ein auf Flexbox basierendes responsives Layoutsystem, reine CSS-Komponenten und Hilfsprogramme mit Best-Practice-Codierung und konsistenter Formensprache.

Oft ist es jedoch besser, von etwas noch einfacherem auszugehen.

## Pure.css

Für dieses Tutorial werden wir ein Theme erstellen, das auf dem beliebten [Pure.css-Framework](http://purecss.io/) basiert, das von Yahoo entwickelt wurde.

Pure ist ein kleines, schnelles und responsives CSS-Framework, das die Grundlagen für die Weiterentwicklung einer Website enthält, ohne den Overhead größerer Frameworks wie [Bootstrap](http://getbootstrap.com/css/) oder [Foundation](http://foundation.zurb.com/). Es enthält mehrere Module, die unabhängig voneinander verwendet werden können, aber alles zusammen ist das daraus entstandene Paket **gezippt nur 4,0KB klein**!

Sie können sich auf der [Pure.css Projekt-Site](http://purecss.io/) über alle Funktionen informieren, die Pure mitbringt.

Außerdem sollten Sie den Blog-Artikel [Wichtige Themes-Updates](https://getgrav.org/blog/important-theme-updates) lesen, der einige wichtige Änderungen der Grav-Themes beschreibt, um die optimale Plugin-Unterstützung für die Zukunft zu sichern.

## Stufe 1 – Installieren des Plugins DevTools

!! Frühere Versionen dieses Tutorials erforderten zunächst die Einrichtung eines Basis-Themes.  Dieser ganze Prozess kann dank unseres neuen **DevTools Plugin** übersprungen werden.

Der erste Schritt bei der Erstellung eines neuen Themes ist die **Installation des Plugin DevTools**. Das kann man auf zwei verschiedenen Wegen erledigen.

#### Installation über CLI GPM

* Wechseln Sie in der Kommandozeile zum Stammverzeichnis Ihrer Grav-Installation.

[prism classes="language-bash command-line"]
bin/gpm install devtools
[/prism]

#### Installation über das Admin-Plugin

* Nach dem Login, in der Seitenleiste, zum Bereich **Plugins** gehen
* Oben rechts auf die Schaltfläche <i class="fa fa-plus"></i> **Hinzufügen** klicken
* Find **DevTools** in the list and click the <i class="fa fa-plus"></i> **Install** button.

## Stufe 2 – Das Basis-Theme anlegen

Im nächsten Schritt müssen Sie unbedingt auf der [Kommandozeile](/cli-console/command-line-intro) sein, da die DevTools eine Reihe von CLI-Befehle enthalten, die den Prozess der Erstellung eines neuen Themes erheblich erleichtern!

Geben Sie im Stammverzeichnis Ihrer Grav-Installation den folgenden Befehl ein:

[prism classes="language-bash command-line"]
bin/plugin devtools new-theme
[/prism]

In der Folge werden Ihnen einige Fragen gestellt, die zur Erstellung des neuen Themes erforderlich sind:

! Wir werden **pure-blank** verwenden, um ein neues Theme zu erstellen. Sie können aber auch ein schlichtes Template im **inheritance** Style erstellen, das von einem anderen Basis-Theme abgeleitet ist.

[prism classes="language-bash command-line" cl-output="2-15"]
bin/plugin devtools new-theme

Enter Theme Name: MyTheme
Enter Theme Description: My New Theme
Enter Developer Name: Acme Corp
Enter Developer Email: contact@acme.co
Please choose a template type
  [pure-blank ] Basic Theme using Pure.css
  [inheritance] Inherit from another theme
  [copy       ] Copy another theme
 > pure-blank

SUCCESS theme mytheme -> Created Successfully

Path: /www/user/themes/my-theme
[/prism]

Der DevTools-Befehl zeigt Ihnen an, wo dieses neue Template erstellt wurde. Das neue Template ist voll funktionsfähig, aber auch sehr einfach.  Sie werden es an Ihre Anforderungen anpassen wollen.

Um Ihr neues Theme in Aktion zu sehen, müssen Sie das Standardthema von `quark` zu `my-theme` ändern. Bearbeiten Sie also Ihre Datei `user/config/system.yaml` und schreiben Sie:

[prism classes="language-yaml line-numbers"]
...
pages:
    theme: my-theme
...
[/prism]

Laden Sie Ihre Website im Browser neu – Sie müssten jetzt erkennen, dass sich das Theme geändert hat.

## Stufe 3 – Theme Basics

Jetzt haben wir ein neues Basis-Theme geschaffen, das modifiziert und weiterentwickelt werden kann. Schauen wir uns doch einmal an, was ein Theme umfasst.  Wenn Sie in den Ordner `user/themes/my-theme` hineinschauen, sehen Sie folgendes:

[prism classes="language-text"]
.
├── CHANGELOG.md
├── LICENSE
├── README.md
├── blueprints.yaml
├── css
│   └── custom.css
├── fonts
├── images
│   └── logo.png
├── js
├── my-theme.php
├── my-theme.yaml
├── screenshot.jpg
├── templates
│   ├── default.html.twig
│   ├── error.html.twig
│   └── partials
│       ├── base.html.twig
│       ├── header.html.twig
│       ├── metadata.html.twig
│       └── navigation.html.twig
└── thumbnail.jpg
[/prism]

Dies ist eine Beispielstruktur, aber es sind noch einige Dinge erforderlich:

### Zum Funktionieren notwendige Elemente

Diese Elemente sind entscheidend und Ihr Theme wird nicht zuverlässig arbeiten, wenn Sie diese Elemente nicht in Ihr Theme aufnehmen.

* **`blueprints.yaml`** – Die Konfigurationsdatei, die Grav nutzt, um Informationen zu Ihrem Theme zu erhalten. Dort kann auch ein Formular definiert werden, das im Adminbereich angezeigt werden kann, wenn die Theme-Details angezeigt werden. In diesem Formular können Sie die Einstellungen für das Theme speichern. [Diese Datei ist im Kapitel Formulare dokumentiert](/forms/blueprints).
* **`my-theme.php`** – Die Benennung dieser Datei folgt dem Theme-Namen. Darin kann jede Logik enthalten sein, die Ihr Theme benötigt. Sie können jeden beliebigen [Plugin Ereignis-Hook](/plugins/event-hooks) verwenden, mit Ausnahme von `onPluginsInitialized()`. Es gibt auch den spezifischen Hook `onThemeInitialized()` für Themes, den Sie stattdessen verwenden können.
* **`my-theme.yaml`** – Diese Konfiguration wird vom Plugin verwendet, um Optionen zu setzen, die das Theme verwenden könnte.
* **`templates/`** – Dieses Verzeichnis enthält die Twig-Templates zum Rendern der Seiten.

### Für die Freigabe erforderliche Elemente

Diese Elemente sind erforderlich, wenn Sie Ihr Theme über GPM veröffentlichen möchten.

* **`CHANGELOG.md`** - Eine Datei, die dem [Grav Changelog Format](/advanced/grav-development#changelog-format) entspricht, um Änderungen in den Versionen anzuzeigen.
* **`LICENSE`** - Eine Lizenzdatei, vermutlich MIT, es sei denn, Sie benötigen ausdrücklich etwas anderes.
* **`README.md`** - Ein „Readme“, das die Dokumentation zum Theme enthalten sollte.  Wie man es installiert, konfiguriert und benutzt.
* **`screenshot.jpg`** - 1009px x 1009px Screenshot zum Theme.
* **`thumbnail.jpg`** - 300px x 300px Screenshot zum Theme.


## Stufe 4 – Basis-Template

Wie Sie aus dem [letzten Kapitel](../theme-basics) wissen, hat jedes Inhaltselement in Grav einen bestimmten Dateinamen, z.B. `default.md`, das Grav anweist, nach einem Twig-Template, hier `default.html.twig`, zu suchen, das für das Rendering zuständig ist. Alles, was Sie zum Anzeigen einer Seite benötigen, können Sie in diese eine Datei packen und es würde gut laufen. Es gibt allerdings eine bessere Lösung.

Mit dem Tag Twig [Extends](http://twig.sensiolabs.org/doc/tags/extends.html) können Sie ein Basis-Layout mit von Ihnen definierten [Blocks](http://twig.sensiolabs.org/doc/tags/block.html) erstellen. So kann jedes Twig-Template das Basis-Template **erweitern** und stellt Definitionen für jeden, der in der Basis erstellten **Blocks** zur Verfügung. Sehen Sie sich die Datei `templates/default.html.twig` an und untersuchen Sie ihren Inhalt:

[prism classes="language-twig line-numbers"]
{% extends 'partials/base.html.twig' %}

{% block content %}
    {{ page.content }}
{% endblock %}
[/prism]

Hier gehen eigentlich zwei Dinge vor sich.

Zunächst erweitert das Template ein anderes Template, das sich in `partials/base.html.twig` befindet.

! Sie brauchen `templates/` nicht in Twig-Templates einzubinden, da Twig bereits selbst in `templates/` sucht, welches die oberste Ebene für jedes Template ist.

Zum anderen wird der Block `content` aus dem Basis-Template überschrieben und an seiner Stelle der Inhalt der Seite ausgegeben.

!! Es ist empfehlenswert und konsistent, den Ordner `templates/partials` zu verwenden, der Twig-Vorlagen enthält, die entweder kleine HTML-Blöcke abbilden oder gemeinsam genutzt werden. Die Ordner `templates/modular` werden für modulare Templates und `templates/forms` für beliebige Formulare verwendet.  Sie können jederzeit eigene Unterverzeichnisse erstellen, wenn Sie Ihre Templates anders organisieren möchten.

Wenn Sie sich die `templates/partials/base.html.twig` anschauen, sehen Sie die Kernstruktur des HTML-Layouts:

[prism classes="language-twig line-numbers"]
{% set theme_config = attribute(config.themes, config.system.pages.theme) %}
<!DOCTYPE html>
<html lang="{{ (grav.language.getActive ?: theme_config.default_lang)|e }}">
<head>
{% block head %}
    <meta charset="utf-8" />
    <title>{% if header.title %}{{ header.title|e }} | {% endif %}{{ site.title|e }}</title>

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    {% include 'partials/metadata.html.twig' %}

    <link rel="icon" type="image/png" href="{{ url('theme://images/logo.png')|e }}" />
    <link rel="canonical" href="{{ page.url(true, true)|e }}" />
{% endblock head %}

{% block stylesheets %}
    {% do assets.addCss('http://yui.yahooapis.com/pure/0.6.0/pure-min.css', 100) %}
    {% do assets.addCss('https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css', 99) %}
    {% do assets.addCss('theme://css/custom.css', 98) %}
{% endblock %}

{% block javascripts %}
    {% do assets.addJs('jquery', 100) %}
{% endblock %}

{% block assets deferred %}
    {{ assets.css()|raw }}
    {{ assets.js()|raw }}
{% endblock %}

</head>
<body id="top" class="{{ page.header.body_classes|e }}">

{% block header %}
    <div class="header">
        <div class="wrapper padding">
            <a class="logo left" href="{{ (base_url == '' ? '/' : base_url)|e }}">
                <i class="fa fa-rebel"></i>
                {{ config.site.title|e }}
            </a>
            {% block header_navigation %}
            <nav class="main-nav">
                {% include 'partials/navigation.html.twig' %}
            </nav>
            {% endblock %}
        </div>
    </div>
{% endblock %}

{% block body %}
    <section id="body">
        <div class="wrapper padding">
        {% block content %}{% endblock %}
        </div>
    </section>
{% endblock %}

{% block footer %}
    <div class="footer text-center">
        <div class="wrapper padding">
            <p><a href="https://getgrav.org">Grav</a> was <i class="fa fa-code"></i> with <i class="fa fa-heart"></i> by <a href="http://www.rockettheme.com">RocketTheme</a>.</p>
        </div>
    </div>
{% endblock %}

{% block bottom %}
    {{ assets.js('bottom')|raw }}
{% endblock %}

</body>
[/prism]

! **TIPP:** Wenn die Variable gefahrlos gerendert werden kann und HTML enthält, sollten Sie immer den Filter `|raw` verwenden, damit das Template mit aktiviertem `autoescape` funktioniert.

!! Es ist sehr wichtig, entweder die Einstellung `autoescape` in Ihrer [Systemkonfiguration](/basics/grav-configuration#twig) zu aktivieren oder daran zu denken, dass jede einzelne Variable in Template-Dateien escaped werden muss, um Ihre Site gegen XSS-Angriffe abzusichern.

## Stufe 5 – Auf den Punkt gebracht

Bitte lesen Sie sich den Code in der Datei `base.html.twig` (siehe oben) durch. So können Sie verstehen, was eigentlich passiert.  Sie sollten einige wichtige Punkte beachten:

1. Die Variable `theme_config` (Zeile 1) wird in der Konfiguration des Themes gesetzt.  Da Twig nicht gut mit Bindestrichen umgehen kann, verwenden wir zum Abfragen von Variablen mit Bindestrichen (z.B. `config.themes.my-theme`) die Twig-Funktion `attribute()`, die Daten dynamisch aus `config.themes` für `my-theme` abruft.

1. Das Element `<html lang=...` (Zeile 3) wird auf die aktive Sprache in Grav gesetzt. Falls es aktiviert ist, ansonsten verwendet es die `default_lang`, wie in der `theme_config` eingestellt.

1. Die Syntax `{% block head %}{% endblock head %}` (Zeile 5-15) definiert einen Bereich in dem Basis-Twig-Template. Beachten Sie, dass die Verwendung von `head` im Tag `{% endblock head %}` nicht erforderlich ist, aber hier aus Gründen der besseren Lesbarkeit verwendet wird. In diesen Block fügen wir alles ein, was sich typischerweise im HTML-Tag `<head>` befindet.

1. Der Tag `<title>` (Zeile 7) wird dynamisch auf der Grundlage der Variablen `title` der Seite gesetzt, wie sie im Header der Seite steht.  Der `header.title` ist eine Kurzform von `page.header.title`.

1. Nachdem ein paar Standard-Meta-Tags gesetzt sind, gibt es eine Referenz zum Einbinden von `partials/metadata.html.twig` (Zeile 11). Diese Datei enthält einen Loop, der die Metadaten der Seite durchläuft. Sie ist (eigentlich) eine Zusammenführung von Metadaten aus `site.yaml` und allen seitenspezifischen Overrides.

1. Der Punkt `<link rel="icon"...` (Zeile 13) wird durch den Verweis auf ein spezifisches Bild des Themes gesetzt. In diesem Fall befindet es sich im Theme-Verzeichnis unter `images/logo.png`. Die Syntax hierfür lautet `{{ url('theme://images/logo.png') }}`.

1. Der Punkt `<link rel="canonical"...` (Zeile 14) legt eine kanonische URL für die Seite fest, die über `{{ page.url(true, true) }}` immer auf die volle URL der Seite umgesetzt wird.

1. Jetzt definieren wir den Block `stylesheets` (Zeile 17-21) und fügen mit dem [Asset Manager](/themes/asset-manager) mehrere Assets hinzu. Das erste lädt das Pure.css-Framework. Mit dem zweiten wird [FontAwesome](http://fontawesome.io/) geladen, um nützliche Symbole zur Verfügung zu stellen. Der letzte Eintrag verweist auf die Datei `custom.css` im Ordner `css/` des Themes. Hier sind einige nützliche Stile für den Anfang, Sie können aber hier noch weitere hinzufügen. Außerdem können Sie bei Bedarf weitere CSS-Datei-Einträge hinzufügen.

1. Der Aufruf `{{ assets.css()|raw }}}` (Zeile 28) ist der Trigger, der das Template veranlasst, alle CSS-Link-Tags zu rendern.

1. Der `javascripts` Block (Zeile 23-25), wie auch der `stylesheets` Block (Zeile 17-21) ist eine gute Stelle, um Ihre JavaScript-Dateien unterzubringen. In diesem Beispiel fügen wir nur die 'jquery'-Bibliothek hinzu, die bereits mit Grav gebundelt, sodass Sie dafür keinen Pfad angeben müssen.

1. Die Funktion `{{ assets.js()|raw }}` (Zeile 29) wird alle JavaScript-Tags rendern.

1. Das Tag `<body>` (Zeile 33) hat ein class-Attribut, das alles anzeigt, was Sie in der Variablen `body_classes` in der Frontmatter der Seite setzen.

1. Der Block `header` (Zeile 35-49) enthält ein paar Elemente, die den HTML-Header der Seite ausgeben.  Besonders zu beachten ist, dass das Logo mit dem  Argument `{{ base_url == '' ? '/' : base_url }}}` auf die `base_url` verlinkt ist. Damit wird verhindert, dass der Link nur aus `/` besteht, falls es kein Unterverzeichnis gibt.

1. In diesem Beispieltheme wird der Titel der Site mit `{{{ config.site.title }}}` (Zeile 40) als Logo ausgegeben. Sie könnten, wenn Sie wollen, diese Variable einfach durch einen `<img>` Tag zu einer Logo-Datei ersetzen.

1. Der Tag `<nav>` (Zeile 43+44) enthält aktuell einen Link zu der Datei `partials/navigation.html.twig`. Sie enthält die Anweisung alle **sichtbaren** Seiten in einer Schleife zu auszulesen und sie als Menü anzuzeigen.  Standardmäßig werden Dropdown-Menüs für verschachtelte Seiten unterstützt. Dies kann jedoch über die Konfiguration des Themes abgeschaltet werden. Schauen Sie sich diese Navigationsdatei an, um einen Eindruck davon zu bekommen, wie das Menü generiert wird.

1. Die Verwendung von `{% block content %}{% endblock %}` (Zeile 54) erstellt einen Platzhalter, der es erlaubt, Inhalte aus einem anderen Template darzustellen, das dieses hier erweitert. Vergessen Sie nicht, dass wir es aber in `default.html.twig` überschrieben haben, um den Inhalt der Seite auszugeben.

1. Der Block `footer` (Zeile 59-65) enthält einen einfachen Footer, den Sie leicht an Ihre Wünsche anpassen können.

1. Ähnlich wie der Inhaltsblock ist der Block `{% block bottom %}{% endblock %}` (Zeile 67-69) als Platzhalter für Templates gedacht, um benutzerdefinierte JavaScript-Initialisierung oder analytische Codes einzufügen. In diesem Beispiel geben wir jedes JavaScript aus, das zu der Asset-Gruppe `bottom` hinzugefügt wurde.  Lesen Sie mehr darüber in der [Asset Manager](/themes/asset-manager) Dokumentation.


## Stufe 6 – Theme-CSS

Vielleicht ist Ihnen aufgefallen, dass wir in der Datei `partials/base.html.twig` über den Asset Manager auf ein benutzerdefiniertes Theme-CSS verweisen: `do assets.add('theme://css/custom.css', 98)`. Diese Datei enthält sämtliches benutzerdefiniertes CSS, das wir benötigen, um die Lücken zu füllen, welches das Pure.css-Framework nicht vorsieht.  Da Pure ein sehr minimalistisches Framework ist, bietet es das Wesentliche, aber fast keine Gestaltung.

1. Sehen Sie sich in Ihrem Verzeichnis `user/themes/my-theme/css` die Datei `custom.css` an:

[prism classes="language-css line-numbers"]
/* Core Stuff */
* {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

body {
    font-size: 1rem;
    line-height: 1.7;
    color: #606d6e;
}

h1,
h2,
h3,
h4,
h5,
h6 {
    color: #454B4D;
}

a {
    color: #1F8CD6;
    text-decoration: none;
}

a:hover {
    color: #175E91;
}

pre {
    background: #F0F0F0;
    margin: 1rem 0;
    border-radius: 2px;
}

blockquote {
    border-left: 10px solid #eee;
    margin: 0;
    padding: 0 2rem;
}

/* Utility Classes */
.wrapper {
    margin: 0 3rem;
}

.padding {
    padding: 3rem 1rem;
}

.left {
    float: left;
}

.right {
    float: right
}

.text-center {
    text-align: center;
}

.text-right {
    text-align: right;
}

.text-left {
    text-align: left;
}

/* Content Styling */
.header .padding {
    padding: 1rem 0;
}

.header {
    background-color: #1F8DD6;
    color: #eee;
}

.header a {
    color: #fff;
}

.header .logo {
    font-size: 1.7rem;
    text-transform: uppercase;
}

.footer {
    background-color: #eee;
}

/* Menu Settings */
.main-nav ul {
    text-align: center;
    letter-spacing: -1em;
    margin: 0;
    padding: 0;
}

.main-nav ul li {
    display: inline-block;
    letter-spacing: normal;
}

.main-nav ul li a {
    position: relative;
    display: block;
    line-height: 45px;
    color: #fff;
    padding: 0 20px;
    white-space: nowrap;
}

.main-nav > ul > li > a {
    border-radius: 2px;
}

/*Active dropdown nav item */
.main-nav ul li:hover > a {
    background-color: #175E91;
}

/* Selected Dropdown nav item */
.main-nav ul li.selected > a {
    background-color: #fff;
    color: #175E91;
}

/* Dropdown CSS */
.main-nav ul li {position: relative;}

.main-nav ul li ul {
    position: absolute;
    background-color: #1F8DD6;
    min-width: 100%;
    text-align: left;
    z-index: 999;

    display: none;
}
.main-nav ul li ul li {
    display: block;
}

/* Dropdown CSS */
.main-nav ul li ul ul {
    left: 100%;
    top: 0;
}

/* Active on Hover */
.main-nav li:hover > ul {
    display: block;
}

/* Child Indicator */
.main-nav .has-children > a {
    padding-right: 30px;
}
.main-nav .has-children > a:after {
    font-family: FontAwesome;
    content: '\f107';
    position: absolute;
    display: inline-block;
    right: 8px;
    top: 0;
}

.main-nav .has-children .has-children > a:after {
    content: '\f105';
}

[/prism]

Das ist ziemlich gewöhnlicher CSS-Kram, der einige Basiswerte für Ränder, Schriftarten, Farben und Hilfsklassen festlegt. Für die Darstellung des Dropdown-Menüs sind einige einfache Inhalts-Stylings und teilweise umfangreichere Stylings erforderlich. Sie können diese Datei je nach Bedarf modifizieren oder sogar neue CSS-Dateien hinzufügen (passen Sie einfach auf, dass Sie eine Referenz im Block `head` einfügen, indem Sie dem Beispiel für `custom. css` folgen).

## Stufe 7 – Testen

Um Ihr Theme in Aktion zu sehen, öffnen Sie Ihren Browser und rufen Sie Ihre Grav-Website auf.  Sie sollten etwa folgendes sehen:

![](pure-theme.png?lightbox&resize=800,600)

Gratulation, Sie haben Ihr erstes Theme erstellt!
