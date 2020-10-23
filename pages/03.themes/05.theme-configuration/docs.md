---
title: Theme Konfiguration
taxonomy:
    category: docs
---

In Grav können Sie leicht auf die Theme-Konfiguration und Blueprint-Informationen aus Ihren Twig- und PHP-Dateien zugreifen.

## Abrufen von Informationen zum Theme-Blueprint

Informationen aus der Datei `blueprints.yaml` des derzeit aktiven Themes können über das Objekt `theme` abgerufen werden. Lassen Sie uns die folgende `blueprints.yaml` Datei als Beispiel benutzen:

[prism classes="language-yaml line-numbers"]
name: Antimatter
slug: antimatter
type: theme
version: 1.7.0
description: "Antimatter is the default theme included with **Grav**"
icon: empire
author:
  name: Team Grav
  email: devs@getgrav.org
  url: https://getgrav.org
homepage: https://github.com/getgrav/grav-theme-antimatter
demo: http://demo.getgrav.org/blog-skeleton
keywords: antimatter, theme, core, modern, fast, responsive, html5, css3
bugs: https://github.com/getgrav/grav-theme-antimatter/issues
license: MIT
[/prism]

Sie können jedes dieser Elemente über `theme` erreichen, indem Sie die Standard- **Punkt-Syntax** verwenden:

[prism classes="language-twig line-numbers"]
Author Email: {{ theme.author.email }}
Theme License: {{ theme.license }}
[/prism]

Sie können dieselben Werte auch über ein Grav-Plugin mit PHP-Syntax abrufen:

[prism classes="language-php line-numbers"]
$theme_author_email = $this->grav['theme']['author']['email'];
$theme_license = $this->grav['theme']['license'];
[/prism]

## Zugriff auf die Theme-Konfiguration

Die Themes haben auch Konfigurationsdateien. Die Konfigurationsdatei eines Themes heißt `<themename>.yaml`. Die Standarddatei befindet sich im Stammverzeichnis des Themes (`user/themes/<themename>`).

Es wird **nachdrücklich** empfohlen, die Standard-YAML-Datei des Themes nicht zu ändern, sondern die Einstellungen im Ordner `user/config/themes` zu modifizieren. Auf diese Weise wird erreicht, dass die ursprünglichen Einstellungen des Themes erhalten bleiben, so dass Sie schnell auf die Änderungen zugreifen und/oder bei Problemen wieder zurückkehren können.

Betrachten wir zum Beispiel das Theme Antimatter. Standardmäßig gibt es eine Datei namens `antimatter.yaml` im Stammordner des Themes. Der Inhalt dieser Konfigurationsdatei sieht wie folgt aus:

[prism classes="language-yaml line-numbers"]
enabled: true
color: blue
[/prism]

Das ist eine einfache Datei, aber sie gibt Ihnen eine Vorstellung davon, was Sie mit den Einstellungen der Theme-Konfiguration tun können. Lassen Sie uns diese Einstellungen übersteuern und eine neue hinzufügen.

Erstellen Sie also eine Datei an der folgenden Stelle: `user/config/themes/antimatter.yaml`.  In diese Datei fügen Sie den folgenden Inhalt ein:

> *Ich weise darauf hin, dass hier `enabled` nicht wiederholt wird. Wenn die Konfigurationsdateien gemergt und nicht einfach ersetzt werden, dann sollte das explizit angegeben werden.*

[prism classes="language-yaml line-numbers"]
color: red
info: Grav ist großartig!
[/prism]

In Ihren Theme-Templates können Sie dann über das Objekt `grav.theme.config` auf diese Variablen zugreifen:

```
<h1 style="color:{{ grav.theme.config.color|e }}">{{ grav.theme.config.info|e }}</h1>
```

Das sollte folgendermaßen gerendert werden:

<h1 style="color:red">Grav ist großartig!</h1>

In PHP können Sie auf die Konfiguration des aktuellen Themes zugreifen mit:

[prism classes="language-php line-numbers"]
$color = $this->grav['theme']->config()['color'];
$info = $this->grav['theme']->config()['info'];
[/prism]

Ein Kinderspiel! Ihrer Fantasie sind hier fast keine Grenzen gesetzt.  Sie können die Themes nach Lust und Laune gestalten! :)

### Alternative Schreibweisen

Die folgenden alternativen Ausdrücke funktionieren ebenfalls:

[prism classes="language-twig line-numbers"]
Theme Color Option: {{ config.theme.color_option|e }}
   or
Theme Color Option: {{ theme_var(color_option)|e }}
   or
Theme Color Option: {{ grav.themes.antimatter.color_option|e }} [AVOID!]
[/prism]

**Auch wenn `grav.themes.<themename>` unterstützt wird, sollte dies jedoch vermieden werden, da es eine korrekte Vererbung des Themas unmöglich macht.**
