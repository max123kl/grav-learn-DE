---
title: 'Beispiel-Plugin: Blueprint'
taxonomy:
    category: docs
---

Das Blueprint eines Plugins gibt Grav einen Überblick darüber, was das Plugin ausmacht, seine Quelle, Support- und Autoreninformationen, Abhängigkeiten und Formularfelder, die zur Verwaltung des Plugins im Grav Admin verwendet werden.

As an example, here's the Blueprint for a plugin:

[prism classes="language-yaml line-numbers"]
name: Assets
slug: assets
type: plugin
version: 1.0.4
description: "This plugin provides a convenient way to add CSS and JS assets directly from your pages."
icon: list-alt
author:
  name: Team Grav
  email: devs@getgrav.org
  url: https://getgrav.org
homepage: https://github.com/getgrav/grav-plugin-assets
demo: https://learn.getgrav.org
keywords: assets, javascript, css, inline
bugs: https://github.com/getgrav/grav-plugin-assets/issues
license: MIT

dependencies:
  - { name: afterburner2 }
  - { name: github }
  - { name: email, version: '~2.0' }
[/prism]

Es gibt verschiedene Eigenschaften, die Sie verwenden können, um Ihrer Ressource eine Identität zu geben. Einige sind **erforderlich\***, andere sind _optional_.

[div class="table"]
| Eigenschaft      | Beschreibung         |
| :-----           | :-----               |
| __name*__        | Der Name der Ressource – vermeiden Sie das Anhängen von Plugin oder Theme, das ist nicht nötig  |
| __slug*__        | Die eindeutige Bezeichnung für die Ressource – sie wird auch verwendet, um den Namen des Ordners zu bestimmen, in dem die Ressource gespeichert ist, z.B. `user/plugins/__slug__` |
| __type*__        | Der Typ der Ressource – sie sollte entweder `plugin` oder `theme` sein |
| __version*__     | Die Version der Ressource – dieser Wert sollte sich bei jedem Release immer inkrementell ändern. Man sollte auch dem Standard [Sem-Ver](https://semver.org/lang/de/) folgen |
| __description*__ | Die Beschreibung Ihrer Ressource. Beachten Sie bitte, dass **200** Zeichen nicht überschritten werden dürfen. Eine Beschreibung sollte kurz und direkt auf den Punkt gebracht sein. Sie können bei Bedarf eine Markdown-Syntax verwenden. Es empfiehlt sich auch, Ihre Beschreibung in Gänsefüßchen zu setzen. |
| __icon*__        | Das Icon wird auf [getgrav.org](https://getgrav.org) verwendet. Zu diesem Zeitpunkt verwenden wir die [FontAwesome](http://fortawesome.github.io/Font-Awesome/icons/) Icon-Bibliothek. Wenn Sie also ein neues Plugin oder Thema entwickeln, sollte es Ihre Pflicht sein, zu überprüfen, ob das von Ihnen gewählte Icon nicht bereits verwendet wird. Andernfalls müssen wir es für Sie austauschen. |
| _screenshot_     | _(optional)_ Der Screenshot wird immer nur für _Themes_ ausgewertet und für _Plugins_ komplett ignoriert. Bei _Themes_ wäre dies der Dateiname des mit dem Theme gelieferten Screenshots (Standard: `screenshot.jpg`). Wenn Sie ein _Screenshot.jpg_-Bild im Stammverzeichnis Ihres Themes haben, können Sie die Benutzung dieser Option umgehen. Unser Repository wird es automatisch übernehmen. |
| __author.name*__ | Vollständiger Name des Entwicklers  |
| _author.email_   | _(optional)_ Die E-Mail-Adresse des Entwicklers |
| _author.url_     | _(optional)_ Die Homepage des Entwicklers |
| _homepage_       | _(optional)_ Wenn Sie eine eigene Homepage für Ihre Ressource haben, wäre dies der richtige Ort dafür |
| _docs_           | _(optional)_ Falls Sie über eine schriftliche Dokumentation für Ihre Ressource verfügen, können Sie diese hier verlinken |
| _demo_           | _(optional)_ Wenn Sie eine Demo-Version Ihrer Ressource erstellt haben, können Sie diese hier verlinken |
| _guide_          | _(optional)_ Wenn Sie Tutorials oder andere Anleitungen für Ihre Ressource haben, können Sie diese hier verlinken  |
| _keywords_       | _(optional)_ Obwohl es noch keine wirkliche Verwendung von Schlüsselwörtern gibt, können Sie hier Schlüsselwörter, durch Kommas getrennt, mit Bezug auf Ihre Ressource angeben  |
| _bugs_           | _(optional)_ Die URL, unter der Bugs gemeldet werden können, normalerweise ist dies der Link zu den [GitHub-Issues](https://guides.github.com/features/issues/)  |
| _license_        | _(optional)_ Die Art der Lizenz Ihrer Ressource (MIT, GPL, usw.). Es wird empfohlen, Ihrer Ressource immer eine `LICENSE` Datei mitzugeben   |
| _dependencies_   | _(optional)_ Eine Liste der Abhängigkeiten, die das Plugin/Theme benötigt. Wenn jedoch eine optionale GIT-Repository-URL angegeben wird, ist die Installation direkt aus dem Repository ebenfalls eine Option. Auch wenn Sie ein Array verwenden, können Sie einen Namen und eine Version explizit über [Paketversionen im Composer-Stil](https://getcomposer.org/doc/articles/versions.md) definieren |
| _gpm_            | _(optional)_ Aktualisierungen über den GPM erhalten. Falls auf `false` gesetzt, werden GPM-Updates für Nicht-GPM-Ressourcen deaktiviert. |
[/div][div class="table"]

Hier folgt ein Beispiel für den Identitätsteil der [GitHub-Plugin](https://github.com/getgrav/grav-plugin-github) Blueprints:

[prism classes="language-yaml line-numbers"]
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
[/prism]

Themes-Blueprints arbeiten auf die gleiche Weise wie Plugins.
