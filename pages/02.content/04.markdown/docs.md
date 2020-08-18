---
title: Markdown Syntax
page-toc:
  active: true
taxonomy:
    category: docs
---

Seien wir ehrlich: Das Schreiben von Web-Inhalten ist ermüdend. WYSIWYG-Editoren helfen, diese Aufgabe zu erleichtern, aber sie führen in der Regel zu schrecklichem Code oder, schlimmer noch, zu hässlichen Webseiten.

**Markdown** ist eine einfachere Art, um **HTML** zu schreiben. Ganz ohne die ganze grauenhafte Komplexität, die normalerweise damit verbunden ist.

Einige der wesentlichen Vorteile sind:

1. Markdown ist einfach zu erlernen, mit nur wenigen zusätzlichen Formatierungen, so dass der Inhalt auch schneller geschrieben werden kann.
2. Geringeres Risiko von Schreibfehlern beim Niederschreiben
3. Erzeugt gültigen XHTML-Quelltext
4. Hält den Inhalt und die visuelle Gestaltung getrennt, so dass Sie das Aussehen Ihrer Website nicht verunstalten können.
5. Kann mit einem beliebigen Texteditor oder einer Markdown-Anwendung geschrieben werden.
6. Markdown macht Spaß!

John Gruber, der Autor von Markdown, drückt es so aus:

> Das vorrangige Designziel für die Formatierungssyntax von Markdown ist es, sie so lesbar wie möglich zu gestalten. Die Idee ist, dass ein mit Markdown formatiertes Dokument so veröffentlicht werden kann, wie es ist, als einfacher Text, ohne dass es aussieht, als sei es mit Tags oder Formatierungsanweisungen versehen worden. Während die Syntax von Markdown durch mehrere bestehende Text-zu-HTML-Filter beeinflusst wurde, ist die größte Inspirationsquelle für die Syntax von Markdown das Format von E-Mails im Nur-Text-Format.
> -- <cite>John Gruber</cite>

Grav verfügt über eine integrierte Schnittstelle für [Markdown](https://daringfireball.net/projects/markdown/) und [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/). Sie können **Markdown Extra** in der Konfigurationsdatei `system.yaml` aktivieren.

Lassen Sie uns ohne große Umschweife die Hauptelemente von Markdown betrachten und uns ansehen, wie das resultierende HTML aussieht:

!! <i class="fa fa-bookmark"></i> Legen Sie für diese Seite ein Lesezeichen an, um in Zukunft leichter darauf zugreifen zu können!

## Überschriften

Überschriften vom Typ `h1` bis `h6` werden mit einem `#` für jede Ebene erzeugt:

[prism classes="language-markdown"]
# h1 Heading
## h2 Heading
### h3 Heading
#### h4 Heading
##### h5 Heading
###### h6 Heading
[/prism]

wird gerendert zu:

# h1 Heading
## h2 Heading
### h3 Heading
#### h4 Heading
##### h5 Heading
###### h6 Heading

HTML:

[prism classes="language-html"]
<h1>h1 Heading</h1>
<h2>h2 Heading</h2>
<h3>h3 Heading</h3>
<h4>h4 Heading</h4>
<h5>h5 Heading</h5>
<h6>h6 Heading</h6>
[/prism]

## Kommentare

Kommentare sollten HTML-kompatibel sein

[prism classes="language-html"]
<!--
This is a comment
-->
[/prism]

Der untenstehende Kommentar sollte **NICHT** sichtbar sein:

<!--
This is a comment
-->

## Horizontale Linien

Das HTML-Element `<hr>` dient dazu, eine „thematische Trennung“ zwischen den Elementen auf Absatzebene zu schaffen. In Markdown können Sie ein `<hr>` mit einem der folgenden Elemente erstellen:

* `___`: drei aufeinanderfolgende Unterstriche
* `---`: drei aufeinanderfolgende Minus-Striche (divis)
* `***`: drei aufeinanderfolgende Sternchen

wird so angezeigt:

___

---

***

## Body-Text

Der Fließtext wird normal geschrieben, einfacher Text wird im gerenderten HTML mit `<p></p>` Tags umhüllt.

So wird dieser Body-Text:

[prism classes="language-markdown"]
Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri, animal tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officiis his ex, soluta officiis concludaturque ei qui, vide sensibus vim ad.
[/prism]

zu diesem HTML gerendert:

[prism classes="language-html"]
<p>Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri, animal tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officiis his ex, soluta officiis concludaturque ei qui, vide sensibus vim ad.</p>
[/prism]

Ein **Zeilenumbruch** kann mit 2 Leerzeichen gefolgt von 1 Return erreicht werden.

## Inline HTML

Wenn Sie einen bestimmten HTML-Tag (mit einer Klasse) benötigen, können Sie auch einfach HTML einsetzen:

[prism classes="language-html"]
Absatz in Markdown.

<div class="class">
    Das ist <b>HTML</b>
</div>

Absatz in Markdown.
[/prism]

## Schriftauszeichnung (Hervorhebungen)

### Bold (fett)

Die Hervorhebung eines Textabschnitts mit einer kräftigeren Schriftart.

Der folgende Textausschnitt wird **als fetter Text** wiedergegeben.

[prism classes="language-markdown"]
**als fetter Text gerendert**
[/prism]

wird gerendert zu:

**als fetter Text gerendert**

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<strong>als fetter Text gerendert</strong>
[/prism]

### Italic (kursiv)

Die Hervorhebung eines Textausschnitts durch Kursivschrift.

Der folgende Textausschnitt wird _als kursivierter Text_ wiedergegeben.

[prism classes="language-markdown"]
_als Kursivschrift gerendert_
[/prism]

wird gerendert zu:

_als Kursivschrift gerendert_

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<em>als Kursivschrift gerendert</em>
[/prism]

### Durchgestrichen

In GFM (GitHub flavored Markdown) können Sie Durchstreichungen vornehmen.

[prism classes="language-markdown"]
~~Dieser Text wird durchgestrichen~~
[/prism]

wird gerendert zu:

~~Dieser Text wird durchgestrichen~~

in HTML:

[prism classes="language-html"]
<del>Dieser Text wird durchgestrichen</del>
[/prism]

## Blockquotes (Zitate)

Zitieren von Inhaltsteilen aus einer anderen Quelle innerhalb Ihres Dokuments:

Fügen Sie `>` vor den Text ein, den Sie zitieren möchten.

[prism classes="language-markdown"]
> **Fusion Drive** combines a hard drive with a flash storage (solid-state drive) and presents it as a single logical volume with the space of both drives combined.
[/prism]

Wird gerendert zu:

> **Fusion Drive** combines a hard drive with a flash storage (solid-state drive) and presents it as a single logical volume with the space of both drives combined.

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<blockquote>
  <p><strong>Fusion Drive</strong> combines a hard drive with a flash storage (solid-state drive) and presents it as a single logical volume with the space of both drives combined.</p>
</blockquote>
[/prism]

Zitate können auch ineinander verschachtelt werden:

[prism classes="language-markdown"]
> Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue.
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
>> Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor
odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.
[/prism]

Wird gerendert zu:

> Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue.
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
>> Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor
odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.

## Hinweise

! Der alte Mechanismus für Hinweise, welche die Syntax von Zitaten (>>>) außer Kraft setzten, ist veraltet. Hinweise werden jetzt über das spezielle Plugin [„Markdown Notices“](https://github.com/getgrav/grav-plugin-markdown-notices) verarbeitet.

## Listen

### Ungeordnet

Eine Liste von Punkten, bei denen die Reihenfolge der Elemente nicht unbedingt von Belang ist.

Sie können eines der folgenden Zeichen verwenden, um für jedes Listenelement Aufzählungszeichen zu setzen:

[prism classes="language-markdown"]
* valid bullet
- valid bullet
+ valid bullet
[/prism]

Zum Beispiel

[prism classes="language-markdown"]
+ Lorem ipsum dolor sit amet
+ Consectetur adipiscing elit
+ Integer molestie lorem at massa
+ Facilisis in pretium nisl aliquet
+ Nulla volutpat aliquam velit
  - Phasellus iaculis neque
  - Purus sodales ultricies
  - Vestibulum laoreet porttitor sem
  - Ac tristique libero volutpat at
+ Faucibus porta lacus fringilla vel
+ Aenean sit amet erat nunc
+ Eget porttitor lorem
[/prism]

Wird gerendert zu:

+ Lorem ipsum dolor sit amet
+ Consectetur adipiscing elit
+ Integer molestie lorem at massa
+ Facilisis in pretium nisl aliquet
+ Nulla volutpat aliquam velit
  - Phasellus iaculis neque
  - Purus sodales ultricies
  - Vestibulum laoreet porttitor sem
  - Ac tristique libero volutpat at
+ Faucibus porta lacus fringilla vel
+ Aenean sit amet erat nunc
+ Eget porttitor lorem

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<ul>
  <li>Lorem ipsum dolor sit amet</li>
  <li>Consectetur adipiscing elit</li>
  <li>Integer molestie lorem at massa</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit
    <ul>
      <li>Phasellus iaculis neque</li>
      <li>Purus sodales ultricies</li>
      <li>Vestibulum laoreet porttitor sem</li>
      <li>Ac tristique libero volutpat at</li>
    </ul>
  </li>
  <li>Faucibus porta lacus fringilla vel</li>
  <li>Aenean sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ul>
[/prism]

### Geordnet

Eine Liste von Punkten, bei denen die Reihenfolge der Elemente grundsätzlich wichtig ist.

[prism classes="language-markdown"]
1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
4. Facilisis in pretium nisl aliquet
5. Nulla volutpat aliquam velit
6. Faucibus porta lacus fringilla vel
7. Aenean sit amet erat nunc
8. Eget porttitor lorem
[/prism]

Wird gerendert zu:

1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
4. Facilisis in pretium nisl aliquet
5. Nulla volutpat aliquam velit
6. Faucibus porta lacus fringilla vel
7. Aenean sit amet erat nunc
8. Eget porttitor lorem

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<ol>
  <li>Lorem ipsum dolor sit amet</li>
  <li>Consectetur adipiscing elit</li>
  <li>Integer molestie lorem at massa</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit</li>
  <li>Faucibus porta lacus fringilla vel</li>
  <li>Aenean sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ol>
[/prism]

**Tipp**: Wenn Sie immer nur die `1.` für jede Ziffer verwenden, wird Markdown jedes Element automatisch nummerieren. Zum Beispiel:

[prism classes="language-markdown"]
1. Lorem ipsum dolor sit amet
1. Consectetur adipiscing elit
1. Integer molestie lorem at massa
1. Facilisis in pretium nisl aliquet
1. Nulla volutpat aliquam velit
1. Faucibus porta lacus fringilla vel
1. Aenean sit amet erat nunc
1. Eget porttitor lorem
[/prism]

Wird gerendert zu:

1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
4. Facilisis in pretium nisl aliquet
5. Nulla volutpat aliquam velit
6. Faucibus porta lacus fringilla vel
7. Aenean sit amet erat nunc
8. Eget porttitor lorem

## Code-Darstellung

### Inline Code

Inline Code-Schnipsel werden mit `` ` `` umschlossen.

[prism classes="language-markdown"]
In this example, `<section></section>` should be wrapped as **code**.
[/prism]

Wird gerendert zu:

In this example, `<section></section>` should be wrapped with **code**.

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<p>In this example, <code>&lt;section&gt;&lt;/section&gt;</code> should be wrapped with <strong>code</strong>.</p>
[/prism]

### Eingerückter Code

Mehrere Zeilen Code um mindestens vier Leerzeichen einrücken, wie in:

<pre>
  // Some comments
  line 1 of code
  line 2 of code
  line 3 of code
</pre>

Wird gerendert zu:

[prism classes="language-txt"]
// Some comments
line 1 of code
line 2 of code
line 3 of code
[/prism]

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<pre>
  <code>
    // Some comments
    line 1 of code
    line 2 of code
    line 3 of code
  </code>
</pre>
[/prism]

### Codeblock „Einzäunungen“

Verwenden Sie die „Einzäunungen“ ```` ``` ````, um mehrere Codezeilen in einem Block darzustellen

<pre>
```
Hier ist ein Beispiel-Text ...
```
</pre>

in HTML:

[prism classes="language-html"]
<pre language-html>
  <code>Hier ist ein Beispiel-Text ...</code>
</pre>
[/prism]

### Syntax Highlighting

GFM oder „GitHub Flavored Markdown“ unterstützt auch Syntax-Highlighting. Um es zu aktivieren, fügen Sie einfach die Dateierweiterung der Sprache, die Sie einsetzen wollen, direkt nach der ersten Codeblock „Einzäunung“ (` ```js `) hinzu. Damit wird die Syntaxhervorhebung automatisch in das gerenderte HTML übernommen. Zum Beispiel, um Syntax-Highlighting auf JavaScript-Code anzuwenden:

<pre>
```js
grunt.initConfig({
  assemble: {
    options: {
      assets: 'docs/assets',
      data: 'src/data/*.{json,yml}',
      helpers: 'src/custom-helpers.js',
      partials: ['src/partials/**/*.{hbs,md}']
    },
    pages: {
      options: {
        layout: 'default.hbs'
      },
      files: {
        './': ['src/templates/pages/index.hbs']
      }
    }
  }
};
```
</pre>

Wird gerendert zu:

[prism classes="language-js"]
grunt.initConfig({
  assemble: {
    options: {
      assets: 'docs/assets',
      data: 'src/data/*.{json,yml}',
      helpers: 'src/custom-helpers.js',
      partials: ['src/partials/**/*.{hbs,md}']
    },
    pages: {
      options: {
        layout: 'default.hbs'
      },
      files: {
        './': ['src/templates/pages/index.hbs']
      }
    }
  }
};
[/prism]

!!! Damit das Syntax-Highlighting funktioniert, muss das [Highlight-Plugin](https://github.com/getgrav/grav-plugin-highlight) installiert und aktiviert sein. Es verwendet wiederum ein Jquery-Plugin, so dass auch Jquery in Ihr Theme geladen werden muss.

## Tabellen

Tabellen werden durch Hinzufügen von Pipes (`|`) als Trennlinien zwischen den einzelnen Zellen und durch Hinzufügen einer Linie aus Minus-Strichen unter der Kopfzeile erstellt, ebenfalls durch Pipes getrennt. Beachten Sie, dass die Trennzeichen vertikal nicht ausgerichtet sein müssen.

[prism classes="language-markdown"]
| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
[/prism]

Wird gerendert zu:

[div class="table"]
| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
[/div]

und so sieht der HTML-Code aus:

[prism classes="language-html"]
<table>
  <thead>
    <tr>
      <th>Option</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>data</td>
      <td>path to data files to supply the data that will be passed into templates.</td>
    </tr>
    <tr>
      <td>engine</td>
      <td>engine to be used for processing templates. Handlebars is the default.</td>
    </tr>
    <tr>
      <td>ext</td>
      <td>extension to be used for dest files.</td>
    </tr>
  </tbody>
</table>
[/prism]

### Rechtsbündiger Text

Durch Hinzufügen eines Doppelpunkts auf der rechten Seite der Minus-Striche unter der Kopfzeile wird der Text für diese Spalte rechtsbündig ausgerichtet.

[prism classes="language-markdown"]
| Option | Description |
| ------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
[/prism]

[div class="table"]
| Option | Description |
| ------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
[/div]

## Links

### Grundlegender Link

[prism classes="language-markdown"]
[Assemble](https://assemble.io)
[/prism]

Rendert zu (beim Maus-Over gibt es _keinen_ Tooltip):

[Assemble](https://assemble.io)

in HTML:

[prism classes="language-html"]
<a href="https://assemble.io">Assemble</a>
[/prism]

### Titel hinzufügen

[prism classes="language-markdown"]
[Upstage](https://github.com/upstage/ "Visit Upstage!")
[/prism]

Rendert zu (beim Maus-Over _sollte_ ein Tooltip erscheinen):

[Upstage](https://github.com/upstage/ "Visit Upstage!")

in HTML:

[prism classes="language-html"]
<a href="https://github.com/upstage/" title="Visit Upstage!">Upstage</a>
[/prism]

### Benannte Anker

Benannte Anker ermöglichen es Ihnen, zum angegebenen Ankerpunkt auf derselben Seite zu springen. Beispielsweise für jedes dieser Kapitel:

[prism classes="language-markdown"]
# Table of Contents
  * [Chapter 1](#chapter-1)
  * [Chapter 2](#chapter-2)
  * [Chapter 3](#chapter-3)
[/prism]

wird der Link zu diesen Abschnitten springen:

[prism classes="language-markdown"]
## Chapter 1 <a id="chapter-1"></a>
Content for chapter one.

## Chapter 2 <a id="chapter-2"></a>
Content for chapter one.

## Chapter 3 <a id="chapter-3"></a>
Content for chapter one.
[/prism]

**HINWEIS:**Die genaue Platzierung des Anker-Tags scheint willkürlich zu sein. Sie werden hier inline platziert, da es unauffällig zu sein scheint und auch funktioniert.

## Bilder

Bilder haben eine ähnliche Syntax wie Links, enthalten aber ein vorangestelltes Ausrufezeichen.

[prism classes="language-markdown"]
![Minion](https://octodex.github.com/images/minion.png)
[/prism]
![Minion](https://octodex.github.com/images/minion.png)

oder:

[prism classes="language-markdown"]
![Alt text](https://octodex.github.com/images/stormtroopocat.jpg "The Stormtroopocat")
[/prism]
![Alt text](https://octodex.github.com/images/stormtroopocat.jpg "The Stormtroopocat")

Wie Links haben auch Bilder eine Syntax im Stil von Fußnoten:

[prism classes="language-markdown"]
![Alt text][id]
[/prism]
![Alt text][id]

Mit einem späteren Verweis im Dokument, der den URL-Standort definiert:

[id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"


    [id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"
