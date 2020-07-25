---
title: Theme-Updates
taxonomy:
    category: docs
---

Den Original-Text finden Sie [hier](https://getgrav.org/blog/important-theme-updates).

## Wichtige Theme-Updates

Kleine Änderung, um Grav noch besser zu machen

!! TLDR - Beginnend mit Grav 1.5.10 und Grav 1.6.0 ist die Erweiterung Deferred Twig verfügbar und eine Aktualisierung Ihres Themas wird dringend empfohlen, um sicherzustellen, dass Sie in Zukunft keine Probleme mit Plugins haben werden.

Was als reines Grav 1.6-Feature begann, wurde auf Grav 1.5.10 zurückportiert, da es immer wichtiger wird, langfristige Probleme mit Plugins zu lösen, die ihre eigenen CSS- und JS-Assets haben. Wir haben Unterstützung für die [Twig Deferred Extension](https://github.com/rybakit/twig-deferred-extension) in Grav aufgenommen, die ein ziemlich ärgerliches Problem in Twig löst, bei dem Inhalte in der Reihenfolge ihres Auftretens gerendert werden und einmal gerendert, nicht mehr manipuliert werden können. Das beste Beispiel für diese Einschränkung betrifft die CSS- und JS-Assets, die dem Grav-Asset-Manager hinzugefügt und im Allgemeinen im `<head></head>` HTML-Block gerendert werden. Wenn eine enthaltene Twig-Vorlage, eine modulare Seite oder sogar ein Plugin verarbeitet wird, nachdem dieser Block gerendert wurde, haben sie keine Möglichkeit, ihre eigenen Objekte zu dem Block hinzuzufügen.

Die bisherige Lösung bestand darin, sicherzustellen, dass alle Objekte vor dem Rendern von Twig hinzugefügt wurden, aber dies ist oft keine praktikable Lösung. Ein Beispiel für dieses Problem sind Formularfelder, die spezielle CSS- oder JS-Anforderungen haben und erst dann versucht werden, diese Assets hinzuzufügen, wenn das Feld gerendert wird, lange nachdem das Rendern der Assets im Tag `<head></head>` abgeschlossen ist. Wir haben uns darum gekümmert, indem wir empfohlen haben, einen neuen `bottom` Asset-Block am Ende des Themes hinzuzufügen, damit diese Assets zumindest zuletzt gerendert werden können.

### Änderungen an der Basis-Twig-Vorlage

Im Hinblick auf die Handhabung von Assets wurde in der Regel ein typisches Theme in dieser Rohfassung aufgebaut, wobei die `<head></head>` Assets in jedem der Asset-Blöcke von `partials/base.html.twig` dargestellt werden:

[prism classes="language-twig line-numbers"]
<!DOCTYPE html>
<html>
    <head>
    ...
    {% block stylesheets %}
        {% do assets.addCss('theme://css/site.css') %}
        {{ assets.css()|raw }}
    {% endblock %}

    {% block javascripts %}
        {% do assets.addJs('theme://js/site.js', {group:'bottom'}) %}
        {{ assets.js()|raw }}
    {% endblock %}
    ...
    </head>
    <body>
    ...
    {% block bottom %}
        {{ assets.js('bottom')|raw }}
    {% endblock %}
    </body>
</html>
[/prism]

Dank der Möglichkeit der **Deferred-Block-Extention** können wir, mit nur einer geringfügigen Änderung, diese Situation dramatisch verbessern. Wir verschieben einfach das Rendering sowohl für CSS als auch für JS aus ihren jeweiligen Blöcken in einen neuen Block mit `assets deferred` darunter.

[prism classes="language-twig line-numbers"]
<!DOCTYPE html>
<html>
    <head>
    ...
    {% block stylesheets %}
        {% do assets.addCss('theme://css/site.css') %}
    {% endblock %}

    {% block javascripts %}
        {% do assets.addJs('theme://js/site.js', {group:'bottom'}) %}
    {% endblock %}

    {% block assets deferred %}
        {{ assets.css()|raw }}
        {{ assets.js()|raw }}
    {% endblock %}
    ...
    </head>
    <body>
    ...
    {% block bottom %}
        {{ assets.js('bottom')|raw }}
    {% endblock %}
    </body>
</html>
[/prism]

Diese einfache Änderung ermöglicht es uns, CSS und JS von überall her hinzuzufügen, ohne die Verwendung des unteren JS-Renderblocks erzwingen zu müssen. Das dient natürlich immer noch einem Zweck, da man oft wirklich möchte, dass das JS am Ende der Seite gerendert wird, aber CSS soll innerhalb des `<head></head>` referenziert werden und bis zu diesem Zeitpunkt war das einfach nicht möglich.

! **Hinweis:** Der Twig-Block {% block bottom %}...{% endblock %} ist auch sehr wichtig für Plugins wie Form, die auf JavaScript angewiesen sind und unten auf der Seite ausgegeben werden. Bitte stellen Sie sicher, dass alle Ihre Themes diese einheitliche Struktur haben.

### Abhängigkeit der Blueprints von der Grav-Version

Wenn Sie Ihr Theme über GPM zur Verfügung stellen, (und wegen der Abwärtskompatibilität) empfiehlt es sich außerdem, in der Datei `blueprints.yaml` Ihres Themes eine Grav-Version von mindestens `1.5.10+` festzulegen:

[prism classes="language-twig line-numbers"]
...
keywords: quark, spectre, theme, core, modern, fast, responsive, html5, css3
bugs: https://github.com/getgrav/grav-theme-quark/issues
license: MIT

dependencies:
  - { name: grav, version: '>=1.5.10' }
[/prism]

Ein vollständiges Beispiel für diese Änderung ist in diesem [Quark-Theme-Commit](https://github.com/getgrav/grav-theme-quark/commit/e1ca4f24b16bc75b344c65d51d641f31227833f8) zu sehen.

Diese Änderungen sind wichtig für Plugins von Drittanbietern und werden für die gesamte Grav-Entwicklung in Zukunft nützlich sein, daher wird **dringend empfohlen**, Ihre Themes zu aktualisieren, um diese Funktionalität nutzen zu können. Wir werden alle Grav-, RocketTheme- und Trilby-Kernthemen sehr bald mit dieser Änderung nachrüsten.

!!! **Tipp:** Es gibt auch noch andere Vorteile bei der Verwendung des `deferred` Verfahrens für andere Twig-Blöcke, da diese auf dieselbe Weise funktionieren wie das bereits vorgestellte Beispiel für Assets. Das Rendern dieser Blöcke wird bis zum Ende des Rendervorgangs verschoben, so dass Sie sie überschreiben oder manipulieren können, als ob sie traditionell gerendert worden wären.
