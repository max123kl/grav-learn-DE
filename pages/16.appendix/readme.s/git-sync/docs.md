---
title: Git Sync Plugin
taxonomy:
    category: docs
---

![](gitsync-logo.png)

Git Sync ist ein Plugin für das Grav-CMS, mit dem Sie ein Git-Repository nahtlos mit Ihrer Grav-Site synchronisieren können und umgekehrt.

Git Sync erfasst jede Änderung, die Sie auf Ihrer Website vornehmen und aktualisiert sofort Ihr Git-Repository. Auf die gleiche Weise unterstützt Git Sync Webhooks, wodurch Ihre Website automatisch synchronisiert werden kann, wenn sich das Repository ändert.

Dank dieses leistungsstarken bidirektionalen Flusses kann Git Sync Ihre Site jetzt in eine kollaborative Umgebung verwandeln, in der die „Quelle der Wahrheit“ immer Ihr Git-Repository ist und in der unbegrenzt viele Mitarbeiter und Sites denselben Inhalt gemeinsam nutzen und zu demselben Inhalt beitragen können.

## Videos: Setup und Demo

| In 2 Minuten eingerichtet und betriebsbereit | Demonstration der 2-Wege-Synchronisation |
| ------------ | ----------------- |
| [![Up and Running in 2 mins](https://img.youtube.com/vi/avcGP0FAzB8/0.jpg)](https://www.youtube.com/watch?v=avcGP0FAzB8) | [![2-way Sync Demonstration](https://img.youtube.com/vi/3fy78afacyw/0.jpg)](https://www.youtube.com/watch?v=3fy78afacyw) |

## Installation mit dem GPM (Grav Package Manager)

Um Git-Sync zu installieren, führen Sie einfach diesen Befehl aut dem Grav-Root-Ordner aus:
```
bin/gpm install git-sync
```

Nach der Installation des Plugins sollten Sie die Einstellungen des Plugins aufrufen, um die Wizard-Konfiguration zu starten.


## Features

![](wizard.png?width=500)

* Einfache Schritt-für-Schritt-Einrichtung Der Assistent führt Sie durch einen detaillierten Prozess zur Einrichtung der Funktionen
* Unterstützte Hosting-Dienste: [GitHub](https://github.com), [BitBucket](https://bitbucket.org), [GitLab](https://gitlab.com) sowie alle selbst gehosteten und Git-Dienste mit Webhooks-Unterstützung.
* Private Repositorys
* Synchronisiert jeden Ordner unter `user` (pages, themes, config)
* 2FA (Zwei-Faktor-Authentifizierung) und Access-Token Unterstützung
* Die Unterstützung von Webhooks ermöglicht die automatische Synchronisierung aus dem Git-Repository mit automatisch generierten sicheren Webhook-URLs und Unterstützung für Webhook Secret (sofern verfügbar)
* Automatisches Handling einfacher Merges im Hintergrund
* Mit nur einem Klick können Sie Ihre lokalen Änderungen zurücksetzen und den aktuellen Zustand des Git-Repositorys wiederherstellen
* Komfortable Ein-Klick-Schaltfläche für die manuelle Synchronisierung
* Unterstützung von Quick Tray im Admin-Bereich, so dass Sie von überall in der Administration synchronisieren können
* Möglichkeit zur Anpassung, ob GitSync nach dem Speichern oder nur manuell synchronisiert werden soll
* Anpassen des Committer-Namens, Auswahl zwischen Git-Benutzer, GitSync Commiter-Name, Grav-Benutzername und Grav-Benutzer Vollständiger Name
* Mit der eingebauten Aktion `gitsync` des Formularprozesses können Sie die Synchronisation jederzeit auslösen, wenn jemand einen Beitrag einreicht.
* Jedes Plugin von Drittanbietern kann sich in Git Sync integrieren und die Synchronisation durch das Event `gitsync` auslösen.
* Integrierte CLI-Befehle zur Automatisierung der Synchronisation.
* Protokollieren von jedem Befehl, der durch GitSync ausgeführt wird, um einen reibungslosen Ablauf zu gewährleisten oder potenzielle Probleme zu beheben

# Die Kommandozeile

Git Sync wird mit einem CLI geliefert, das die Ausführung von Synchronisationen direkt in im Terminal ermöglicht. Diese Funktion ist äußerst nützlich, wenn Sie einen autonomen periodischen Crontab-Job zur Synchronisierung mit Ihrem Repository ausführen möchten.

Zur Ausführung diesen Befehl einfach aufrufen:

```bash
bin/plugin git-sync sync
```

So können Sie einen Status erhalten:
```bash
bin/plugin git-sync status
```

Seit Version 2.1.1 können Sie jetzt auch Benutzer und Kennwort programmgesteuert über die Funktion `bin/plugin git-sync passwd` ändern. Das ist nützlich, wenn Sie einen Container haben, der Ihr Passwort zurücksetzt oder wenn Sie einige laufende Skripte haben, die eine programmgesteuertes Update des Passworts erfordern.

# System-Anforderungen

Damit das Plugin funktioniert, muss auf dem Server `git` 1.7.1 oder höher laufen.

Die PHP-Funktionen `exec()` und `escapeshellarg()` sind obligatorisch. Stellen Sie sicher, dass die Optionen in Ihrer PHP-Version aktiviert sind.

# Bekannte Probleme und Lösungsansätze
**Frage:** `error: The requested URL returned error: 403 Forbidden while accessing...` [#39](https://github.com/trilbymedia/grav-plugin-git-sync/issues/39)  
**Antwort:** Die Ursache dafür könnte sein, dass Ihr Computer einen Benutzer/Kennwort gespeichert hat, der mit dem von Ihnen beabsichtigten in Konflikt geraten könnte.  
[Folgen Sie den Anweisungen zur Lösung des Problems...](https://github.com/trilbymedia/grav-plugin-git-sync/issues/39#issuecomment-538867548)

# Gesponsert von

Dieses Plugin hätte ohne das Sponsoring von [Hibbitts Design](http://www.hibbittsdesign.org/blog/) und der Entwicklung von [Trilby Media](http://trilby.media) nicht realisiert werden können.
