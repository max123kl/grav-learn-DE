---
title: Intro Kommandozeile
taxonomy:
    category: docs
---

Es ist kein Geheimnis, dass Grav mit Gedanken an die Kommandozeile entwickelt wurde. Während das Admin-Plugin es sicherlich einfacher macht, ohne das Öffnen eines Terminals (MacOS und Linux) oder einer Eingabeaufforderung (Windows) mehr zu erledigen, so gibt es doch eine Menge über die Geschwindigkeit und den Umfang der Kontrolle zu sagen, die die Arbeit auf der Befehlszeile mit sich bringt.

Das gilt besonders für Leute, die ihre eigenen Entwicklungsserver oder einen Remote-Server betreiben, mit dem sie die Möglichkeit haben, auf die Kommandozeile zuzugreifen. Die Menge der Tools, die Ihnen von der Befehlszeile aus zur Verfügung stehen, ist unglaublich. Sie können praktisch alle Aspekte des Hostings Ihrer Website mit einer geringen Anzahl von Tastaturbefehlen kontrollieren, einschließlich Grav sowie seiner Plugins und Themes.

Am Ende kommt es auf die persönlichen Vorlieben an. Auf dieser Seite werden wir einige hervorragende Hilfsmittel vorstellen, die Ihnen helfen sollen, sich mit der Kommandozeile vertraut zu machen.

!! Nicht alle Betriebssysteme sind miteinander kompatibel, wenn es um Befehle geht. Es gibt geringfügige Unterschiede zwischen MacOS und vielen Linux-Distributionen, wobei die Windows-Eingabeaufforderung einen ganz anderen Befehlssatz hat als die beiden anderen.

## MacOS

MacOS basiert auf Unix und entspricht den POSIX-Standards. Das bedeutet, dass die meisten Befehle, die Sie eventuell von anderen Unix- oder Linux-basierten Betriebssystemen kennen, funktionieren in MacOS genau so wie man es erwartet. Es gibt einige Ausnahmen von dieser Regel und aus diesem Grund empfehlen wir, die Terminal-Befehle für das spezifische Betriebssystem, mit dem Sie arbeiten, zu überprüfen.

Hier finden Sie einige nützliche Ressourcen, die Ihnen helfen sollen, sich an die Verwendung des Terminals unter MacOS zu gewöhnen:

* [MacOs Terminal – Benutzerhandbuch](https://support.apple.com/de-de/guide/terminal/welcome/mac) – Eine mehrteilige Anleitung zur Benutzung des Terminals.
* [Terminal in MacOS nutzen – Kommandozeile](https://www.macwelt.de/ratgeber/Terminal-Kommandozeile-Mac-OS-X-MacOS-4956946.html) – Eine ausführliche Beschreibung mit praktischen Beispielen für diverse Aufgaben.
* [Michael Hogg's MacOS Terminal Commands Guide](http://michael-hogg.co.uk/os_x_terminal.php) – Eine praktische Informationsquelle für MacOS-kompatible Terminal-Befehle, was sie bewirken und wie man sie verwendet.
* [MacRumors Guide to Terminal](http://guides.macrumors.com/Terminal) – Eine informative Ressource für die Navigation und Benutzung des Terminals, einschließlich Tipps für die Verwendung mit der GUI.
* [Envato Tuts+ Terminal Tips and Tricks](http://computers.tutsplus.com/tutorials/40-terminal-tips-and-tricks-you-never-thought-you-needed--mac-51192) - 40 clevere Tipps und Tricks zur Bedienung des Terminals. Enthält Befehle, die Sie in vielen einfachen Einführungen nicht finden.
* [Envato Tuts+ Taming the Terminal](http://computers.tutsplus.com/articles/new-mactuts-session-taming-the-terminal--mac-45471) - Ein mehrteiliger, ausführlicher Kurs zur Benutzung des Terminals. Enthält Videos, Screenshots und mehr.


## Linux

Die überwiegende Mehrzahl der Linux- (und Unix-) Distributionen da draußen haben eine große Gemeinsamkeit: die Kommandozeilen-Schnittstelle (Terminal) der Bash. Unabhängig davon, ob Sie eine GUI wie Gnome, Unity oder KDE benutzen oder nicht, besteht eine gute Chance, dass Sie die Kommandozeile aufgerufen haben, wenn Sie eine Linux-Distribution auf Ihrem Desktop oder Laptop laufen haben.

Denn sie ist mächtig. Sie können so ziemlich alles, was Sie mit der GUI machen könnten, direkt in der Befehlszeile ausführen, oft mit mehr Kontrolle darüber, wie die Befehle ausgeführt werden. Hier sind einige ausgezeichnete Ressourcen, die Ihnen helfen sollen, sich mit dem Terminal unter Linux vertraut zu machen:

* [TechSpot's Beginner's Guide to the Linux Command Line](http://www.techspot.com/guides/835-linux-command-line-basics/) – Ein ausgezeichneter Leitfaden für Anfänger an der Kommandozeile.
* [MakeUseOf's Quick Guide to Getting Started with the Linux Command Line](http://www.makeuseof.com/tag/a-quick-guide-to-get-started-with-the-linux-command-line/) – Eine weitere hervorragende Quelle, um mehr über das Terminal zu erfahren.
* [Die wichtigsten Linux-Befehle im Überblick](https://www.ionos.de/digitalguide/server/konfiguration/linux-befehle-terminal-kommandos-im-ueberblick/) – Eine umfangreiche Liste der wichtigsten Terminalbefehle.
* [O'Reilly Linux DevCenter Directory of Linux Commands](http://www.linuxdevcenter.com/cmd/) - An index of commands available in the Terminal.
* [Ryan's Tutorials Linux Tutorial](http://ryanstutorials.net/linuxtutorial/) – Ein ausgezeichneter All-in-One-Leitfaden für Linux und die Kommandozeilen-Schnittstelle (Terminal) der Bash.

## Windows

Windows ist aus einer Reihe von Gründen vom eigentlichen Programm getrennt. Viele der Befehle in der Befehlszeile für Windows erinnern an seine DOS-Wurzeln. Gängige Befehle wie `ls` für eine Verzeichnisauflistung funktionieren hier nicht. Stattdessen müssen Sie `dir` eingeben. Hier sind eine Reihe von Quellen, die Ihnen helfen sollen, mit der Windows-Eingabeaufforderung zurechtzukommen:

* [Die Kommandozeile unter Windows](https://www.wintotal.de/die-kommandozeile-unter-windows/) – Eine ausführliche Einführung in das „DOS-Prompt“ mit zahlrichen Querverweisen.
* [Windows-Befehle](https://docs.microsoft.com/de-de/windows-server/administration/windows-commands/windows-commands) – Eine Zusammenstellung, direkt vom Hersteller (auch in englisch verfügbar).
* [MakeUseOf's Beginner's Guide to the Windows Command Line](http://www.makeuseof.com/tag/a-beginners-guide-to-the-windows-command-line/) - A well-written introduction to the command line for Windows.
* [DOSPrompt.info](http://dosprompt.info/) - An entire site devoted to familiarizing users with the Command Prompt.

!! Alle CLI-Befehle von Grav basieren auf PHP, das in Windows nicht direkt verfügbar ist. Ob eine Installation vorhanden ist, können Sie herausfinden, indem Sie eine Konsole öffnen und zur Überprüfung `php -v` eingeben. Wenn `'php' als Befehl nicht erkannt wird ...`, ist es nicht installiert.

Wenn Sie PHP zu Ihrem Windows-System hinzufügen wollen, müssen Sie Ihre „Umgebungsvariablen“ finden, indem Sie entweder im Startmenü danach suchen oder über Systemsteuerung -> System -> Erweiterte Systemeinstellungen -> auf den Button „Umgebungsvariablen“ klicken.

Suchen Sie unter „Systemvariablen“ nach „Pfad“ und klicken Sie auf `Bearbeiten`. Kopieren Sie den „Variablenwert“ in einen Editor, und fügen Sie am Ende ein Semikolon hinzu – um eine neue Variablen einzufügen. Suchen Sie dann den Pfad zu Ihrer PHP-Installation (von der [Download-Seite](http://windows.php.net/) oder anhand einer aktuellen PHP-Installation, die mit Ihrer Entwicklungsumgebung gekommen ist). Fügen Sie den Pfad am Ende dieser langen Liste von Variablen hinzu. Sie benötigen nur den Ordner-Pfad, ohne die `php.exe`.

Wenn das erledigt ist, öffnen Sie wieder eine Konsole (oder starten Sie Ihre aktuelle Konsole neu), damit der neue Pfad übernommen wird. Dann versuchen Sie erneut `php -v`, Sie sollten eine Ausgabe erhalten wie: `PHP 7.0.7 (cli) ...`. Wenn Sie Grav-Befehle ausführen wollen, müssen Sie ihnen `php` voranstellen, zum Beispiel: `php grav/gpm index`

## Spezifische Grav-Befehle

Eine der besten Eigenschaften von Grav ist die Möglichkeit, dass Sie über eine Vielzahl leistungsfähiger Befehle verfügen können. Diese umfassen alles von der Installation zusätzlicher Plugins und Themes bis hin zum Anlegen von Benutzern in der Administration. In diesem Abschnitt werden wir viele der am häufigsten verwendeten Befehle vorstellen.

Alle unten aufgeführten Befehle sind mit <strong>jedem Betriebssystem</strong> kompatibel.

[version=15]
[div class="table table-keycol"]
| Befehl                            | Beschreibung                                                                                                                                      |
| :----------------                 | :--------------------------------------                                                                                                           |
| `bin/grav list`                   | Listet alle in Grav verfügbaren Befehle auf (außer dem GPM).                                                                                      |
| `bin/grav help <command>`         | Bietet Unterstützung bei einem bestimmten Befehl an.                                                                                              |
| `bin/grav new-project <location>` | Zur Erstellung einer neuen, sauberen Grav-Instanz in einem anderen Ordner. Kann von einer vorhandenen Grav-Installation aus gestartet werden.     |
| `bin/grav install`                | Dieser Befehl installiert alle Abhängigkeiten, die zur Ausführung Ihrer aktuellen Grav-Installation erforderlich sind.                            |
| `bin/grav clear-cache`            | Dieser Befehl löscht den Cache Ihrer Grav-Installation. Optionen sind unter anderem: `--all`, `--assets-only`, `--images-only` und `--cache-only` |
| `bin/grav backup`                 | Erstellt ein Zip-Backup Ihrer aktuellen Grav-Site.                                                                                                |
| `bin/grav composer`               | Aktualisierung manuell installierter, composer-basierter Anbieterpakete.                                                                          |
| `bin/gpm list`                    | Listet alle über den Grav-GPM (Grav Paket-Manager) verfügbaren Befehle auf.                                                                       |
| `bin/gpm help <command>`          | Bietet Unterstützung bei einem bestimmten GPM-Befehl an.                                                                                          |
| `bin/gpm index`                   | Zeigt eine Liste aller verfügbaren Ressourcen im Grav-Repository, nach Themes und Plugins geordnet.                                               |
| `bin/gpm info`                    | Zeigt die Details des gewünschten Pakets an, wie z.B. Beschreibung, Autor, Homepage usw.                                                          |
| `bin/gpm install`                 | Installiert mit einem einfachen Befehl eine Ressource aus dem Repository in Ihre aktuelle Grav-Instanz.                                           |
| `bin/gpm update`                  | Prüft installierte Plugins und Themes auf verfügbare Updates und listet diese auf.                                                                |
| `bin/gpm uninstall`               | Entfernt ein installiertes Theme oder Plugin und löscht den Cache.                                                                                |
| `bin/gpm self-upgrade`            | Hiermit können Sie Grav auf die neueste Version aktualisieren.                                                                                    |
| `bin/gpm security`                | Wird die konfigurierten XSS-Sicherheitsüberprüfungen auf allen Grav-Seiten durchführen.                                                           |
[/div]
[/version]


[version=16]
[div class="table table-keycol"]
| Befehl                            | Beschreibung                                                                                                                                      |
| :----------------                 | :--------------------------------------                                                                                                           |
| `bin/grav list`                   | Listet alle in Grav verfügbaren Befehle auf (außer dem GPM).                                                                                      |
| `bin/grav help <command>`         | Bietet Unterstützung bei einem bestimmten Befehl an.                                                                                              |
| `bin/grav new-project <location>` | Zur Erstellung einer neuen, sauberen Grav-Instanz in einem anderen Ordner. Kann von einer vorhandenen Grav-Installation aus gestartet werden.     |
| `bin/grav install`                | Dieser Befehl installiert alle Abhängigkeiten, die zur Ausführung Ihrer aktuellen Grav-Installation erforderlich sind.                            |
| `bin/grav clear-cache`            | Dieser Befehl löscht den Cache Ihrer Grav-Installation. Optionen sind unter anderem: `--all`, `--assets-only`, `--images-only` und `--cache-only` |
| `bin/grav backup`                 | Erstellt ein Zip-Backup Ihrer aktuellen Grav-Site.                                                                                                |
| `bin/grav composer`               | Aktualisierung manuell installierter, composer-basierter Anbieterpakete.                                                                          |
| `bin/gpm list`                    | Listet alle über den Grav-GPM (Grav Paket-Manager) verfügbaren Befehle auf.                                                                       |
| `bin/gpm help <command>`          | Bietet Unterstützung bei einem bestimmten GPM-Befehl an.                                                                                          |
| `bin/gpm index`                   | Zeigt eine Liste aller verfügbaren Ressourcen im Grav-Repository, nach Themes und Plugins geordnet.                                               |
| `bin/gpm info`                    | Zeigt die Details des gewünschten Pakets an, wie z.B. Beschreibung, Autor, Homepage usw.                                                          |
| `bin/gpm install`                 | Installiert mit einem einfachen Befehl eine Ressource aus dem Repository in Ihre aktuelle Grav-Instanz.                                           |
| `bin/gpm update`                  | Prüft installierte Plugins und Themes auf verfügbare Updates und listet diese auf.                                                                |
| `bin/gpm uninstall`               | Entfernt ein installiertes Theme oder Plugin und löscht den Cache.                                                                                |
| `bin/gpm self-upgrade`            | Hiermit können Sie Grav auf die neueste Version aktualisieren.                                                                                    |
| `bin/gpm security`                | Wird die konfigurierten XSS-Sicherheitsüberprüfungen auf allen Grav-Seiten durchführen.                                                           |
| `bin/gpm logviewer`               | Anzeige der Grav-Protokolldateien mit Optionen zur Wahl der Protokolldatei, Anzahl der Zeilen und Verbosität.                                     |
| `bin/gpm scheduler`               | Die geplanten Aufgaben verwalten und bei Bedarf den Scheduler-Prozess manuell starten.                                                            |
[/div]
[/version]


!! Diese Befehle werden in der Dokumentation zu [Grav-CLI](../grav-cli) und [Grav-GPM](../grav-cli-gpm) ausführlicher erläutert.

Die unten aufgeführten Befehle sind kompatibel mit <strong>Mac- oder Unix-Systemen</strong>.

[div class="table table-keycol"]
| Befehl                                   | Beschreibung                                                                                                              |
| :----------------                        | :--------------------------------------                                                                                   |
|  `bin/gpm index \| grep '\| installed'`  | Listet alle Plugins und Themes auf, die derzeit installiert sind.                                                         |
[/div]

## Symbolische Links

Symbolische Links (auch bekannt als Symlinks) sind unglaublich praktisch und innerhalb der Befehlszeile einfach einzusetzen. Dabei wird eine virtuelle Kopie (Klon) eines bestimmten Verzeichnisses oder seines Inhalts erstellt und an die gewünschte Stelle verschoben. Im Gegensatz zu einer echten Kopie ist es einfach ein Tunnel zum Original, so dass alles, was Sie sehen und ändern, an verschiedenen Stellen gleichzeitig gespiegelt wird.

Ein weiterer großer Vorteil dieser Vorgehensweise ist, dass damit praktisch kein zusätzlicher Speicherplatz benötigt wird. Denn es gibt keine Mehrfachkopien derselben Dateien.

In Zusammenhang mit Grav sind Symlinks eine großartige Möglichkeit, Plugins, Themen und Inhalte zu mehreren Instanzen hinzuzufügen, und zwar auf eine Weise, die das Aktualisieren und Modifizieren unendlich erleichtert. Sie nehmen eine Änderung einmal vor, und sie erscheint überall dort, wo die Datei(en) mit einem Symlink versehen sind.

Das Einrichten eines Symlinks ist unkompliziert, wobei es geringfügige Unterschiede zwischen den Betriebssystemen gibt.

### Symbolische Links in MacOS und Linux

![](osx_symlink.png)

Der Befehl folgt einem allgemeinen Schema: `ln -s <~/Pfad/zur/Original-Datei oder -Verzeichnis> <~/Pfad/zur/virtuellen_Kopie>`.

Die Befehle, die einen Symlink initiieren, unterscheiden sich von Betriebssystem zu Betriebssystem. Für MacOS und die Mehrheit der Unix- und Linux-Distributionen ist der Befehl `ln -s`. Der `ln` Teil teilt dem System mit, dass Sie einen Link erstellen wollen. Der `-s` Schalter definiert den Link auf symbolisch.

### Symbolische Links in Windows

Die prinzipielle Struktur des entsptechenden Befehls in Windows ist `mklink <Typ> <Laufwerk:\Pfad\zur\virtuellen_Kopie> <Laufwerk:\Pfad\zur\Original-Datei oder -Verzeichnis>`. Im Gegensatz zu MacOS oder Linux müssen Sie das Argument für den Typ der Datei angeben, die Sie symbolisch verknüpfen wollen. Die Reihenfolge von Quelle und Ziel wird außerdem umgekehrt angegeben, wobei der neue symbolische Link vor der zu verknüpfenden Datei steht. Es gibt drei Typ-Argumente, die hier verwendet werden können:

* `/j` – Dieses Argument wird am häufigsten verwendet. Es erzeugt einen Symlink auf ein Verzeichnis.
* `/h` – Damit wird ein symbolischer Link für eine konkrete Datei erzeugt.
* `/d` – Erzeugt einen Softlink oder eine Verknüpfung. Es ist unwahrscheinlich, dass es für die hier beschriebenen Zwecke verwendet wird.


### Beispiele für Befehle

Im Wesentlichen geben Sie den Befehl an, der den Symlink initiiert, was Sie symbolisch verlinken und wohin Sie die virtuellen Kopien einfügen. Nachstehend finden Sie detaillierte Beispiele für diese Befehle:

##### Verlinken des Inhalts eines Ordners zu einem anderen Ordner

[div class="table"]
| MacOS und Linux             | Windows                           |
| :-----                      | :-----                            |
| `ln -s ~/Ordner1 ~/Ordner2` | `mklink /J C:\Ordner2 C:\Ordner1` |
[/div]

Mit diesem Befehl wird ein Symlink erstellt, der Inhalte, die ursprünglich in **Ordner1** abgelegt sind, übernimmt und eine symbolisch verknüpfte Kopie davon in **Ordner2** ablegt. Wenn **Ordner2** noch nicht existiert, wird er mit diesem Befehl erstellt.

##### Komplette Ordner von einem Ort zum anderen verlinken

[div class="table"]
| MacOS und Linux              | Windows                            |
| :-----                       | :-----                             |
| `ln -s ~/Ordner1 ~/Ordner2/` | `mklink /J C:\Ordner2\ C:\Ordner1` |
[/div]

Mit diesem Befehl wird das gesamte Verzeichnis **Ordner1** kopiert und am Zielort (in diesem Fall **Ordner2**) abgelegt. In diesem Fall müsste **Ordner2** bereits existieren, da er mit diesem Befehl nicht erstellt wird.
Beachten Sie den einzelnen Slash respektive den Backslash am Ende bei der Angabe von **Ordner2**.

##### Einzelne Datei(en) von einem Ort zum anderen verlinken

[div class="table"]
| MacOS und Linux                      | Windows                                     |
| :-----                               | :-----                                      |
| `ln -s ~/Ordner1/file.jpg ~/Ordner2` | `mklink /H C:\Ordner2\ C:\Ordner1\file.jpg` |
[/div]

Dies ist ein praktischer Befehl, um einzelne Dateien symbolisch zu verlinken. Das ist besonders dann nützlich, wenn Sie Dateien haben, die von mehreren Ordnern gemeinsam genutzt werden und Sie möchten, dass sie überall gleichzeitig aktualisiert werden. Denken Sie daran, dass die Originaldatei die einzige tatsächliche Kopie ist und daher an ihrem ursprünglichen Ort verbleiben muss, damit alle symbolischen Links funktionieren.
