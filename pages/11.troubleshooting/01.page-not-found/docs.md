---
title: 404 Not Found
taxonomy:
    category: docs
---

Es gibt eine Reihe von Gründen, warum Sie einen Fehler **Not Found** erhalten können. Sie alle können unterschiedliche Ursachen haben.

![404 Not Found](404-not-found.png?classes=shadow)

!! Die folgenden Beispiele beziehen sich hauptsächlich auf den Apache-Webserver. Diese Server-Software wird weltweit häufig eingesetzt.

### Verwendung der .htaccess-Datei durch den IIS-Server

Nachdem Sie URL Rewrite mit dem Web Platform Installer auf dem IIS-Server hinzugefügt haben, starten Sie den IIS-Server neu. Gehen Sie zur Management-Schnittstelle, IIS.  Doppelklicken Sie auf URL Rewrite und klicken Sie unter Inbound Rules auf Import Rules. Suchen Sie unter Rules to Import die Konfigurationsdatei, wählen Sie die .htaccess-Datei im Stammverzeichnis und klicken Sie dann auf Import. Starten Sie den IIS-Server neu. Jetzt können Sie auf Grav zugreifen.

### Fehlende .htaccess Datei

Als erstes sollten Sie überprüfen, ob die mitgelieferte `.htaccess`-Datei im Stammverzeichnis Ihrer Grav-Installation abgelegt ist. Da es sich um eine **versteckte** Datei handelt, werden Sie diese normalerweise nicht in Ihrem Explorer- oder Finder-Fenster sehen.  Wenn Sie Grav extrahiert und die Dateien dann **ausgewählt** und **verschoben** oder **kopiert** haben, haben Sie diese sehr wichtige Datei möglicherweise vergessen.

Es wird **dringend empfohlen**, die Grav-Zipdatei zu entpacken und den **ganzen Ordner** an seinen Platz zu verschieben und den Ordner dann einfach umzubenennen. Dadurch wird sichergestellt, dass alle Dateien ihre richtige Position behalten.

### AllowOverride All

Damit die von Grav bereitgestellte `.htaccess` Datei in der Lage ist, die für ein einwandfreies Routing erforderlichen Rewrite-Regeln zu setzen, muss Apache sie zuerst lesen. Wenn Sie Ihre `<Directory>` oder `<VirtualHost>` Direktive mit `AllowOverride None` eingerichtet haben, wird die `.htaccess` Datei komplett ignoriert. Die einfachste Lösung ist, dies in `AllowOverride All` zu ändern, wobei RewriteRule verwendet wird. **FollowSymLinks** oder **SymLinksIfOwnerMatch** muss in der Options-Direktive gesetzt werden. Fügen Sie einfach in der gleichen Zeile '+FollowSymlinks' nach 'Options' ein.

Weitere Einzelheiten zu AllowOverride und zu allen verfügbaren Konfigurationsoptionen finden Sie in der [Apache Dokumentation](http://httpd.apache.org/docs/2.4/mod/core.html#allowoverride).

### RewriteBase Fehler

Wenn die Homepage Ihrer Grav-Site geladen wird, aber **jede andere Seite** diesen sehr groben _Apache-style_ Fehler anzeigt, dann ist es am wahrscheinlichsten, daß es ein Problem mit Ihrer `.htaccess` Datei gibt.

Die standardmäßige `.htaccess`, die mit Grav zusammen ausgeliefert wird, funktioniert in den meisten Fällen problemlos Out-of-the-Box.  Es gibt jedoch bestimmte Setups mit virtuellen Hosts, bei denen das Dateisystem nicht direkt mit dem Setup des virtuellen Hosts übereinstimmt. In diesen Fällen müssen Sie die Option `RewriteBase` im `.htaccess` konfigurieren, um auf den korrekten Pfad zu verweisen.

Es gibt dazu in der `.htaccess` Datei selbst eine kurze Beschreibung:

[prism classes="language-apacheconf line-numbers"]
##
# If you are getting 404 errors on subpages, you may have to uncomment the RewriteBase entry
# You should change the '/' to your appropriate subfolder. For example if you have
# your Grav install at the root of your site '/' should work, else it might be something
# along the lines of: RewriteBase /<your_sub_folder>
##

# RewriteBase /
[/prism]

Entfernen Sie einfach das `#` vor der Direktive `RewriteBase /`, um sie zu aktivieren, und passen Sie den Pfad an Ihre Serverumgebung an.

Wir haben zusätzliche Informationen in unsere [htaccess-Seite](../htaccess) aufgenommen, die Ihnen helfen sollen, Ihre `.htaccess` Datei zu finden und deren Fehler zu beheben.

### Fehlende Rewrite Module

Bei einigen Webserver-Paketen (ich denke dabei an EasyPHP und WAMP!) ist das Apache-Modul **rewrite** nicht standardmäßig aktiviert. Sie können normalerweise in den Konfigurationseinstellungen für Apache aktiviert werden oder Sie können das manuell über die `httpd.conf` tun. Dazu müssen Sie (`#`) in dieser Zeile (oder etwas Vergleichbarem) das Kommentar-Zeichen entfernen, damit sie von Apache geladen werden:

[prism classes="language-apacheconf"]
#LoadModule rewrite_module modules/mod_rewrite.so
[/prism]

Starten Sie dann Ihren Apache-Server neu.

### .htaccess Test-Skript

Um `.htaccess`- und **rewrite**-Probleme zu isolieren, können Sie die Datei [htaccess_tester.php](https://gist.githubusercontent.com/rhukster/a727fb70d9341536d49980d1239bd97e/raw/a3078da16b894ba86f9d000bcfc4850e098199fc/htaccess_tester.php) herunterladen und in Ihrem Grav-Stammverzeichnis ablegen.

Dann rufen Sie in Ihrem Browser `http://yoursite.com/htaccess_tester.php` auf.  Sie sollten eine Erfolgsmeldung erhalten und eine Kopie der Grav `.htaccess` Datei angezeigt bekommen.

![](htaccess_tester.png?classes=shadow)

Als nächstes können Sie testen, ob die Rewrites funktionieren, indem Sie die vorhandene .htaccess-Datei sichern:

[prism classes="language-bash command-line"]
mv .htaccess .htaccess-backup
[/prism]

Dann testen Sie diese einfache `.htaccess` Datei:

[prism classes="language-apacheconf line-numbers"]
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^.*$ htaccess_tester.php
</IfModule>
[/prism]

Dann probieren Sie diese URL aus: `http://yoursite.com/test`.  Eigentlich sollte jeder Pfad, den Sie verwenden, eine Erfolgsmeldung anzeigen, die Ihnen mitteilt, dass `mod_rewrite` funktioniert.

Nachdem Sie das Testen beendet haben, sollten Sie die Testdatei löschen und Ihre `.htaccess` Datei wiederherstellen:

[prism classes="language-bash command-line"]
rm htaccess_tester.php
mv .htaccess-backup .htaccess
[/prism]

### Grav Error-404 Seite

![404 Not Found](error-404.png?classes=shadow)

Wenn Sie einen Fehler im _Grav-Stil_ mit der Meldung **Error 404** erhalten, dann funktioniert Ihre `.htaccess` korrekt, aber Sie haben versucht, eine Seite zu erreichen, die Grav nicht finden kann.

Die häufigste Ursache dafür ist einfach, dass die Seite verschoben oder umbenannt wurde. Eine andere Sache, die es zu überprüfen gilt, ist, ob die Seite einen `Slug` in den YAML-Headern der Seite gesetzt hat. Dieser setzt den expliziten Ordnernamen außer Kraft, der standardmäßig zur URL-Konstruktion verwendet wird.

Eine andere Ursache könnte sein, dass Ihre Seite **nicht routingfähig** ist. Die Routing-Option für eine Seite kann in den [page headers](../../content/headers) eingestellt werden.

### 404 Page Not Found auf Nginx

Wenn sich Ihre Site in einem Unterordner befindet, stellen Sie sicher, dass Ihrer nginx.conf die Option `location` auf diesen Unterordner verweist. Die [Grav-Beispieldatei nginx.conf](https://github.com/getgrav/grav/blob/master/webserver-configs/nginx.conf) hat einen Kommentar im Code, der erklärt, wie das geht.
