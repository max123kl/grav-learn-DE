---
title: Installation
taxonomy:
    category: docs
---

Die Installation von Grav ist ein einfacher Prozess. Eigentlich gibt es keine wirkliche Installation. Sie haben **drei** Optionen zur Installation von Grav. Die erste – und einfachste – Methode besteht darin, das **Zip-Archiv** herunterzuladen und es zu entpacken. Die zweite Art ist die Installation mit dem **Composer**. Die dritte Variante besteht darin, das Quellprojekt direkt aus **GitHub** zu klonen und dann einen enthaltenen Skriptbefehl auszuführen, um die benötigten Abhängigkeiten zu installieren:

## Die PHP-Version kontrollieren

Grav ist außerordentlich einfach einzurichten und zum Laufen zu bringen. Vergewissern Sie sich, dass Sie mindestens die PHP-Version [version=15]5.6.3+[/version][version=16]7.1.3+[/version] verwenden und geben Sie auf dem Terminal `php -v` ein:

[prism classes="language-bash command-line" cl-output="2-10"]
php -v
PHP 7.2.15 (cli) (built: Feb  7 2019 20:10:03) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.15, Copyright (c) 1999-2018, by Zend Technologies
[/prism]


## Option 1: Installation aus einem ZIP-Archiv

Am einfachsten lässt sich Grav installieren, indem man das ZIP-Paket herunterlädt und entpackt:

1. Laden Sie das neueste und leistungsfähigste **[Grav](https://getgrav.org/download/core/grav/latest)** oder **[Grav + Admin](https://getgrav.org/download/core/grav-admin/latest)** Paket herunter.
2. Extract the ZIP file in the [webroot](https://www.wordnik.com/words/webroot) of your web server, e.g. `~/webroot/grav`

!!! There are [Skeleton](https://getgrav.org/downloads/skeletons)-packages available, which include the core Grav system, sample pages, plugins, and configuration. They are a great way to get started; all you have to do is [download the Skeleton](https://getgrav.org/downloads/skeletons)-package you prefer, and follow the steps above.

!!!! Wenn Sie die ZIP-Datei heruntergeladen haben und dann planen, sie in Ihr Webroot zu verschieben, verschieben Sie bitte den **vollständigen Ordner**, da er mehrere versteckte Dateien (wie .htaccess) enthält, die nicht automatisch ausgewählt werden. Das Weglassen dieser versteckten Dateien kann Probleme beim Ausführen von Grav verursachen.


## Option 2: Mit dem Composer installieren

Die andere Methode ist die Installation von Grav mit dem  [Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx):

[prism classes="language-bash command-line"]
composer create-project getgrav/grav ~/webroot/grav
[/prism]

Wenn Sie die Entwickler-Version von Grav ausprobieren möchten, fügen Sie `1.x-dev` als zusätzlichen Parameter hinzu:

[prism classes="language-bash command-line"]
composer create-project getgrav/grav ~/webroot/grav 1.x-dev
[/prism]

## Option 3: Aus GitHub installieren

Eine weitere Methode ist das Klonen von Grav aus dem GitHub-Repository und das anschließende Ausführen eines einfachen Installationsskripts um die Abhängigkeiten anzupassen:

1. Sie können das Grav-Repository von [GitHub](https://github.com/getgrav/grav) in einen Ordner im Webroot Ihres Servers klonen, z.B. `~/<webroot>/grav`. Starten Sie ein **Terminal** oder eine **Konsole** und navigieren Sie zum Webroot-Ordner:

   [prism classes="language-bash command-line"]
   cd ~/webroot
   git clone -b master https://github.com/getgrav/grav.git
   [/prism]

2. Installieren Sie die **Vendor-Abhängigkeiten** über den [Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx):

   [prism classes="language-bash command-line"]
   cd ~/webroot/grav
   composer install --no-dev -o
   [/prism]

3. Installieren Sie die **Plugin-** und **Theme-Abhängigkeiten** mit Hilfe der [Grav-CLI-Anwendung](../../advanced/grav-cli) `bin/grav`:

   [prism classes="language-bash command-line"]
   cd ~/webroot/grav
   bin/grav install
   [/prism]

   Dadurch werden automatisch die erforderlichen Abhängigkeiten von GitHub direkt in diese Grav-Installation **geklont**.

## Der Webserver

#### Apache, IIS oder Nginx

Die Verwendung von Grav mit einem Webserver wie Apache, IIS oder Nginx ist so einfach wie das Extrahieren von Grav in einen Ordner unterhalb der [Webroot](https://www.wordnik.com/words/webroot). Alles, was zum Funktionieren erforderlich ist, ist [version=15]PHP 5.6.3[/version][version=16]PHP 7.1.3[/version] oder höher. Sie sollten also überprüfen, ob Ihre Serverinstallation diese Anforderung erfüllt. Weitere Informationen zu den Grav-Anforderungen finden Sie im Kapitel über die [Systemanforderungen](../requirements) in diesem Handbuch.

Wenn Ihre Webroot z.B. `~/public_html` ist, können Sie die Datei in dieses Verzeichnis extrahieren, das sie über `http://localhost` erreichen. Wenn Sie es in `~/public_html/grav` extrahieren würden, könnten Sie es über `http://localhost/grav` aufrufen.

!!! Jeder Webserver muss konfiguriert werden. Grav wird mit einer . htaccess-Datei für Apache ausgeliefert und kommt mit einigen [standardmäßigen Server-Konfigurationsdateien](https://github.com/getgrav/grav/tree/master/webserver-configs) für `nginx`, `caddy server`, `iis`, und `lighttpd`. Benutzen Sie diese je nach Notwendigkeit entsprechend.

#### Grav mit dem integrierten PHP-Webserver mit `router.php` ausführen

Sie können Grav mit einem einfachen Befehl aus dem Terminal bzw. der Eingabeaufforderung über den integrierten PHP-Server ausführen, der auf jedem System mit [version=15]PHP 5.6.3+[/version][version=16]PHP 7.1.3+[/version] installiert ist. Alles, was Sie tun müssen, ist mit dem Terminal oder der Eingabeaufforderung zum Stammverzeichnis Ihrer Grav-Installation zu navigieren und `php -S localhost:8000 system/router.php` einzugeben. Sie können die Portnummer, in unserem Beispiel ist es die `8000`, durch einen beliebigen Port ersetzen.

Wenn Sie diesen Befehl eingeben, erhalten Sie eine ähnliche Ausgabe wie die folgende:

[prism classes="language-bash command-line" cl-output="2-10"]
php -S localhost:8000 system/router.php
PHP 7.2.15 Development Server started at Sun Feb 17 21:02:14 2019
Listening on http://localhost:8000
Document root is /Users/rhuk/Projects/grav/grav
Press Ctrl-C to quit.
[/prism]

Ihr Terminal informiert Sie auch in Echtzeit über alle Aktivitäten auf diesem Ad-hoc-Server. Sie können die URL, die in der `Listening on` Zeile angegeben ist, kopieren und in den Browser Ihrer Wahl einfügen, um auf die Website zuzugreifen, inklusive des Administrators.

!!!! Dieses Werkzeug ist nützlich für eine zügige Entwicklung und sollte **nicht** an Stelle eines dedizierten Webservers wie Apache verwendet werden.

## Erfolgreiche Installation

Beim ersten Starten von Grav werden einige Dateien im Voraus kompiliert. Wenn Sie nun Ihren Browser aktualisieren, dann erhalten Sie eine schnellere, im Cache gespeicherte Version.

![Grav Installed](install.png)

!! In den vorherigen Beispielen steht **$** für die Eingabeaufforderung. Je nach Betriebssystem kann diese unterschiedlich aussehen.

Standardmässig wird Grav mit einigen Beispielseiten geliefert, um Ihnen einen ersten Eindruck zu vermitteln. Ihre Website ist bereits vollständig eingerichtet und Sie können sie konfigurieren, Inhalte hinzufügen, erweitern oder anpassen, so weit Sie es wollen.

## Probleme bei Installation und Einrichtung

Wenn beim Laden der ersten Seite (oder nach einem Cache-Löschen Vorgang) irgendwelche Probleme entdeckt auftreten, wird möglicherweise eine Fehlerseite angezeigt:

![Grav with Problems](problems.png)

Bitte schauen Sie im Kapitel [Fehlerbehebung](../../troubleshooting) für weitere Informationen zu spezifischen Problemen nach.

! Wenn Sie Probleme mit Dateiberechtigungen haben, lesen Sie bitte die Dokumentation zur [Fehlerbehebung bei Berechtigungen](/troubleshooting/permissions). Sie können sich auch die Dokumentation zu den [Hosting-Leitfäden](/webservers-hosting) ansehen, die spezifische Anweisungen für verschiedene Hosting-Umgebungen enthält.

## Grav Updates

### Automatische Updates

Die bevorzugte Methode zur Aktualisierung von Grav ist die Verwendung des **Grav-Paketmanagers (GPM)**. Alles, was Sie tun müssen, ist zur Root Ihrer Grav-Site zu navigieren und diesen Befehl einzugeben:

[prism classes="language-bash command-line"]
bin/gpm selfupgrade -f
[/prism]

Vollständige Informationen finden Sie in der [Grav GPM-Dokumentation](../../advanced/grav-gpm). Wir haben GPM auch in unser [Admin-Panel-Plugin](../../admin-panel) integriert, das alle Aktualisierungen prüft, vorschlägt und automatisch installiert.
