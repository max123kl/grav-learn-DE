Wichtige Theme-Updates
Kleine Änderung, um Grav noch besser zu machen

TLDR - Beginnend mit Grav 1.5.10 und Grav 1.6.0 ist die Erweiterung Deferred Twig verfügbar und eine Aktualisierung Ihres Themas wird dringend empfohlen, um sicherzustellen, dass Sie in Zukunft keine Probleme mit Plugins haben werden.

Was als reines Grav 1.6-Feature begann, wurde auf Grav 1.5.10 zurückportiert, da es immer wichtiger wird, langfristige Probleme mit Plugins zu lösen, die ihre eigenen CSS- und JS-Assets haben. Wir haben Unterstützung für die Twig Deferred Extension in Grav aufgenommen, die ein ziemlich ärgerliches Problem in Twig löst, bei dem Inhalte in der Reihenfolge ihres Auftretens gerendert werden und einmal gerendert, nicht mehr manipuliert werden können. Das beste Beispiel für diese Einschränkung betrifft die CSS- und JS-Assets, die dem Grav-Asset-Manager hinzugefügt und im Allgemeinen im <head></head> HTML-Block gerendert werden. Wenn eine enthaltene Twig-Vorlage, eine modulare Seite oder sogar ein Plugin verarbeitet wird, nachdem dieser Block gerendert wurde, haben sie keine Möglichkeit, ihre eigenen Objekte zu dem Block hinzuzufügen.

Die bisherige Lösung bestand darin, sicherzustellen, dass alle Objekte vor dem Rendern von Twig hinzugefügt wurden, aber dies ist oft keine praktikable Lösung. Ein Beispiel für dieses Problem sind Formularfelder, die spezielle CSS- oder JS-Anforderungen haben und erst dann versucht werden, diese Assets hinzuzufügen, wenn das Feld gerendert wird, lange nachdem das Rendern der Assets im Tag <head></head> abgeschlossen ist. Wir haben uns darum gekümmert, indem wir empfohlen haben, einen neuen Bottom-Asset-Block am Ende des Themes hinzuzufügen, damit diese Assets zumindest zuletzt gerendert werden können.
Änderungen an der Basis-Zweig-Vorlage

Im Hinblick auf die Handhabung von Assets wurde in der Regel ein typisches Theme in dieser Rohfassung aufgebaut, wobei die <head></head> Assets in jedem der Asset-Blöcke von partials/base.html.twig dargestellt werden:
