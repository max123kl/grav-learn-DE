---
title: Blueprints
page-toc:
  active: true
taxonomy:
    category: docs
---

## Was ist ein Blueprint?

Blueprints sind ein wichtiger Bestandteil von Grav. Sie sind im Wesentlichen die Grundlage für die Interaktion eines Themes oder Plugins mit dem Grav-Administrator. Sie informieren Grav über die Eigenschaften eines Themes oder Plugins, seinen Namen, wo es auf GitHub zu finden ist, usw. Sie generieren auch die Konfigurationsoptionen für dieses Theme oder Plugin im Grav-Admin.

Ein Blueprint wird über eine YAML-Datei beschrieben und kann in der Regel sowohl Eigenschaften als auch Formularfelder enthalten.

Die große Mehrheit der Grav-Anwender wird nie mit Blueprints arbeiten müssen. Sie bestimmen kurz gesagt, wie Plugins und Themes im Backend der Website angezeigt werden. Für die meisten Benutzer ist das der Punkt, an dem sie ihre Themes und Plugins mit dem Grav-Admin konfigurieren oder Optionen innerhalb der primären YAML-Datei des Themes oder Plugins bearbeiten.

Die Leute, die am meisten mit Blueprints arbeiten werden, sind Entwickler, die neue Themes und Plugins erstellen und die Optionen einer Ressource im Backend anpassen wollen. Sie sind ein leistungsstarkes Werkzeug, das den Charakter Ihrer Ressource definiert, wo Grav Updates für sie finden kann und welche Konfigurationsoptionen Sie im Backend vornehmen können sollen.

## Blueprint-Typen

Grav verwendet Blueprints für:

- die Definition von Themes und Plugin-Informationen.
- die Definition von Theme/Plugin-Konfigurationsoptionen, die im Admin-Bereich angezeigt werden.
- die Definition der Pages-Formulare des Admin-Bereichs.
- die Definition, der im Konfigurationsbereich des Admins angezeigten Optionen.

Im Folgenden werden wir Ihnen zusätzliche Details über die Arbeitsweise von Blueprints in Grav näher beschreiben.

#### Themes und Plugins

Bei der Verwendung mit Themes und Plugins ist die Regel, eine Datei **blueprints.yaml** in das Paket einzufügen. Grav erhält auf diese Weise die Metadaten dieser Ressource, die sie dem Grav-Admin zugänglich macht.

Die Datei **blueprints.yaml** ist ein wichtiger Bestandteil jedes Themes und Plugins. Sie ist für den GPM (Grav Package Manager) unerlässlich. GPM verwendet die im Blueprint gespeicherten Informationen, um das Plugin den Anwendern zur Verfügung zu stellen.

In unserem [Beispiel-Plugin „Blueprint“](example-plugin-blueprint) gehen wir auf das Konzept des **Assets-Plugins** ein. Dieses Blueprint legt den Namen, die Autoreninformationen, Schlüsselwörter, die Homepage, den Link zum Bug-Report und andere Metadaten fest. Diese Metadaten teilen nicht nur Grav mit, wo Updates für das Plugin zu finden sind, sondern stellen eine nützliche Ressource für den Benutzer dar, die über den Grav Admin zugänglich ist.

Sobald diese Informationen vorliegen, finden Sie weiter unten auf der Seite des Blueprints Informationen zu den Formularen. Mit diesen Informationen werden die Admin-Formulare erstellt, die für den Backend-Benutzer von Grav verfügbar sind. Wenn Sie beispielsweise einen Schalter einfügen möchten, der eine bestimmte Funktion dieses Plugins aktiviert oder deaktiviert, würden Sie ihn hier anlegen.

![Admin Forms](blueprints_1.png?classes=shadow)

Die Datei **blueprints.yaml** arbeitet mit der nach dem Plugin benannten YAML-Datei (Beispiel: **assets.yaml**). Das Blueprint legt die konfigurierbaren Optionen fest und die selbstbenannte YAML-Datei der Ressource setzt deren Werte. Diese benannte YAML-Datei wird dann in die Sektion `user/config` der Grav-Instanz dupliziert, um diese Voreinstellungen entweder manuell oder durch den Grav-Admin zu überschreiben.

Im Wesentlichen definiert die Datei **blueprints.yaml** eine beliebige Option für ein Theme oder Plugin. Die genannte YAML-Datei für diese Ressource gibt Auskunft darüber, auf welchen Wert sie gesetzt ist.

#### Pages

Grav Pages können wirklich alles und jedes enthalten. Eine Seite kann beispielsweise eine Blog-Auflistung, ein Blog-Beitrag, eine Produktseite, eine Bildergalerie usw. sein.

Entscheidend dafür, was eine Seite darstellt und wie sie aussehen soll, das ist das **Page Blueprint**.

Grav enthält einige einfache Page Blueprints: Standard und Modular. Das sind die beiden Haupt-Bausteine von Grav.

Zusätzliche Seiten-Blueprints werden vom Theme hinzugefügt und eingerichtet. Das Theme kann entscheiden, so viele Seiten-Blueprints wie möglich hinzuzufügen oder sich auf bestimmte Seiten-Blueprints zu konzentrieren, die sich auf die zu erledigenden Aufgaben beschränken.

Ein Grav Theme ist viel flexibler und mächtiger als das, was Sie vielleicht aus anderen Plattformen gewöhnt sind.

Die Themes können so spezifisch angewandt werden. Zum Beispiel könnte sich ein Theme auf eines dieser Ziele spezialisieren:

- Aufbau einer Dokumentationsseite, wie die, die Sie gerade lesen
- Aufbau einer E-Commerce-Website
- Aufbau eines Blogs
- Aufbau einer Portfolio-Site

Ein Theme kann es seinen Benutzern auch ermöglichen, alle diese Funktionen umzusetzen. Normalerweise kann jedoch ein fein abgestimmtes, für einen einzigen Zweck erstelltes Theme eines dieser Ziele besser erfüllen als ein universelles Theme.

Eine Page-Datei wird von einer Seite verwendet, wenn der Name der Markdown-Datei festgelegt wird, z.B. `blog.md`, `default.md` oder `form.md`.

Jede dieser Dateien wird eine andere Seiten-Datei verwenden. Sie können den Dateityp auch mit Hilfe der [Header-Eigenschaft des Templates](../../content/headers#template) ändern.

Das von einer Seite verwendete Template bestimmt nicht nur das „Look and Feel“ im Frontend, sondern auch die Art und Weise, wie das Admin-Plugin die Seite rendert, indem es Ihnen erlaubt, Optionen hinzuzufügen, Boxen auszuwählen, benutzerdefinierte Eingaben zu machen und Schalter zu betätigen.

So wird es gemacht: Fügen Sie in Ihrem Theme einen Ordner `blueprints/` hinzu und erstellen dort eine YAML-Datei mit dem Namen des von Ihnen hinzugefügten Seiten-Templates. Beispielsweise können Sie ein Blog-Seiten-Template hinzufügen, indem Sie die Datei `blueprints/blog.yaml` anlegen. Ein [Beispiel für dieses Verzeichnis finden Sie im Theme **Antimatter**](https://github.com/getgrav/grav-theme-antimatter/tree/develop/blueprints).

## Komponenten eines Blueprints

Es gibt zwei Gruppen von Informationen, die in einer **blueprints.yaml** Datei angeboten werden. Die erste Gruppe von Metadaten-Informationen ist die Identität der Ressource selbst, die zweite Gruppe bezieht sich auf die Formulare. Alle diese Informationen werden in einer einzelnen **blueprints.yaml** Datei gespeichert, die im Stammverzeichnis eines jeden Plugins und Theme hinterlegt ist.

Hier folgt ein Beispiel für den Metadaten-Teil einer **blueprints.yaml** Datei:

```yaml
name: GitHub
slug: github
type: plugin
version: 1.0.1
description: "This plugin wraps the [GitHub v3 API](https://developer.github.com/v3/) and uses the [php-github-api](https://github.com/KnpLabs/php-github-api/) library to add a nice GitHub touch to your Grav pages."
icon: github
author:
  name: Team Grav
  email: devs@getgrav.org
  url: https://getgrav.org
homepage: https://github.com/getgrav/grav-plugin-github
keywords: github, plugin, api
bugs: https://github.com/getgrav/grav-plugin-github/issues
license: MIT
```

Wie man sehen kann, enthält dieser Bereich viele allgemeine, identifizierende Informationen über das Plugin, einschließlich seines Namens, seiner Versionsnummer, Beschreibung, Autoreninformationen, Lizenz, Schlüsselwörter und URLs, bei denen weitere Informationen gefunden oder Fehler gemeldet werden können. Sie können unten, im Screenshot aus dem Grav Admin, diesen Bereich in Aktion betrachten.

![Admin Forms](blueprints_2.png?classes=shadow)

Der nächste Abschnitt ist der Formularbereich, der sich nur ein paar Leerzeilen unterhalb der oben aufgeführten Daten befindet. Dieser Bereich des Blueprints generiert Formulare und Felder, die zur Konfiguration des Plugins aus dem Grav Admin heraus verwendet werden. Hier folgt ein kurzes Beispiel für diesen Bereich der Datei **blueprints.yaml**.

```yaml
form:
  validation: strict
  fields:
    enabled:
        type: toggle
        label: Plugin status
        highlight: 1
        default: 1
        options:
            1: Enabled
            0: Disabled
        validate:
            type: bool
```

Dieser Teil der Datei erstellt alle administrativen Optionen, auf die im Grav Admin zugegriffen werden kann. In diesem speziellen Fall haben wir einen einfachen **Plugin Status** Schalter erstellt, mit dem der Benutzer das Plugin vom Admin aus aktivieren oder deaktivieren kann (siehe Abbildung unten).

![Admin Forms](blueprints_3.png?classes=shadow)
