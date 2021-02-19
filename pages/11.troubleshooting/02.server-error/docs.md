---
title: Grav Server Error
taxonomy:
    category: docs
---

![](grav-server-error.png?classes=center)

Serverfehler werden fast immer durch eine Fehlkonfiguration von Grav verursacht. Etwas Unerwartetes ist passiert und aus diesem Grund ist Grav nicht in der Lage, die Seite zu sichern und zu aktualisieren.

Wenn Sie diese Meldung sehen, bedeutet das, dass Ihr Server im `Produktionsmodus` läuft, um potentiell sensible Informationen vor der Veröffentlichung gegenüber Ihren Benutzern zu verbergen.  Der Fehler selbst wird in der Datei `logs/grav.log` aufgezeichnet.  Bitte untersuchen Sie diese Datei, um die genaue Art des Fehlers festzustellen.

Mögliche Gründe sind unter anderem:

* Eine veraltete Konfiguration verursacht Server-Fehler
* Falsche Dateiberechtigungen hindern Grav daran, Daten zu speichern
* Grav kennt Änderungen am Dateisystem noch nicht
* Fehler beim Parsen der Konfiguration wegen inkorrekter Konfigurationsdateien


!!! Wenn Sie das **Grav-Admin** Plugin installiert haben, können Sie die Server-Fehler von dort aus einsehen. Durch Anklicken der einzelnen Fehler können Sie die Debug-Seiten sehen, auch wenn der Debugger ausgeschaltet war.

## Veraltete Konfiguration

Als erstes sollten Sie den Cache leeren, um sicherzustellen, dass die Konfiguration auf dem aktuellen Stand ist:

[prism classes="language-bash command-line"]
bin/grav clearcache
[/prism]

!! Bevor Sie weitermachen, vergewissern Sie sich, dass Sie keine anderen Probleme mit den Dateiberechtigungen, wie die folgenden haben.

## Mögliche Installations- und Konfigurationsfehler

- Systemanforderungen nicht erfüllt
- Dateiberechtigungen falsch gesetzt
- Installationsprobleme
- Konfigurationsprobleme
