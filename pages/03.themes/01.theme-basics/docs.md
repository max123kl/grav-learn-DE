---
title: Allgemeines zu Themes
taxonomy:
    category: docs
---

Die Themes in Grav sind relativ simpel und sehr flexibel, da sie mit der leistungsstarken [Twig Templating-Engine](https://twig.sensiolabs.org/) erstellt werden. Jedes Theme wird aus einer Kombination von Twig-Dateien (einer Mischung aus Twig-ähnlichem PHP-Code und HTML), den sogenannten Templates, und CSS erstellt. Wir verwenden normalerweise die [Sass CSS-Erweiterung](http://sass-lang.com) zur Generierung unserer CSS-Dateien, aber nichts hindert Sie daran auch [Less](http://lesscss.org/) oder sogar normales CSS zu verwenden. Es kommt einfach auf Ihre persönlichen Vorlieben an.

## Inhaltsseiten und Twig-Templates

Als erstes muss die genaue Beziehung verstanden werden, die zwischen den **Seiten**  (pages) in Grav und den **Twig-Template-Dateien** eines Themes besteht.

Jede Seite, die Sie erstellen, verweist auf eine bestimmte Template-Datei, entweder durch den Namen der Seitendatei oder durch das Setzen der Template-Header-Variablen für die Seite. Aus Gründen der leichteren Wartbarkeit empfehlen wir, wenn immer möglich, den Seitennamen zu verwenden, anstatt ihn mit der Header-Variablen zu überschreiben.

Wir wollen ein einfaches Beispiel durchspielen. Falls Sie das [Grav-Basis-Paket](../../basics/installation) installiert haben, werden Sie feststellen, dass Sie im Ordner `user/pages/01.home` eine Datei mit dem Namen `default.md` vorfinden. Diese Datei enthält den gesamten Inhalt der Seite auf Basis von Markdown. Der Name dieser Datei, d.h. `default`, sagt Grav, dass diese Seite mit dem Twig-Template namens `default.html.twig` gerendert werden soll, das sich im Ordner `templates/` des Themes befindet.

!! Die Namen der Seiten-Templates **müssen** kleingeschrieben werden, wie „default“, „blog“, usw.

Wenn Sie eine Seiteninhalt-Datei namens `blog.md` hätten, würde Grav versuchen, sie mit dem Twig-Template `<Ihr_Theme>/templates/blog.html.twig` zu rendern:

!! Die Namen der Dateien in Grav werden nicht auf dem Frontend von Grav angezeigt. Nur die Ordnernamen erscheinen dort. Machen Sie sich keine Gedanken, wenn alle Ihre Blog-Beiträge den gleichen Dateinamen haben. Das ist ganz normal.

## Organisatorische Struktur der Themes

### Definition und Konstruktion

Jedes Theme sollte eine Definitionsdatei mit Namen blueprints.yaml haben. Sie enthält einige Informationen über das Thema. Optional kann sie **Formular-Definitionen** aufnehmen, die im [**Admin-Panel**](../../admin-panel/introduction) verwendet werden. Dadurch wird das Editieren von Theme-Optionen ermöglicht. Das Theme **Antimatter** hat die folgende Datei `blueprints.yaml`:

[prism classes="language-yaml line-numbers"]
name: Antimatter
slug: antimatter
type: theme
version: 1.6.7
description: "Antimatter is the default theme included with **Grav**"
icon: empire
author:
  name: Team Grav
  email: devs@getgrav.org
  url: https://getgrav.org
homepage: https://github.com/getgrav/grav-theme-antimatter
demo: https://demo.getgrav.org/blog-skeleton
keywords: antimatter, theme, core, modern, fast, responsive, html5, css3
bugs: https://github.com/getgrav/grav-theme-antimatter/issues
license: MIT

dependencies:
    - { name: grav, version: '>=1.6.0' }

form:
  validation: loose
  fields:
    dropdown.enabled:
        type: toggle
        label: Dropdown in navbar
        highlight: 1
        default: 1
        options:
          1: Enabled
          0: Disabled
        validate:
          type: bool
[/prism]

Wenn Sie Theme-Konfigurationsoptionen verwenden möchten, sollten Sie die Standardeinstellungen in der Datei `<Ihr_Theme>.yaml` vornehmen. Als Beispiel:

[prism classes="language-yaml line-numbers"]
enabled: true
color: blue
[/prism]

!! Die Option `color: blue` bewirkt hier eigentlich gar nichts. Sie dient lediglich als Beispiel dafür, wie eine Einstellung übersteuert werden kann.

Weitere Informationen über die verfügbaren Formulare, die Sie erstellen können, finden Sie in [Kapitel 6. Formulare](../../forms). Sie sollten auch ein Bild (mit `300px` x `300px`) Ihres Theme-Designs im Stammverzeichnis des Themes zur Verfügung stellen und es `thumbnail.jpg` nennen. Es wird in der Theme-Rubrik des Admin-Panels angezeigt.

### Templates

Es gibt **keine festen Regeln** bezüglich der Struktur eines Grav-Themes, außer dass es für jeden der Seitentypen, die Sie in Ihrem Content verwenden, entsprechende Twig-Vorlagen in dem Ordner `templates/` geben muss.

!! Aufgrund dieser engen Kopplung in einem Theme zwischen Seiteninhalt und Twig-Templates ist es oft sinnvoll, Themes in Verbindung mit dem Inhalt zu entwickeln, mit dem sie auch verwendet werden sollen. Eine gute Möglichkeit, _universelle_ Themes zu erstellen, ist die Verwendung der Template-Typen, die von den Skeleton-Paketen angewendet werden. Sie sind auf unserer [Download-Seite](https://getgrav.org/downloads) verfügbar. Zum Beispiel support: **default**, **blog**, **error**, **item** und **modular**.

Im Allgemeinen sollte das Stammverzeichnis des Ordners `templates/` dazu dienen, die primären, benutzten Vorlagen abzulegen und dann den Unterordner `partials/` zu erstellen, der Teile oder kleinere Template _Chunks_ enthält.

Wenn Sie **modulare** Templates in Ihrem Theme nutzen möchten, sollten Sie auch das Unterverzeichnis `modular/` erstellen und darin Ihre modularen Twig-Template-Dateien speichern.

Das gleiche gilt für die Unterstützung von **Formularen**. Erstellen Sie das Unterverzeichnis `forms/` und speichern Sie darin alle benutzerdefinierten Formularvorlagen.

### SCSS / LESS / CSS

Auch hier ist nichts in Stein gemeißelt, aber eine bewährte Praxis ist ein Unterverzeichnis `scss/` anzulegen, wenn Sie mit Sass entwickeln wollen, oder `less/`, wenn Sie Less bevorzugen, zusammen mit einem Ordner `css/`, um die statischen CSS-Dateien unterzubringen. Außerdem sollte es einen Ordner `css-compiled/` geben, für alle automatisch generierten Dateien aus Ihren Sass- oder Less-Arbeiten.

Wie Sie Ihre Dateien hier strukturieren, liegt ganz in Ihrer Hand. Zögern Sie nicht, für einige Ideen unserem Beispiel im Standardtheme **Antimatter** zu folgen, das mit dem Grav-Basis-Paket mitgeliefert wird. Wir arbeiten mit der Variante **scss** von Sass, die CSS ähnlicher ist und, offen gesagt, es ist angenehmer zu schreiben.

Um Sass auf Ihrem Computer zu installieren, befolgen Sie einfach die Anweisungen auf der Website [sass-lang.com](http://sass-lang.com/install).

1. Starten Sie das mitgelieferte scss-Shell-Skript und geben Sie `./scss.sh` aus dem Stammverzeichnis des Themes ein.
2. Alternativ kann der Befehl `scss --source-map --watch scss:css-compiled` auch direkt ausgeführt werden.

Dadurch werden Ihre scss-Dateien automatisch kompiliert und in dem Ordner `css-compiled/` abgelegt. Sie können dann die resultierende css-Datei in Ihrem Theme referenzieren.

### Blueprints

Das Verzeichnis `blueprints/` dient dazu, Formulare für Optionen und Konfiguration für jede der Template-Dateien zu definieren. Diese werden vom **Admin-Panel** verwendet und sind optional. Das Theme ist ohne diese Dateien zu 100% funktionsfähig, aber sie können nicht über das Admin-Panel bearbeitet werden ohne die entsprechenden Formulare.

### Theme- und Plugin-Events

Eine weitere leistungsstarke Funktion, die rein optional ist, ist die Möglichkeit, dass ein Thema über die **Plugin**-Architektur mit Grav interagieren kann. Kurz gesagt, während der Initialisierungssequenz von Grav gibt es mehrere Punkte in der Sequenz, an denen Sie Ihr eigenes Codefragment "anhängen" können. Dadurch lassen sich, z.B. während der Initialisierung von Twig, in Ihrem Theme zusätzliche Pfadverknüpfungen definieren, sodass Sie diese in Ihren Twig-Templates übernehmen können. Diese Hooks, mit vom Grav-System vordefinierten Namen, stehen Ihnen über eine Reihe von "leeren" Funktionen zur Verfügung, die Sie nach Belieben befüllen können. [Kapitel 4. Plugins](../../plugins) enthält zusätzliche Informationen über das Plugin-System und die verfügbaren Event-Hooks. Damit Sie diese Hooks in Ihrem Thema verwenden können, müssen Sie lediglich eine Datei `mytheme.php` erstellen und dafür folgende Struktur verwenden:

[prism classes="language-php line-numbers"]
<?php
namespace Grav\Theme;

use Grav\Common\Theme;

class MyTheme extends Theme
{

    public static function getSubscribedEvents(): array
    {
        return [
            'onThemeInitialized' => ['onThemeInitialized', 0]
        ];
    }

    public function onThemeInitialized(): void
    {
        if ($this->isAdmin()) {
            $this->active = false;
            return;
        }

        $this->enable([
            'onTwigSiteVariables' => ['onTwigSiteVariables', 0]
        ]);
    }

    public function onTwigSiteVariables(): void
    {
        $this->grav['assets']
            ->addCss('plugin://css/mytheme-core.css')
            ->addCss('plugin://css/mytheme-custom.css');

        $this->grav['assets']
            ->add('jquery', 101)
            ->addJs('theme://js/jquery.myscript.min.js');
    }
}
[/prism]

Wie Sie sicher bemerkt haben, müssen die Event-Hooks, vor der möglichen Verwendung, zuerst mit der Funktion `getSubscribedEvents` in einer Liste deklariert und dann mit Ihrem eigenen Code definiert werden. Wenn Sie ein bestimmtes Event nutzen wollen, subskribieren Sie es natürlich auch. Sie erhalten andernfalls eine Fehlermeldung.

### Weitere Verzeichnisse

Wir empfehlen dringend, individuelle Ordner im Stammverzeichnis Ihres Themes für `images/`, `fonts/` und `js/` anzulegen. In diesen Verzeichnissen können Sie die Bilder Ihres Themes, alle benutzerdefinierten Web-Fonts und die benötigten Javascript-Dateien ablegen.

## Theme-Beispiel

Nehmen wir beispielsweise das Standardtheme **Antimatter**. Nachfolgend sehen Sie die Gesamtstruktur dieses Themes:

![Theme Folders](theme-folders.png)

In diesem Beispiel haben wir die vorhandenen Dateien `css`, `css-compiled`, `fonts`, `images`, `js`, `scss` und `templates` ignoriert, um die Lesbarkeit zu verbessern. Es ist wichtig, die Gesamtstruktur des Themes zu erkennen.
