---
title: Individuelle Anpassung
taxonomy:
    category: docs
---

Es gibt viele Möglichkeiten, ein Theme anzupassen. Grav schränkt Ihre Kreativität in dieser Hinsicht wirklich nicht ein. Es gibt jedoch einige Features und einige Funktionen, die von Grav unterstützt werden, um diesen Prozess zu erleichtern.

## Angepasstes CSS

Der einfachste Weg, ein Theme anzupassen, ist die Erstellung einer eigenen `custom.css` Datei. Das **Quark**-Standardthema bietet mit Hilfe des **Asset-Managers** einen Zugriff auf eine Datei `css/custom.css`. Erfreulicherweise übernimmt der **Asset-Manager** diese Aufgabe für uns. Falls die Datei nicht vorhanden ist, wird die Verlinkung nicht in den HTML-Code eingefügt.

However, if you do provide a file called `custom.css` in Quark's `css/` folder, this will get picked up and referenced. You just need to ensure that you provide CSS elements with enough [specificity](http://www.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/) to override the default CSS. For example:

**custom.css**

[prism classes="language-css line-numbers"]
body a {
    color: #CC0000;
}
[/prism]

Dadurch wird die voreingestellte Linkfarbe überschrieben und stattdessen die Farbe **rot** verwendet.

## Angepasstes SCSS/LESS

Der nächste Schritt nach der Bereitstellung einer benutzerdefinierten CSS-Datei ist die Verwendung einer `_custom.scss` Datei. Quark wurde mit [SCSS](http://sass-lang.com/) geschrieben, einem CSS-kompatiblen Präprozessor, der Ihnen ermöglicht CSS mit Hilfe von [Variablen, verschachtelten Strukturen, partiellen Strukturen, Importen, Operatoren und Mix-Ins](http://sass-lang.com/guide) effizienter zu schreiben.

Das mag zunächst etwas abschreckend klingen, aber Sie können so viel oder so wenig SCSS verwenden, wie Sie möchten. Wenn Sie erst einmal damit begonnen haben, werden Sie Schwierigkeiten haben, zu traditionellem CSS zurückzukehren. Versprochen!

Das Quark Theme hat einen Ordner `scss/`, der eine Vielzahl von `.scss` Dateien enthält. Diese sollten in den Ordner `css-compiled/` kompiliert werden.

Sie können eine Datei mit dem Namen `scss/theme/_custom.scss` erstellen und sie mit `@import 'theme/custom';` ans Ende der Datei `theme.scss` einfügen. Es gibt mehrere wesentliche Gründe, Ihren Code in diese Datei aufzunehmen:

1. Die daraus entstandenen Änderungen werden zusammen mit dem übrigen CSS-Code in die `css-compiled/theme.min.css` Datei geschrieben.
2. Sie haben Zugriff auf alle Variablen und Mix-Ins, die für jedes andere im Theme verwendete SCSS verfügbar sind.
3. Sie haben Zugriff auf alle Standard-SCSS-Features und -Funktionen, die die Entwicklung erleichtern.

Ein Beispiel für diese Datei wäre:

**_custom.scss**

[prism classes="language-css line-numbers"]
body {
    a {
        color: darken($core-accent, 30%);
    }
}
[/prism]

Der Nachteil dieses Ansatzes ist, dass diese Datei bei jeder *Theme-Aktualisierung* überschrieben wird, so dass Sie darauf achten sollten, eine Sicherungskopie Ihrer eigenen Arbeit zu erstellen.  Dieses Problem wird durch die Verwendung der Vererbung von Themes, wie weiter unten beschrieben, gelöst.

## Wellington SCSS

[Wellington](https://github.com/wellington/wellington) ist ein nativer Wrapper für [LibSass](http://libsass.org/), das sowohl für Linux als auch für MacOS verfügbar ist. Es bietet eine viel schnellere Lösung zum Kompilieren von SCSS als der standardmäßige Ruby-basierte scss-Compiler. Mit schneller meinen wir etwa **20X schneller!** Er ist über Brew super einfach zu installieren:

[prism classes="language-bash command-line"]
brew install wellington
[/prism]

Um die Vorteile beim Kompilieren der `scss` Ordner in einen `css-compiled` Ordner, wie im Beispiel oben, zu nutzen, können Sie diesen [Gist](https://gist.github.com/rhukster/bcfe030e419028422d5e7cdc9b8f75a8) verwenden.

!! Wellington ist unser Werkzeug, das wir für alle Themes von _Team Grav_ verwendet haben und es hat großartig funktioniert!


## Theme-Vererbung

Das ist der empfohlene Ansatz zum Ändern oder Anpassen eines Themes. Er erfordert jedoch eine etwas umfangreichere Konfiguration.

Das Grundkonzept besteht darin, dass Sie ein Theme als das **Basis-Theme** definieren, von dem Sie etwas erben wollen. Anschließend definieren Sie **nur die Bits, die Sie modifizieren wollen** und lassen das Basis-Theme den Rest erledigen. Der große Vorteil dabei ist, dass Sie das Basis-Theme leichter auf dem aktuellsten Stand halten können, ohne dass dies direkte Auswirkungen auf Ihr angepasstes geerbtes Theme hat.

Es gibt zwei Wege ein vorhandenes Theme zu beerben:

1. Verwenden der Befehlszeile (CLI) mit dem DevTools-Plugin
2. Manuell

### Vererbung mit der CLI-Methode

Wie im [Themes-Tutorial](../theme-tutorial) besprochen, können Sie mit dem DevTools-Plugin ein neues Theme erstellen. Sie können aber auch ein bestehendes Theme beerben. Das Prozedere ist einfach.

1. Falls nicht bereits geschehen, [installieren Sie das DevTools-Plugin](../theme-tutorial#step-1-install-devtools-plugin).
2. Folgen Sie dann der Anleitung [Basis-Theme](../theme-tutorial#step-2-create-base-theme) erstellen, doch wenn Sie aufgefordert werden, `Bitte wählen Sie ein Template`, geben Sie `inheritance` (Vererbung) ein. Wenn Quark im einzigen Thema enthalten ist, wird es als Option 0 angezeigt. Geben Sie also `0` ein, um von Quark zu erben. Ihr neues geerbtes Thema wird dann erstellt.
3. Kopieren Sie alle Optionen aus der von Ihnen geerbten Theme-YAML-Datei (bzw. aus dem Ordner `user/config/themes`, falls Sie diese angepasst haben) an den Anfang der neu erstellten YAML-Konfigurationsdatei Ihres Themes: `/user/themes/mytheme/mytheme.yaml`.
4. Kopieren Sie den Abschnitt „form“ aus der Datei `/user/themes/quark/blueprints.yaml` in die Datei `/user/themes/mytheme/blueprints.yaml`, um die anpassbaren Elemente des Themes in das Admin-Panel aufzunehmen (oder ersetzen Sie einfach die Datei und bearbeiten Sie ihren Inhalt).
5. Um Ihr neues **mytheme** zu verwenden, müssen Sie Ihr Standard-Theme ändern und hierzu die Option `pages: theme:` in Ihrer Konfigurationsdatei `user/config/system.yaml` bearbeiten:
   [prism classes="language-yaml line-numbers"]
   pages:
     theme: mytheme
   [/prism]

### Manuell vererben

Dazu müssen Sie die folgenden Schritte ausführen:

1. Erstellen Sie einen neuen Ordner: `user/themes/mytheme`, der Ihr neues Thema beherbergt.
2. Kopieren Sie die YAML-Datei des Themes (oder aus dem Ordner `user/config/themes`, falls Sie es angepasst haben), das Sie vererben wollen nach `/user/themes/mytheme/mytheme.yaml` und fügen Sie den folgenden Inhalt hinzu (ersetzen Sie dabei `user/themes/quark` durch den Namen des Themes, das Sie vererben):

   [prism classes="language-yaml line-numbers"]
   streams:
     schemes:
       theme:
         type: ReadOnlyStream
         prefixes:
           '':
             - user/themes/mytheme
             - user/themes/quark
   [/prism]

3. Kopieren Sie die Datei `/user/themes/quark/blueprints.yaml` nach `/user/themes/mytheme/blueprints.yaml`, um die konfigurierbaren Elemente des Themes in das Admin-Panel zu übernehmen.
4. Passen Sie Ihr Standard-Theme an Ihr neues **mytheme** an, indem Sie die Option `pages: theme:` in Ihrer Konfigurationsdatei `user/config/system.yaml` bearbeiten:

   [prism classes="language-yaml line-numbers"]
   pages:
     theme: mytheme
   [/prism]

5. Erstellen Sie eine neue Themes-Klassendatei, die zum Hinzufügen erweiterter ereignisgesteuerter Funktionen verwendet werden kann. Erstellen Sie eine Datei `user/themes/mytheme/mytheme.php`:

   [prism classes="language-php line-numbers"]
   <?php
   namespace Grav\Theme;

   class Mytheme extends Quark
   {
       // Some new methods, properties etc.
   }
   ?>
   [/prism]

Sie haben nun ein neues Thema mit dem Namen **mytheme** erstellt und die Streams so eingerichtet, dass zuerst im Theme **mytheme** nachgesehen und dann **quark** verwendet wird.  Quark ist also im Wesentlichen das Basis-Theme für dieses neue Theme.

Sie können dann genau die Dateien zur Verfügung stellen, die Sie benötigen, einschließlich **JS**, **CSS**, oder sogar Modifikationen an den **Twig-Template-Dateien**, wenn Sie dies wünschen.

### Verwenden von SCSS

Für die Modifikation bestimmter **SCSS**-Dateien müssen wir eine kleine Konfiguration vornehmen, damit das Programm weiß, dass es zuerst in Ihrem neuen `mytheme`-Verzeichnis nachsehen soll und erst dann in Ihrem `Quark`-Verzeichnis. Dazu sind ein paar Dinge erforderlich.

1. Zuerst müssen Sie die SCSS-Hauptdatei von Quark, die alle `@import`-Aufrufe für verschiedene Unterdateien enthält, kopieren. Kopieren Sie also die Datei `theme.scss` von `quark/scss/` in den Ordner `mytheme/scss/`.
2. Wenn Sie sich innerhalb der Datei `theme.scss` befinden, ändern Sie den Anfang aller Importzeilen in `@import '../../quark/scss/theme/';` so weiß das Programm, dass es die Dateien aus dem Quark-Thema verwenden soll. Beispielsweise wird so die erste Zeile `@import '../../quark/scss/theme/variables';` heißen.
3. Fügen Sie `@import 'theme/custom';` am unteren Ende der Datei `theme.scss` hinzu.
4. Der nächste Schritt besteht darin, eine Datei zu erstellen, die sich unter `mytheme/scss/theme/_custom.scss` befindet. Dort werden Ihre Änderungen vorgenommen.
5. Kopieren Sie die Dateien `gulpfile.js` und `package.json` in den Basis-Ordner des neuen Themes.

Um das neue SCSS für **mytheme** zu kompilieren, müssen Sie das Terminal öffnen und zum Themes-Ordner navigieren. Quark verwendet gulp, um den SASS-Code zu kompilieren, so dass Sie dieses installiert haben müssen und Sie benötigen Yarn für die Abhängigkeiten. Führen Sie `npm install -g gulp`, `yarn install` und dann `gulp watch` aus. So werden alle an den Dateien vorgenommenen Änderungen neu kompiliert.
