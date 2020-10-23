---
title: Twig Primer
taxonomy:
    category: docs
---

Twig ist eine einfache, effiziente Template-Engine für PHP. Sie ist von vornherein auf die Erstellung von Templates ausgelegt. Das erleichtert sowohl dem Entwickler als auch dem Designer die Arbeit.

Dank seiner leicht verständlichen Syntax und seiner unkomplizierten Prozeduren eignet es sich für jeden, der mit Smarty, Django, Jinja, Liquid oder Stencil vertraut ist.

Wir verwenden Twig für unsere Grav-Templates unter anderem wegen seiner Flexibilität und der enthaltenen Sicherheitsfunktionen. Die Tatsache, dass es gleichzeitig eine der schnellsten Template-Engines für PHP ist, machte die Entscheidung für den Einsatz in Grav zu einem Kinderspiel.

Twig kompiliert Templates bis hinunter zu purem PHP. Dadurch wird der PHP-Overhead auf ein Minimum reduziert, was zu einer noch schnelleren und effektiveren Arbeitsweise führt.

Dank seines **Lexers** und **Parsers** ist es auch eine sehr flexible Engine. Der Entwickler kann somit seine eigenen benutzerdefinierten Tags und Filter erstellen. Darüber hinaus ermöglicht es ihm, seine eigene [domänenspezifische Sprache](https://de.wikipedia.org/wiki/Dom%C3%A4nenspezifische_Sprache) (DSL) zu entwickeln.

Wenn es um die Sicherheit geht, macht Twig keine Abstriche. Es bietet dem Entwickler einen Sandbox-Modus, der ihm ermöglicht, jeden nicht vertrauenswürdigen Code zu untersuchen. So können Sie Twig als Template-Sprache für Applikationen verwenden und gleichzeitig den Benutzern die Möglichkeit geben, das Template-Design zu modifizieren.

Im Prinzip ist es ein leistungsstarker Motor, der Ihnen die Kontrolle über die Benutzeroberfläche gibt. In Kombination mit YAML zur Konfiguration ergibt sich ein leistungsfähiges und einfaches System, mit dem jeder Entwickler oder Site-Manager arbeiten kann.

## Wie funktioniert Twig?

Twig arbeitet, indem es auf den ganzen Hokuspokus aus dem Vorlagen-Design verzichtet. Templates sind hier nur Textdateien, die *Variablen* oder *Ausdrücke* enthalten, die bei der Ausführung des Templates durch Werte ersetzt werden.

*Tags* sind auch ein entscheidender Teil einer Template-Datei, da sie die Funktionalität des Templates selbst steuern.

Twig hat zwei wesentliche sprachliche Einschränkungen.

* `{{ }}` gibt das Resultat einer Auswertung eines Ausdrucks aus
* `{% %}` führt Anweisungen aus

Hier ist ein Basis-Template, das mit Twig erstellt wurde:

[prism classes="language-html line-numbers"]
<!DOCTYPE html>
<html>
    <head>
        <title>All About Cookies</title>
    </head>
    <body>
        My name is {{ name|e }} and I love cookies.
        My favorite flavors of cookies are:
        <ul>
        {% for cookie in cookies %}
            <li>{{ cookie.flavor|e }}</li>
		{% endfor %}
        </ul>
        <h1>Cookies are the best!</h1>
    </body>
</html>
[/prism]

In diesem Beispiel richten wir den Titel der Website so ein, wie Sie es von jeder Standard-Webseite gewohnt sind. Der Unterschied besteht darin, dass wir die einfache Twig-Syntax verwenden konnten, um den Namen des Autors zu präsentieren und eine dynamische Liste von Elementtypen zu erstellen.

Ein Template wird zuerst geladen, dann durch den **Lexer** geleitet, wo sein Quellcode in Tokens umgewandelt und in kleine Stücke zerlegt wird. An diesem Punkt übernimmt der **Parser** die Tokens und wandelt sie in den abstrakten Syntaxbaum um.

Sobald dies geschehen ist, konvertiert der Compiler den Quellcode in PHP-Code, der dann ausgewertet und dem User angezeigt werden kann.

Twig kann auch erweitert werden, um zusätzliche Tags, Filter, Tests, Operatoren, globale Variablen und Funktionen hinzuzufügen. Nähere Informationen zur Erweiterung von Twig finden Sie in der [offiziellen Dokumentation](http://twig.sensiolabs.org/doc/advanced.html).

## Twig Syntax

Ein Twig-Template hat mehrere Schlüsselkomponenten, die analysieren was Sie gerne tun möchten. Dazu gehören Tags, Filter, Funktionen und Variablen.

Werfen wir einen genaueren Blick auf diese wichtigen Hilfsmittel und wie sie Ihnen helfen können, ein erstaunliches Template zu gestalten.

### Tags

Tags weisen Twig an, was es zu machen hat. Damit können Sie festlegen, welchen Code Twig verarbeiten soll und welchen Code es bei der Evaluierung ignorieren soll.

Es gibt diverse unterschiedliche Typen von Tags und jeder hat seine eigene spezifische Syntax, die sie voneinander unterscheidet.

#### Kommentar-Tags

Kommentar-Tags (`{# hier Kommentar einfügen #}`) werden verwendet, um Kommentare zu setzen, die in der Twig Template-Datei enthalten sind, aber vom Enduser nicht sichtbar sind. Sie werden während der Evaluierung entfernt und weder geparst noch ausgegeben.

Eine sinnvolle Anwendung dieser Tags ist die Beschreibung einer bestimmten Codezeile oder eines Befehls. So kann ein anderer Entwickler oder Designer in Ihrem Team diese Tags schnell lesen und verstehen.

Hier ist ein Beispiel für einen Kommentar-Tag, wie Sie ihn in einer Twig Template-Datei vorfinden könnten:

[prism classes="language-twig"]
{# Schokokekse sind großartig! Nicht weitersagen! #}
[/prism]

#### Output-Tags

Output-Tags (`{{ hier Output einfügen }}`) werden analysiert und dem generierten Output hinzugefügt. Hier würden Sie alles einfügen, was Sie auf dem Frontend oder in anderen generierten Inhalten erscheinen lassen wollen.

Hier folgt ein Beispiel für die Verwendung von Ausgabe-Tags in einem Twig-Template:

[prism classes="language-twig"]
Mein Name ist {{ name|e }} und ich liebe Cookies.
[/prism]

Die Variable `name` wurde in diese Zeile eingefügt und erscheint dem Endbenutzer als `Mein Name ist Jake und ich liebe Cookies.`, denn der Wert der Variable „name“ war `Jake`.

!! Es ist sehr wichtig, entweder die Einstellung `autoescape` in Ihrer [Systemkonfiguration](/basics/grav-configuration#twig) zu aktivieren oder daran zu denken, jede einzelne Variable in Template-Dateien mit Hilfe des `|e` Filters zu escapen, um Ihre Site gegen XSS-Angriffe sicher zu machen. Für einen gesicherten HTML-Inhalt sollten Sie den `|raw`-Filter einsetzen.

#### Action-Tags

Action Tags sind die Leistungsträger in der Twig-Welt. Im Gegensatz zu den anderen Tags, die entweder etwas weitergeben oder untätig im Quellcode sitzen und darauf warten, dass ein Designer sie liest, tun diese Tags tatsächlich etwas.

Aktions-Tags setzen Variablen, Schleifen durch Arrays und Testkonditionen. Ihre `for` und `if` Statements werden mit Hilfe dieser Tags gemacht.

So könnte ein Action-Tag in einer Twig-Vorlage aussehen:

[prism classes="language-twig line-numbers"]
{% set hour = now | date("G") %}
{% if hour >= 9 and hour < 17 %}
    <p>Zeit für Cookies!</p>
{% else %}
    <p>Zeit, mehr Kekse zu backen!</p>
{% endif %}
[/prism]

Das initiale Action-Tag setzt die Stunde als die aktuelle Stunde in einer 24-Stunden-Uhr. Dieser Wert wird dann dazu verwendet, um festzustellen, ob sie zwischen 9 und 17 Uhr liegt. Wenn ja, dann wird `Zeit für Cookies!` angezeigt. Ist dies nicht der Fall, wird stattdessen `Zeit, mehr Kekse zu backen!` angezeigt.

Es ist sehr wichtig, dass sich die Tags nicht überlappen. Es ist nicht möglich, einen Output-Tag innerhalb eines Action-Tags zu platzieren oder umgekehrt.

### Filter

Filter sind nützlich, vor allem wenn Sie die Output-Tags verwenden, um Daten anzuzeigen, die möglicherweise nicht so formatiert sind, wie Sie es wünschen.

Nehmen wir an, der Wert der Variablen `name` würde unerwünschte SGML/XML-Tags enthalten. Sie könnten diese mit Hilfe des folgenden Codes herausfiltern:

[prism classes="language-twig"]
{{ name|striptags|e }}
[/prism]

### Funktionen

Funktionen können Inhalte generieren. Normalerweise schließen sich Argumente an, die in Klammern direkt nach dem Funktionsaufruf stehen. Selbst wenn kein Argument vorhanden ist, wird die Funktion immer noch eine Klammer `()` aufweisen, die direkt nach dem Funktionsaufruf steht.

[prism classes="language-twig line-numbers"]
{% if date(cookie.created_at) < date('-2days') %}
    {# Cookies essen! #}
{% endif %}
[/prism]

## weitere Quellen

* [Offizielle Twig Dokumentation](http://twig.sensiolabs.org/documentation)
* [Twig for Template Designers](http://twig.sensiolabs.org/doc/templates.html)
* [Twig for Developers](http://twig.sensiolabs.org/doc/api.html)
* [6 Minute Video Introduction to Twig](http://www.dev-metal.com/6min-video-introduction-twig-php-templating-engine/)
* [Introduction to Twig](http://www.slideshare.net/markstory/introduction-to-twig)
* [Twig: The Basics (kostenlose Einführung zu einem kostenpflichtigen Kurs)](https://knpuniversity.com/screencast/twig/basics)
