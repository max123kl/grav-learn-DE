---
title: Erste Schritte
taxonomy:
    category: docs
---

Angenommen, Sie haben [Grav installiert](../installation) und die im vorherigen Kapitel genannten Anweisungen befolgt, dann können wir fortfahren und ein wenig mit Grav herumspielen, um es besser kennenzulernen.

Da Grav keine Datenbank benötigt, ist es ziemlich einfach, damit zu arbeiten, ohne sich Sorgen machen zu müssen, Probleme zwischen Ihrer Grav-Installation und einer anderen wichtigen Datenquelle zu verursachen. Wenn etwas schief geht, können Sie normalerweise die Daten sehr leicht wiederherstellen.

## Grundsätzliches zu den Inhalten

Machen wir uns zunächst damit vertraut, wo Grav die Inhalte ablegt. Wir werden in einem [späteren Kapitel](../folder-structure) ausführlicher darauf eingehen, aber bis auf weiteres müssen Sie sich darüber im Klaren sein, dass alle unsere Benutzerdaten im Ordner `user/pages/` der Grav-Installation gespeichert sind.

Zur Zeit befinden sich zwei Ordner im pages-Ordner, der erste heißt `01.home` und der zweite ist `02.typography`.  Der Teil `01.` des Ordnernamens ist optional, bietet aber ein paar praktische Möglichkeiten.

Erstens können Sie damit die Reihenfolge Ihrer Seiten ausdrücklich festlegen.  Zum Beispiel wird `01` vor `02` stehen, aber `00` wird vor `01` stehen.

Die andere Funktion des numerischen Teils im Ordnernamen besteht darin, Grav explizit darauf hinzuweisen, dass diese Seite im Menü sichtbar sein sollte.  Es ist wichtig darauf zu achten, dass der numerische Teil bis einschließlich des `.` aus den URLs entfernt wird.

## Konfiguration der Homepage (Startseite)

Es gibt eine Option in der Datei `user/config/system.yaml`, die den Speicherort der __home page__ festlegt. Mit anderen Worten heißt das, dass Grav auf diese Position zeigt, wenn Sie auf die Startseite Ihrer Site verweisen: `https://Ihre_Site.de`.

Wenn Sie diese Konfigurationsdatei in der Installation überprüfen, werden Sie sehen, dass sie bereits auf den Alias für `/home` verweist.  In diesem Beispiel können wir es so beibehalten.

## Bearbeitung der Seiten

Die Seiten in **Grav** sind nach der **Markdown**-Syntax aufgebaut.  Markdown ist eine einfache Textformatierungs-Syntax, die ein Computer leicht analysieren und nach HTML konvertieren kann. Sie verwendet einfache Textsymbole zur Markierung der Darstellung (z.B. **fett**, _kursiv_, Überschriften, Listen usw.). Das erleichtert das Verfassen von Texten, ohne dass man die Komplexität von HTML kennen muss. Zu den Vorteilen von Markdown gehören eine niedrige Fehlerquote, gute Lesbarkeit, einfache Erlernbarkeit und Verwendung usw.

Sie können eine ausführliche [Syntax-Beschreibung](../../content/markdown) mit Beispielen in der Dokumentation lesen. Folgen Sie aber vorerst den Beispielen.

Öffnen Sie die Startseite in Ihrem Texteditor. Die Datei, welche die Homepage steuert, befindet sich im Ordner `user/pages/01.home/` und heißt `default.md`. Der gesamte von Ihnen erstellte Inhalt wird im Ordner `user/pages/` Ihrer Grav-Installation abgelegt.

Wenn Sie die Seite in einem Texteditor öffnen, sieht der Text so ungefähr so aus:

[div class="no-margin-bottom"]
[prism classes="language-yaml line-numbers"]
---
title: Home
body_classes: title-center title-h1h2
---
[/prism]
[/div]
[div class="no-margin-top"]
[prism classes="language-markdown line-numbers" ln-start="5"]
# Say Hello to Grav!
## installation successful...

Congratulations! You have installed the **Base Grav Package** that provides a **simple page** and the default **Quark** theme to get you started.

!! If you see a **404 Error** when you click `Typography` in the menu, please refer to the [troubleshooting guide](https://learn.getgrav.org/troubleshooting/page-not-found).
[/prism]
[/div]

Lassen Sie uns das ein wenig aufschlüsseln, damit Sie sehen können, wie einfach es ist, mit Markdown zu schreiben. Das „Gerümpel“ zwischen den `---` Markierungen sind die [Kopfzeilen (Headers)](../../content/headers) der Seite. Diese werden in einem einfachen Format namens [YAML](../../advanced/yaml) geschrieben. Dieser Konfigurationsblock, der sich in der `.md` Datei befindet, ist allgemein als **YAML Front Matter** bekannt.

[prism classes="language-bash line-numbers"]
title: Home
body_classes: title-center title-h1h2
[/prism]

Dieser Block setzt das HTML-Titel-Tag für die Seite (der Text, den Sie im Browser-Tab sehen). Sie können auch von Ihren Themen aus über das Attribut `page.title` darauf zugreifen. Es gibt ein paar [Standard-Kopfzeilen](../../content/headers), mit denen Sie eine Reihe von Optionen für diese Seite konfigurieren können. Ein weiteres Beispiel ist `menu: Irgendwas`. Hiermit können Sie den Text überschreiben, der zur Darstellung des Seitennamens in einem Menü verwendet wird. Standardmäßig verwendet Grav den Titel für den Menüwert.

[prism classes="language-markdown line-numbers"]
# Say Hello to Grav!
## installation successful...
[/prism]

Die Syntax `#` oder `Hashes` in Markdown bezeichnet eine Überschrift.  Ein einzelnes `#` mit einem Leerzeichen und dann Text wird in eine `<h1>` Titelzeile in HTML umgewandelt. Ein `##` oder doppelte Hashes würden sich in einen `<h2>` Tag umwandeln.  Das geht natürlich bis zum gültigen HTML-Tag `<h6>` hin, der dann aus sechs Hashes bestehen würde: `######## Mein H6 Level Header`.

[prism classes="language-markdown line-numbers"]
Congratulations! You have installed the **Base Grav Package** that provides a **simple page** and the default **Quark** theme to get you started.
[/prism]

Dies ist ein einfacher Absatz, der bei der Konvertierung in HTML in reguläre `<p>` Tags eingebettet worden wäre.  Die Markierungen `**` zeigen fetten Text oder `<strong>`, früher `<b>`, in HTML an.  Kursiver Text wird gekennzeichnet, indem Text in `_`-Markierungen eingeschlossen wird.

[prism classes="language-markdown line-numbers"]
!! If you see a **404 Error** when you click `Typography` in the menu, please refer to the [troubleshooting guide](https://learn.getgrav.org/troubleshooting/page-not-found).
[/prism]

Dieser Abschnitt verwendet eine benutzerdefinierte Kennzeichnung, die vom integrierten Plugin `markdown-notices` zur Verfügung gestellt wird.  Damit können Sie einfache Hinweise erzeugen, wenn Sie einem Absatz des Textes eine Reihe von `!` Symbolen (Ausrufezeichen) voranstellen, von `!` bis `!!!!`.

Diese Übersicht sollte Ihnen einige wichtige Hinweise zum Schreiben von Markdown geben, aber Sie sollten sich unsere [ausführlichere Beschreibung](../../content/markdown) ansehen, um ein tieferes Verständnis zu erlangen.

!! Achten Sie darauf, dass Ihre `.md`-Dateien im Format `UTF8` kodiert werden. Dadurch wird die Verwendung von sprachspezifischen Sonderzeichen ermöglicht.

## Eine neue Seite hinzufügen

Das Erstellen einer neuen Seite ist eine einfache Angelegenheit in **Grav**.  Folgen Sie einfach diesen wenigen Schritten:

1. Wechseln Sie zu Ihrem Pages-Ordner: `user/pages/` und erstellen Sie einen neuen Ordner. In diesem Beispiel werden wir ausdrücklich die [Standardreihenfolge](https://learn.getgrav.org/content/content-pages) verwenden und den Ordner `03.mypage` benennen.
2. Starten Sie Ihren Texteditor, erzeugen Sie eine neue Datei und fügen Sie den folgenden Beispielcode ein:

[div class="no-margin-bottom"]
[prism classes="language-yaml line-numbers"]
---
title: Meine neue Seite
---
[/prism]
[/div]
[div class="no-margin-top"]
[prism classes="language-markdown line-numbers" ln-start="4"]
# My New Page!

Das ist der Hauptteil (body) **meiner neuen Seite** und ich kann hier leicht die _Markdown_-Syntax verwenden.
[/prism]
[/div]

3. Speichern Sie diese Datei im Ordner `user/pages/03.mypage/` als `default.md`. Dadurch wird **Grav** angewiesen, die Seite unter Verwendung des Templates **default** im jeweiligen Theme zu rendern, hier: `user/themes/quark/templates/default.html.twig`.
4. Das war’s schon! Aktualisieren Sie die Site in Ihrem Browser, um Ihre neue Seite oben im Menü zu sehen.

Die Seite erscheint automatisch im Menü nach dem Menüpunkt **„Typographie“**. Wenn Sie den Anzeige-Namen im Menü  ändern möchten, fügen Sie `menu: Meine Seite` zwischen den Bindestrichen im Seiten-Header hinzu.

**Gratulation:** Sie haben jetzt erfolgreich eine neue Seite in Grav erstellt.  Es gibt noch viel mehr, was Sie mit Grav noch machen können. Lesen Sie also bitte weiter, um mehr über die fortgeschrittenen Fähigkeiten und tiefer gehenden Funktionen zu erfahren.

!! Wenn Sie Schwierigkeiten beim Abrufen dieser neuen Seite haben, fehlt entweder eine `.htaccess` Datei (nur Apache-Webserver) oder Sie müssen den Befehl `RewriteBase` in der `.htaccess` Datei bearbeiten. Bitte lesen Sie das Kapitel [Fehlerbehebung](../../troubleshooting) für weitergehende Informationen.
