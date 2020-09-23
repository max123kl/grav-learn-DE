---
title: Twig-Tags
page-toc:
  active: true
  start: 3

process:
    twig: false
taxonomy:
    category: docs
---

Grav bietet auch eine große Auswahl an individuellen Twig-Tags, welche die bereits vorhandenen Fähigkeiten zur Erstellung von Twig-Templates um einige neue nützliche Tags erweitern.

### Markdown

Das Markdown-Tag bietet eine effiziente neue Methode zur Einbettung von Markdown in das Twig-Template.  Sie könnten eine Variable einsetzen und diese Variable mit dem Filter `|markdown` abbilden, aber die Syntax `{% markdown %}` macht das Erstellen von Markdown-Textblöcken noch einfacher.

[prism classes="language-twig line-numbers"]
{% markdown %}
This is **bold** and this _underlined_

1. This is a bullet list
2. This is another item in that same list
{% endmarkdown %}
[/prism]

### Scripts

Der Scripts-Tag ist praktisch gesehen ein Komfort-Tag, der Ihr Twig im Vergleich zum üblichen Ansatz `{% do assets...%}` besser lesbar erhält.  Er ist lediglich eine alternative Art, um Inhalte zu schreiben.

#### File

[prism classes="language-twig line-numbers"]
{% script 'theme://js/something.js' in 'bottom' priority: 20 with { defer: true, async: true } %}
[/prism]

#### Inline

[prism classes="language-twig line-numbers"]
{% script in 'bottom' priority: 20 %}
    alert('Warning!');
{% endscript %}
[/prism]

### CSS-Styles

#### File

[prism classes="language-twig line-numbers"]
{% style 'theme://css/foo.css' priority: 20 %}
[/prism]

#### Inline

[prism classes="language-twig line-numbers"]
{% style priority: 20 with { media: 'screen' } %}
    a { color: red; }
{% endstyle %}
[/prism]

### Switch

In den meisten Programmiersprachen ist die Verwendung einer `switch`-Anweisung der übliche Weg, um eine Reihe von `is else`-Anweisungen sauberer und lesbarer zu machen.  Sie könnten sich auch als geringfügig schneller erweisen.  Wir stellen lediglich eine einfache Möglichkeit zur Verfügung, um diese zu erstellen, da sie in der Basisfunktionalität von Twig fehlten.

[prism classes="language-twig line-numbers"]
{% switch type %}
  {% case 'foo' %}
     {{ my_data.foo }}
  {% case 'bar' %}
     {{ my_data.bar }}
  {% default %}
     {{ my_data.default }}
{% endswitch %}
[/prism]

### Deferred Blocks

Eine große Neuerung in Grav 1.6 ist die Funktion **deferred blocks** (verzögerte Blöcke).  Bei herkömmlichen Blöcken kann ein einmal gerenderter Block nicht mehr manipuliert werden.  Nehmen wir das Beispiel eines `{% block scripts %}`, das einige Einträge für JavaScript-Includes enthalten könnte. Wenn Sie eine untergeordnete Twig-Vorlage haben und Sie eine Basisvorlage erweitern, in der dieser Block definiert ist, können Sie den Block erweitern und Ihre eigenen JavaScript-Einträge hinzufügen.  Partielle Twig-Templates, die von dieser Seite aus eingebunden werden, können den Block jedoch nicht erreichen oder mit ihm interagieren.

Das verzögerte Attribut des Blocks, das durch die [Deferred Extension](https://github.com/rybakit/twig-deferred-extension) (verzögerte Erweiterung) aktiviert wird, bedeutet, dass Sie diesen Block in jedem Twig-Template definieren können, aber sein Rendern wird verzögert, so dass er nach allem anderen gerendert wird. Das bedeutet, dass Sie JavaScript-Referenzen über den Aufruf `{% do assets.addJs() %}` von überall in Ihrer Seite aus hinzufügen können, und da das Rendering verzögert ist, enthält die Ausgabe alle Assets, die Grav kennt, unabhängig davon, wann Sie sie hinzugefügt haben.

[prism classes="language-twig line-numbers"]
{% block myblock deferred %}
    This will be rendered after everything else.
{% endblock %}
[/prism]

[version=16]
### Eine Exception auslösen

Es gibt bestimmte Situationen, in denen Sie eine Exception manuell auslösen müssen, daher haben wir auch dafür ein Tag.

[prism classes="language-twig line-numbers"]
{% throw 404 'Not Found' %}
[/prism]

### Try / Catch Exceptions

Außerdem ist es nützlich, eine leistungsfähigere Fehlerbehandlung im PHP-Stil in Ihren Twig-Templates vorzusehen, daher haben wir den neuen Tag `try/catch` eingebaut.

[prism classes="language-twig line-numbers"]
{% try %}
   <li>{{ user.get('name') }}</li>
{% catch %}
   User Error: {{ e.message }}
{% endcatch %}
[/prism]

### Render Object (nur Flex)

Flex-Objekte finden langsam ihren Weg in immer mehr Elemente von Grav.  Dabei handelt es sich um eigenständige Objekte, denen eine Twig-Template-Struktur zugeordnet ist, so dass sie sich selbst zu rendern wissen.  Um diese zu verwenden, haben wir einen neuen Tag `render` implementiert, der ein optionales Layout verwendet, das wiederum steuert, mit welchem der Template-Layouts das Objekt gerendert werden soll.

[prism classes="language-twig line-numbers"]
{% render object layout: 'default' with { variable: 'value' } %}
[/prism]
[/version]
