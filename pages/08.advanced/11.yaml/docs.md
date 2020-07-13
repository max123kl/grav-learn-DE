---
title: YAML Syntax
page-toc:
  active: true
taxonomy:
    category: docs
---

Einführung
-----

YAML steht für _"YAML Ain't Markup Language"_ und wird in Grav für seine Konfigurationsdateien, Blueprints und auch in den Seiteneinstellungen ausgiebig genutzt.

Mit YAML wird konfiguriert, welcher Markdown zum Auszeichnen verwendet werden soll. Es ist im Grunde ein vom Menschen lesbares, strukturiertes Datenformat. Es ist weniger komplex und unhandlicher als XML oder JSON, bietet aber ähnliche Funktionalitäten. Es ermöglicht im Wesentlichen die Einrichtung mächtiger Konfigurationseinstellungen, ohne dass Sie einen komplexeren Code-Typ wie CSS, JavaScript oder PHP erlernen müssen.

YAML ist von Grund auf so aufgebaut, dass es einfach zu bedienen ist. Im Kern wird eine YAML-Datei zur Beschreibung von Daten verwendet. Einer der Vorteile von YAML besteht darin, dass die Informationen in einer einzigen YAML-Datei leicht in mehrere Sprachtypen übersetzt werden können.

Im Wesentlichen werden die Daten, die Sie in eine YAML-Datei eingeben, in Verbindung mit einer Bibliothek verwendet, um die Seiten zu erstellen, die Sie in Grav sehen.

Grundlegende YAML-Regeln
-----

Es gibt einige Regeln, die in YAML festgelegt sind, um Probleme im Zusammenhang mit Mehrdeutigkeiten im Hinblick auf unterschiedliche Programmiersprachen und Editorprogramme zu vermeiden. Diese Regeln ermöglichen es, eine einzelne YAML-Datei konsistent zu interpretieren, unabhängig davon, mit welcher Anwendung und/oder Bibliothek sie ausgewertet wird.

* YAML-Dateien sollten, wann immer möglich, in Grav auf `.yaml` enden.
* In YAML wird zwischen Groß- und Kleinschreibung unterschieden.
* YAML lässt die Verwendung von Tabulatoren nicht zu. Stattdessen werden Leerzeichen verwendet, da Tabulatoren nicht universell unterstützt werden.

Basisdaten-Typen
-----

YAML eignet sich hervorragend für die Arbeit mit **mappings** (Hashes / Wörterbücher), **sequences** (Arrays / Listen) und **scalars** (Zeichenketten / Zahlen). Während es mit den meisten Programmiersprachen verwendet werden kann, funktioniert es am besten mit Sprachen, die um diese Datenstrukturtypen herum aufgebaut sind. Dazu gehören: PHP, Python, Perl, JavaScript und Ruby.

## Scalars

Scalars sind ein ziemlich einfaches Konzept. Das sind die Zeichenfolgen und Zahlen, aus denen sich die Daten auf der Seite zusammensetzen. Ein Skalar könnte eine boolesche Eigenschaft wie `Yes`, eine Ganzzahl (Zahl) wie `5` oder eine Zeichenkette, wie ein Satz oder der Titel Ihrer Website, sein.

Scalars werden in der Programmierung oft als Variablen bezeichnet. Wenn Sie eine Liste von Tierarten erstellen würden, wären dies die Namen, die diesen Tieren gegeben werden.

Die meisten Scalars werden nicht in Anführungszeichen gesetzt, aber wenn Sie eine Zeichenkette eingeben, die Interpunktion und andere Elemente verwendet, die mit der YAML-Syntax verwechselt werden könnten (Bindestriche, Doppelpunkte usw.), sollten Sie diese Daten mit einfachen `'` oder doppelten `"` Anführungszeichen umschließen. Doppelte Anführungszeichen ermöglichen Ihnen die Verwendung von Escapezeichen zur Darstellung von ASCII- und Unicode-Zeichen.

[prism classes="language-yaml line-numbers"]
integer: 25
string: "25"
float: 25.0
boolean: Yes
[/prism]

## Sequences

Hier sehen Sie eine einfache Sequenz, die Sie in Grav finden könnten. Es handelt sich um eine einfache Liste, bei der jeder Punkt in der Liste in einer eigenen Zeile mit einem einleitenden Bindestrich steht.

[prism classes="language-yaml line-numbers"]
- Cat
- Dog
- Goldfish
[/prism]

Durch diese Reihenfolge wird jedes Element in der Liste auf die gleiche Ebene gestellt. Wenn Sie eine verschachtelte Folge mit Positionen und Unterpositionen erstellen möchten, können Sie dies tun, indem Sie in den Unterpositionen vor jedem Bindestrich ein einzelnes Leerzeichen setzen. YAML verwendet Leerzeichen und **keine** Tabulatoren zum Einrücken. Nachfolgend sehen Sie hierfür ein Beispiel.

[prism classes="language-yaml line-numbers"]
-
 - Cat
 - Dog
 - Goldfish
-
 - Python
 - Lion
 - Tiger
[/prism]

Wenn Sie Ihre Sequenzen noch tiefer verschachteln möchten, brauchen Sie nur weitere Ebenen hinzuzufügen.

[prism classes="language-yaml line-numbers"]
-
 -
  - Cat
  - Dog
  - Goldfish
[/prism]

Sequenzen können zu anderen Datenstrukturtypen, wie Mappings oder Scalars, hinzugefügt werden.

## Mappings

Mapping gibt Ihnen die Möglichkeit, Schlüssel mit Werten aufzulisten. Das ist in den Fällen sinnvoll, in denen Sie einem bestimmten Element einen Namen oder eine Eigenschaft zuweisen.

[prism classes="language-yaml line-numbers"]
animal: pets
[/prism]

Dieses Beispiel mappt den Wert `pets` auf den Schlüssel `animal`. In Verbindung mit einer Sequenz können Sie sehen, dass damit eine Liste von `pets` erstellt wird. Im folgenden Beispiel dient der Bindestrich, der zur Kennzeichnung der einzelnen Elemente verwendet wird, zusammen mit der Einrückung, dazu, dass die Zeilen als untergeordnete Elemente und die Mappingzeile `pets` als übergeordnetes Element gilt.

[prism classes="language-yaml line-numbers"]
pets:
 - Cat
 - Dog
 - Goldfish
[/prism]

Quellen und weiterführende Dokumentationen
-----

Weitere Informationen zu YAML, einschließlich einer ausführlichen Dokumentation zur Funktionsweise, finden Sie in den unten verlinkten Ressourcen.

* [Dave's YAML Primer](https://github.com/darvid/trine/wiki/YAML-Primer)
* [Official YAML 1.2 Documentation](https://yaml.org/spec/1.2/spec.html)
* [YAML Reference Card](https://yaml.org/refcard.html)
* [Xavier Shay's YAML Tutorial](http://rhnh.net/2011/01/31/yaml-tutorial)
* [YAMLLint](http://www.yamllint.com/)
