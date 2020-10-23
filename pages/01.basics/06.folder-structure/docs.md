---
title: Ordnerstruktur
taxonomy:
    category: docs
---

Da Grav ein auf **Flat-Files basierendes CMS** ist, bedeutet das, dass es von keiner Datenbank begleitet wird. Daher ist die Ordnerstruktur Ihrer Website sehr wichtig.  Auf der **obersten Ebene** Ihrer Grav-Installation sieht die Ordnerstruktur wie folgt aus:

[prism classes="language-bash line-numbers"]
/assets
/backup
/bin
/cache
/images
/logs
/system
/tmp
/vendor
/user
[/prism]

Lassen Sie uns daher ein wenig tiefer in die einzelnen Top-Level-Ordner eintauchen und ihren Zweck erklären:

### /assets

Der Ordner `assets` wird vom neuen Asset-Management-System innerhalb von Grav zur Speicherung der aufbereiteten `.css'- und `.js'-Dateien verwendet.

!! Dieser Ordner sollte nicht zum Speichern von Nutzerdaten verwendet werden, da er routinemäßig von allen Daten bereinigt wird.

### /backup

Der Ordner `backup` ist der Standardspeicherort für die Grav-Backups.

### /bin

Der Ordner `bin` enthält die [Grav CLI-Software](../../cli-console/grav-cli), mit der einige praktische Aufgaben über die Befehlszeile ausgeführt werden können. Dies ist eine relativ fortgeschrittene Funktion, die in erster Linie für Entwickler gedacht ist, daher werden wir dieses Thema erst zu einem späteren Zeitpunkt behandeln.

### /cache

Im Ordner `cache` werden temporäre Cache-Dateien abgelegt, die zur Leistungssteigerung automatisch von Grav generiert werden.  Standardmäßig übernimmt Grav das Caching automatisch und wählt die beste verfügbare Option für Ihre Hosting-Umgebung aus. Auf diese Weise stellen wir sicher, dass Ihre Website so schnell wie möglich funktioniert.

Wenn Grav feststellt, dass das **Dateisystem** die beste Caching-Methode ist, werden die von ihm erzeugten Cache-Dateien hier gespeichert.  Die Twig-Template-Engine verwendet diesen Ort auch, um ihre vorkompilierten Template-Dateien zu speichern.  Auch hier soll die optimale Geschwindigkeit von Grav erreicht werden.

!! Dieser Ordner sollte nicht zum Speichern von Nutzerdaten verwendet werden, da er routinemäßig von allen Daten bereinigt wird.

### /images

Grav ist mit einer integrierten, leistungsstarken und dennoch sehr einfach zu bedienenden [Bibliothek zur Bildbearbeitung](../../content/media) ausgestattet. So können Sie leicht die Größe eines Bildes in Echtzeit aus Ihrem Inhalt oder sogar aus einem Plugin heraus ändern. Diese Bilder werden im Ordner `images` gespeichert, so dass sie wiederverwendet werden können, wann das gleiche Bild mit der gleichen Größe erneut angefordert wird.

Dieser Ordner fungiert wie ein Bilder-Cache und ist für automatisch generierte Dateien vorgesehen.  Vom Benutzer bereitgestellte Medien sollten im Ordner `user/pages/`, `user/themes/` oder auch in einem benutzerdefinierten Ordner `user/images/` hinterlegt werden.

!! Dieser Ordner sollte nicht zum Speichern von Nutzerdaten verwendet werden, da er routinemäßig von allen Daten bereinigt wird.

### /logs

Wenn Grav ein Problem entdeckt oder wenn Sie eine extra Protokollierung oder Profilerstellung aktiviert haben, speichert Grav die entsprechenden Protokolldateien im Ordner `logs`.

### /system

Der Ordner `system` ist der Bereich, in dem die Dateien, die Grav zum Funktionieren verhelfen.  Sie sollten in diesem Ordner nichts bearbeiten, da ein Update der Grav-Software die Änderungen überschreiben würde.  Wenn Sie etwas ändern müssen, das mit der Funktionsweise von Grav zu tun hat, können Sie die Plugins benutzen, die in späteren Kapiteln besprochen werden.

### /tmp

The `tmp` folder is used by Grav and plugins to store temporary files.

!! This folder should not be used to store any user data, as it is routinely flushed of all data.

### /vendor

Der Ordner `vendor` enthält wichtige Bibliotheken, auf die sich Grav stützt.  Dieser Ordner ist dem Ordner `system` dahingehend ähnlich, dass sein Inhalt nicht verändert werden sollte, es sei denn, Sie sind sich absolut sicher, was Sie tun.

Wenn Sie Grav aus GitHub heraus [installiert](../installation) haben, wurde der Ordner `vendor` nicht mit installiert. Um diesen Ordner zu erstellen und zu befüllen, müssen Sie `bin/grav install` oder `composer install` aus dem Stammverzeichnis Ihrer Grav-Instanz ausführen. Weitere Einzelheiten finden Sie im Abschnitt [Installation](../installation).

### /user

Für die meisten Grav-Anwender ist dieser Ordner der Wichtigste. In diesem Ordner werden Sie Ihre Arbeitszeit damit verbringen, Inhalte zu erstellen, Plugins einzusetzen und Ihre Themes zu bearbeiten. Lassen Sie uns ein wenig tiefer in diesen Ordner hineinschauen:

[prism classes="language-bash line-numbers"]
/user/accounts
/user/blueprints
/user/config
/user/data
/user/images
/user/languages
/user/pages
/user/plugins
/user/themes
[/prism]

### /user/accounts

Im Ordner `accounts` definieren Sie die Benutzer-Accounts. Hier können Sie festlegen, ob für bestimmte Bereiche Ihrer Website Zugangsbeschränkungen erforderlich sind.

### /user/blueprints

Der Ordner `blueprints` enthält Ihre benutzerdefinierten Konzepte für die Website.

### /user/config

Die [Dateien im config-Ordner](../grav-configuration) dienen zur Konfiguration der Website und wurden im vorigen Kapitel besprochen.

### /user/data

Der Ordner `data` kann von Plugins verwendet werden, um darin einige Daten zu speichern, auf die Sie später zurückgreifen können.  Ein gutes Beispiel für ein Plugin, das diesen Ordner verwendet, ist das **Forms-Plugin**, das ein Web-Formular verwenden und die übermittelten Daten in einer Textdatei in diesem Ordner speichern kann.  Sie können dort auch andere Objekte wie Benutzer-Uploads oder irgendetwas anderes, das Sie wirklich brauchen, abspeichern.

!! Dieser Ordner ist normalerweise nicht von einem Browser zu sehen.

### /user/images

Der Ordner `images` kann zum Speichern Ihrer Bilder verwendet werden. Auf ihn kann mit Hilfe des Streams `image://` zugegriffen werden.


### /user/languages

Der Ordner `languages` enthält die [Übersetzung-Overrides](../../content/multi-language#Übersetzung-Overrides).

### /user/pages

Dies ist das eigentliche Zentrum von Grav. Der Ordner `pages` ist der Ort, an dem Sie Ihren Content erstellen und bearbeiten.  Wir werden im [nächsten Kapitel](../../content) noch viel detaillierter darauf eingehen.

### /user/plugins

Ein Plugin kann die leistungsfähige Grav-Software um weitere Funktionen ergänzen, die Sie möglicherweise für Ihre Website benötigen. Plugins können von [GetGrav.org/downloads/plugins](https://getgrav.org/downloads/plugins) heruntergeladen werden oder Sie [entwickeln Ihr eigenes Plugin](../../plugins/plugin-tutorial).

### /user/themes

Ein Theme verwandelt Ihren Inhalt in eine echte Website. Es wandelt den von Ihnen erstellten Inhalt in das HTML-Format um, das ein Browser versteht und Ihren Besuchern anzeigt. Es gibt ein Standardtheme, das Sie zusammen mit Grav erhalten. Sie können andere Themes von [GetGrav.org/downloads/themes](https://getgrav.org/downloads/themes) herunterladen oder auch Ihr eigenes Theme erstellen. Im Kapitel [Themes](../../themes) wird darauf näher eingegangen.
