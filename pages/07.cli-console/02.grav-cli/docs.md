---
title: Grav-Befehle
page-toc:
  active: true
taxonomy:
    category: docs
---

Grav verfügt über ein eingebautes Kommandozeilen-Interface (CLI), das unter `bin/grav` zu finden ist. Die CLI ist äußerst praktisch für das Ausführen von wiederkehrenden Aufgaben wie **Leeren des Caches**, Erstellen von **Sicherungen** und mehr.

Der Zugriff auf die CLI ist ein einfacher Vorgang, aber Sie müssen ein **Terminal** verwenden. Auf einem Mac heißt das `Terminal`, unter Windows heißt es `cmd` und unter Linux ist es eine `Shell`. Befehle im UNIX-Stil sind in Windows cmd nicht nativ verfügbar. Durch die Installation des Pakets [msysgit](http://msysgit.github.io/) auf einem Windows-Rechner werden [Git](https://git-scm.com/) und Git BASH hinzugefügt, eine alternative Eingabeaufforderung, die UNIX-Befehle verfügbar macht. Wenn Sie Ihren Server per Fernzugriff erreichen, werden Sie höchstwahrscheinlich **SSH** verwenden, um sich per Fern-Anmeldung bei Ihrem Server einzuloggen. Schauen Sie sich dieses hervorragende [Tutorial zum Thema SSH](http://code.tutsplus.com/tutorials/ssh-what-and-how--net-25138) an, um genauere Informationen über SSH zu erhalten.

Obwohl einige Operationen manuell ausgeführt werden können, indem man sich auf die CLI _verlässt_. Diese Aufgaben könnten allerdings über _cronjobs_ automatisiert werden, die täglich ausgeführt werden.

Um eine Liste aller in Grav verfügbaren Befehle zu erhalten, können Sie diesen Befehl ausführen:

[prism classes="language-bash command-line"]
bin/grav list
[/prism]

Das dürfte dann ungefähr so aussehen:

[version=15]
[prism classes="language-text"]
Available commands:
  backup       Creates a backup of the Grav instance
  clean        Handles cleaning chores for Grav distribution
  clear-cache  [clearcache] Clears Grav cache
  composer     Updates the composer vendor dependencies needed by Grav.
  help         Displays help for a command
  install      Installs the dependencies needed by Grav. Optionally can create symbolic links
  list         Lists commands
  new-project  [newproject] Creates a new Grav project with all the dependencies installed
  sandbox      Setup of a base Grav system in your webroot, good for development, playing around or starting fresh
  security     Capable of running various Security checks
[/prism]
[/version]

[version=16]
[prism classes="language-text"]
Available commands:
  backup       Creates a backup of the Grav instance
  cache        [clearcache|cache-clear] Clears Grav cache
  clean        Handles cleaning chores for Grav distribution
  composer     Updates the composer vendor dependencies needed by Grav.
  help         Displays help for a command
  install      Installs the dependencies needed by Grav. Optionally can create symbolic links
  list         Lists commands
  logviewer    Display the last few entries of Grav log
  new-project  [newproject] Creates a new Grav project with all the dependencies installed
  sandbox      Setup of a base Grav system in your webroot, good for development, playing around or starting fresh
  scheduler    Run the Grav Scheduler.  Best when integrated with system cron
  security     Capable of running various Security checks
[/prism]
[/version]

Um Hilfe zu einem bestimmten Befehl zu erhalten, können Sie dem Befehl `help` voranstellen:

[prism classes="language-bash command-line"]
bin/grav help install
[/prism]

## Backup

[version=15]
Die Sicherung Ihres Projekts ist nichts anderes als die Erstellung eines Archivs der _ROOT_ von Grav. Keine Datenbank, keine Komplikationen.

Natürlich können Sie diese Aufgabe noch weiter vereinfachen, indem Sie einfach die Grav-CLI verwenden. Angenommen, wir haben unser `~/workspace/portfolio` Projekt und wollen ein Backup davon erstellen, dann werden wir folgendes tun:

[prism classes="language-bash command-line"]
cd ~/workspace/portfolio
bin/grav backup
[/prism]

Eine neue Sicherungsdatei `portfolio-20140812174352.zip` wurde im Ordner `backup/` des Projekts erstellt. Die lange Zahl hinter dem Namen ist nur das Datum im Format _Jahr Monat Tag Stunde Minute Sekunde_.

[/version]
[version=16]
Das Grav-Backup-System wurde in Grav 1.6 vollständig überarbeitet, um mehrere Backup-Profile zu unterstützen.  Diese Profile werden in der Datei `user/config/backups.yaml` konfiguriert.  Wenn Sie keine benutzerdefinierte Konfigurationsdatei haben, verwendet Grav die Standardkonfigurationsdatei aus `system/config/backups.yaml`.

Wenn Grav mehrere Sicherungsprofile erkennt, werden Sie vom CLI-Befehl gefragt, welches Sie mit dem CLI-Befehl sichern möchten.

[prism classes="language-bash command-line" cl-output="2-10"]
cd ~/workspace/portfolio
bin/grav backup

Grav Backup
===========

Choose a backup?
  [0] Default Site Backup
  [1] Pages Backup
[/prism]

Alternativ können Sie einen Profil-Index direkt mitgeben:

[prism classes="language-bash command-line" cl-output="2-10"]
$ cd ~/workspace/portfolio
bin/grav backup 1

Archiving 36 files [===================================================] 100% < 1 sec Done...

 [OK] Backup Successfully Created: /users/joe/workspace/portfolio/backup/pages_backup--20190227120510.zip
[/prism]

Weiterführende Informationen über die Backup-Funktion finden Sie im Abschnitt [Erweitert -> Backups](/advanced/backups).
[/version]

## Clean

Dieser CLI-Befehl wird in erster Linie während des Erstellens von Paketen verwendet, da er überflüssige Dateien und Ordner aus Grav entfernt.  Es wird dringend empfohlen, diesen Befehl **nicht selbst zu benutzen**, es sei denn, Sie verwenden ihn zum Generieren Ihrer eigenen Grav-Pakete.

[prism classes="language-bash command-line"]
bin/grav clean
[/prism]

## Clear-Cache

Sie können den Cache löschen, indem Sie alle Dateien und Ordner unterhalb von `cache/` löschen.

Der entsprechende CLI-Befehl lautet:

[version=15]
[prism classes="language-bash command-line"]
$ cd ~/webroot/my-grav-project
bin/grav clear-cache
[/prism]

Die Kompatibilität ist anhand mehrerer Aliasnamen gewährleistet (`clear-cache`, `clearcache`, `clear`).

Die Voreinstellung ist der normale Cache-Löschvorgang, Sie können diesen Prozess mit folgenden Optionen weiter steuern:

[prism classes="language-text"]
--all             Falls angegeben, werden alle, einschließlich kompilierter, Twig- und Doktrin-Caches, entfernt.
--assets-only     Falls angegeben, wird nur assets/* entfernt.
--images-only     Falls angegeben, wird nur images/* entfernt.
--cache-only      Falls angegeben, wird nur cache/* entfernt.
--tmp-only        Falls angegeben, wird nur /* entfernt.
[/prism]
[/version]

[version=16]
[prism classes="language-bash command-line"]
$ cd ~/webroot/my-grav-project
bin/grav cache
[/prism]

Die Kompatibilität ist anhand mehrerer Aliasnamen gewährleistet (`cache`, `cache-clear`, `clearcache`, `clear`).

Die Voreinstellung ist der normale Cache-Löschvorgang, Sie können diesen Prozess mit folgenden Optionen weiter steuern:

[prism classes="language-text"]
--purge           Falls angegeben, werden alte Caches gelöscht
--all             Falls angegeben, werden alle, einschließlich kompilierter, Twig- und Doktrin-Caches, entfernt.
--assets-only     Falls angegeben, wird nur assets/* entfernt.
--images-only     Falls angegeben, wird nur images/* entfernt.
--cache-only      Falls angegeben, wird nur cache/* entfernt.
--tmp-only        Falls angegeben, wird nur tmp/* entfernt.
[/prism]
[/version]

## Composer

Wenn Sie Grav über GitHub installiert haben und manuell composer-basierte Anbieterpakete eingespielt haben, können Sie problemlos so updaten:

[prism classes="language-bash command-line"]
bin/grav composer
[/prism]

Sie können auch Composer-Optionen wie `install` übergeben:

[prism classes="language-bash command-line"]
bin/grav composer --install
[/prism]

oder

[prism classes="language-bash command-line"]
bin/grav composer --update
[/prism]

!! Diese Befehle verwenden alle die `--no-dev` Composer-Option. Um also Tests durchführen zu können, sollten Sie den composer direkt verwenden: `bin/composer.phar`

## Install

Um die Abhängigkeiten zu installieren, auf die Grav angewiesen ist (**error**-Plugin, **problems**-Plugin, **antimatter**-Theme), starten Sie ein **Terminal** oder eine **Konsole** und navigieren Sie zu dem Ordner grav, in dem Sie die Abhängigkeiten installieren möchten, und führen Sie den folgenden CLI-Befehl aus:

[prism classes="language-bash command-line"]
$ cd ~/webroot/my-grav-project
bin/grav install
[/prism]

Die Abhängigkeiten sollten nun installiert sein unter:
* `~/webroot/my-grav-project/user/plugins/error`
* `~/webroot/my-grav-project/user/plugins/problems`
* `~/webroot/my-grav-project/user/themes/antimatter`

[version=16]
## Log Viewer

Als Teil von Grav 1.6 wurde ein neuer CLI-Befehl zur schnellen Anzeige von Grav-Protokollen entwickelt.

Der einfachste Weg, diesen Befehl zu verwenden, ist die direkte Eingabe:

[prism classes="language-bash command-line"]
cd ~/webroot/my-grav-project
bin/grav logviewer
[/prism]

Dadurch werden die letzten 20 Logbuch-Einträge der Datei `logs/grav.log` ausgegeben.  Dafür existieren einige Optionen:

[prism classes="language-text line-numbers"]
-f, --file[=FILE]     benutzerdefinierte Log-Datei (default = grav.log)
-l, --lines[=LINES]   Anzahl der Zeilen (default = 10)
-v, --verbose         ausführlichere Darstellung einschließlich einer Stack-Auswertung, falls verfügbar
[/prism]

z.B.

[prism classes="language-bash command-line" cl-output="2-11"]
bin/grav logviewer --lines=4                                                                           [12:27:20]

Log Viewer
==========

viewing last 4 entries in grav.log

2019-02-27 12:00:30 [WARNING] Plugin 'foo-plugin' enabled but not found! Try clearing cache with `bin/grav cache`
2019-02-27 12:04:57 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/default_site_backup--20190227120450.zip
2019-02-27 12:05:10 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/pages_backup--20190227120510.zip
2019-02-27 12:26:00 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/pages_backup--20190227122600.zip
[/prism]
[/version]

Und hier eine ausführliche Ausgabe mit Stack-Auswertung:

[prism classes="language-bash command-line" cl-output="2-22"]
bin/grav logviewer -v                                                                                                       [16:12:12]

Log Viewer
==========

viewing last 20 entries in grav.log

2019-03-14 05:52:44 [WARNING] Plugin 'simplesearch.bak' enabled but not found! Try clearing cache with `bin/grav clear-cache`
2019-03-14 05:52:44 [CRITICAL] A function must be an instance of \Twig_FunctionInterface or \Twig_SimpleFunction.
0 /Users/joe/my-grav-project/plugins/acme-twig-filters/acme-twig-filters.php(52): Twig\Environment->addFunction(Object(Twig\TwigFilter))
1 /Users/joe/my-grav-project/vendor/symfony/event-dispatcher/EventDispatcher.php(212): Grav\Plugin\ACMETwigFiltersPlugin->onTwigInitialized(Object(RocketTheme\Toolbox\Event\Event), 'onTwigInitializ...', Object(RocketTheme\Toolbox\Event\EventDispatcher))
2 /Users/joe/my-grav-project/vendor/symfony/event-dispatcher/EventDispatcher.php(44): Symfony\Component\EventDispatcher\EventDispatcher->doDispatch(Array, 'onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
3 /Users/joe/my-grav-project/vendor/rockettheme/toolbox/Event/src/EventDispatcher.php(23): Symfony\Component\EventDispatcher\EventDispatcher->dispatch('onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
4 /Users/joe/my-grav-project/system/src/Grav/Common/Grav.php(365): RocketTheme\Toolbox\Event\EventDispatcher->dispatch('onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
5 /Users/joe/my-grav-project/system/src/Grav/Common/Twig/Twig.php(175): Grav\Common\Grav->fireEvent('onTwigInitializ...')
6 /Users/joe/my-grav-project/system/src/Grav/Common/Processors/TwigProcessor.php(24): Grav\Common\Twig\Twig->init()
7 /Users/joe/my-grav-project/system/src/Grav/Framework/RequestHandler/Traits/RequestHandlerTrait.php(45): Grav\Common\Processors\TwigProcessor->process(Object(Nyholm\Psr7\ServerRequest), Object(Grav\Framework\RequestHandler\RequestHandler))
8 /Users/joe/my-grav-project/system/src/Grav/Framework/RequestHandler/Traits/RequestHandlerTrait.php(57): Grav\Framework\RequestHandler\RequestHandler->handle(Object(Nyholm\Psr7\ServerRequest))
9 /Users/joe/my-grav-project/system/src/Grav/Common/Processors/AssetsProcessor.php(28): Grav\Framework\RequestHandler\RequestHandler->handle(Object(Nyholm\Psr7\ServerRequest))

2019-03-14 05:52:46 [WARNING] Plugin 'simplesearch.bak' enabled but not found! Try clearing cache with `bin/grav clear-cache`
...
[/prism]

## New Project

Jedes Mal, wenn Sie ein neues Projekt mit Grav starten möchten, müssen Sie mit einer sauberen Grav-Instanz beginnen. Durch die CLI ist dieser Vorgang supereinfach und dauert nur wenige Sekunden.

1. Starten Sie ein **Terminal** oder eine **Konsole** und navigieren Sie zum Ordner _grav_ (für dieses Dokument nehmen wir an, dass es sich unter `~/Projekte/grav` befindet)

[prism classes="language-bash command-line"]
cd ~/Projects/grav
[/prism]

2. Führen Sie die Grav-CLI aus, um ein neues Projekt zu erstellen, wobei das Ziel der Ort ist, an dem sich Ihr Projekt befinden wird (normalerweise die [Webroot](http://en.wikipedia.org/wiki/Webroot) Ihres Webservers). Nehmen wir an, wir erstellen ein **portfolio** und wollen es unter `~/Webroot/portfolio` erstellen.

[prism classes="language-bash command-line"]
bin/grav new-project ~/webroot/portfolio
[/prism]

Dadurch wird eine neue Grav-Instanz erstellt und alle erforderlichen Abhängigkeiten heruntergeladen.

## Sandbox

Grav verfügt über ein raffiniertes Dienstprogramm namens `sandbox`, mit dem schnell eine mit [Symlinks](/cli-console/command-line-intro#symbolic-links) versehene Kopie der Grav-Installation erstellt werden kann. Einfach ausgedrückt: Wenn Sie `bin/grav sandbox -s ZIEL` aufrufen, wobei „ZIEL“ der Pfad zu dem Ordner ist, in dem Sie die kopierte Installation haben möchten, wird die Grav-Installation in dem anderen Ordner neu erstellt.

Beispielsweise der Aufruf:

[prism classes="language-bash command-line"]
bin/grav sandbox -s ../copy
[/prism]

Erstellt von Ihrem aktuellen Grav-Ordner ein Schwesterverzeichnis namens `copy`, wobei die folgenden Verzeichnisse virtuelle Kopien sind: `/bin, /system, /vendor, /webserver-configs`. Dazu kommen die Standard-Dateien, die sich typischerweise im Wurzelverzeichnis von Grav befinden. Alle Inhalte in /user sind nicht virtuelle, sondern Carbon-Kopien, so dass Sie einfach mit der Anpassung der neuen Installation beginnen können, ohne einen Overhead von Core-Dateien erzeugt zu haben.

[version=16]
## Scheduler

Wie im Abschnitt [Erweitert -> Scheduler](/advanced/scheduler) beschrieben, kann der Scheduler über den CLI-Befehl überwacht werden.

Der Basisbefehl führt die fälligen Scheduler-Aufgaben manuell aus:

[prism classes="language-bash command-line"]
bin/grav scheduler
[/prism]

Um mehr Details zu erhalten, können Sie die Option `-v` verwenden:

[prism classes="language-bash command-line" cl-output="2-10"]
bin/grav scheduler -v

Running Scheduled Jobs
======================

[2019-02-27T12:34:07-07:00] Success: Grav\Common\Cache::purgeJob
[2019-02-27T12:34:07-07:00] Success: Grav\Common\Cache::clearJob
[2019-02-27T12:34:07-07:00] Success: ls -lah
[/prism]

Weitere Optionen sind:

[prism classes="language-text line-numbers"]
-i, --install         Show Install Command
-j, --jobs            Show Jobs Summary
-d, --details         Show Job Details
[/prism]

Ausführlichere Informationen zu diesen Optionen finden Sie im Abschnitt [Erweitert -> Scheduler](/advanced/scheduler).
[/version]

## Security

Added in Grav 1.5 is a new security scanner CLI command.  You can run this to quickly scan your contents against the [configured security settings](/basics/grav-configuration#security).

[prism classes="language-bash command-line" cl-output="2-10"]
bin/grav security                                                                                       [12:34:12]

Grav Security Check
===================

Scanning 11 pages [===================================================] 100% < 1 sec

[OK] Security Scan complete: No issues found...
[/prism]

#### PHP CGI-FCGI Information

Um festzustellen, ob Ihr Server `cgi-fcgi` auf der Kommandozeile ausführt, geben Sie Folgendes ein:

[prism classes="language-bash command-line" cl-output="2-5"]
$ php -v
PHP 5.5.17 (cgi-fcgi) (built: Sep 19 2014 09:49:55)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
    with the ionCube PHP Loader v4.6.1, Copyright (c) 2002-2014, by ionCube Ltd.
[/prism]

Wenn Sie einen Verweis auf `(cgi-fcgi)` sehen, müssen Sie allen `bin/grav`-Befehlen `php-cli` voranstellen. Alternativ können Sie einen Alias in Ihrer Shell einrichten, ungefähr wie folgt: `alias php="php-cli"`. Dadurch wird sichergestellt, dass die **CLI** PHP-Version auf der Kommandozeile läuft.
