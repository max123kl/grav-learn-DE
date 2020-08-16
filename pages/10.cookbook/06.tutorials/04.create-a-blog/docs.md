---
title: Einen Blog einrichten
taxonomy:
    category: docs
---

!! Laden Sie das Skeleton der Blog-Site von [https://getgrav.org/downloads/skeletons](https://getgrav.org/downloads/skeletons) herunter und installieren Sie es lokal oder laden Sie zumindest das Repository [https://github.com/getgrav/grav-skeleton-blog-site](https://github.com/getgrav/grav-skeleton-blog-site) herunter, um es zu überprüfen. Dies ist eine Beispielseite, die das Thema Antimaterie verwendet. Eine funktionierende Grav-Site zu haben, die bereits mit einer Blog-Struktur arbeitet, wird Ihnen sicherlich eine Hilfe sein, wenn Sie nicht weiterkommen oder nicht wissen, was Sie als nächstes tun sollen.

## Überprüfen, ob Ihr Theme die Templates für Blog- und Artikelseiten enthält

Fangen wir ganz einfach an: Wählen Sie ein Thema, das bereits eine Blog-Seitenvorlage bietet. Zum Beispiel Antimatter, TwentyFifteen, Deliver, Lingonberry, Afterburner2 und viele andere.
Wie können Sie überprüfen, ob Ihr Theme bereits ein Blog-Seiten-Template enthält? Gehen Sie in den Ordner `/user/themes/[IhrTheme]/templates` und überprüfen Sie die Existenz der Dateien `blog.html.twig` und `item.html.twig`.

Wenn Sie bereits ein Theme ausgewählt haben und in Ihrem Theme diese Daten nicht enthalten sind, dann kopieren Sie die von Antimatter: [https://github.com/getgrav/grav-theme-antimatter/tree/develop/templates](https://github.com/getgrav/grav-theme-antimatter/tree/develop/templates)

Möglicherweise müssen Sie das Markup (die „Textauszeichnung“) an Ihr Theme abpassen. Wenn Sie aber gerade erst anfangen, sollten Sie besser ein Theme verwenden, das bereits im Lieferumfang enthalten ist.

## Erstellen der Struktur von Blog-Seiten
Es gibt mehrere Varianten für die Gliederung der Seiten. Die vorgegebene und einfachere ist, eine Hauptseite vom Typ Blog und untergeordnete Seiten für die Blog-Posts zu definieren.

### Mit dem Admin Plugin
Richten Sie eine Seite vom Typ Blog ein. Diese Seite ist der Blog „Homepage“, mit der Liste der Blog-Beiträge.

Erstellen Sie eine oder mehrere untergeordnete Seiten vom Typ `Item`. Das sind die Blog-Beiträge.

### oder manuell
Wechseln Sie zu Ihrem Ordner pages/, erstellen Sie eine Seite `01.blog` (ändern Sie die Nummer, um Ihre Menüstruktur wiederzugeben) und fügen Sie eine Datei `blog.md` hinzu.
In dieser Datei sollten Sie diesen Inhalt einfügen:

[prism classes="language-yaml line-numbers"]
---
content:
    items: '@self.children'
---
[/prism]

Das weist Grav an, sich durch die Unterseiten (die Blog-Einträge) zu bewegen, zu iterieren.

Erstellen Sie einen Unterordner für jeden Beitrag, den Sie hinzufügen möchten, und fügen Sie in jedem Ordner eine Datei `item.md` mit dem Inhalt des Blogbeitrags hinzu.

## URLs

Die oben erläuterte Struktur erzeugt Blog-Einträge mit `/blog/` in der URL. Möglicherweise ist dies nicht das, was Sie benötigen. Zum Beispiel: Wenn ein Blog alles ist, was Sie auf Ihrer Website haben, und die Auflistung der Blog-Beiträge die Startseite ist. In diesem Fall möchten Sie nur, dass Ihre Root-Domain auf diesen Inhalt zugreift, anstatt Besucher auf ein untergeordnetes Verzeichnis zu verweisen.

Setzen Sie daher die Option `home.hide_in_urls` (Startseite in URLs ausblenden, im Admin) in der Datei system.yaml (Systemkonfiguration im Admin) auf true.

## Die inneren Mechanismen

Vielleicht möchten Sie erfahren, wie das Ganze funktionieren soll. Das Blog-Template, der Inhalt der Datei `blog.html.twig`, die im Theme-Ordner `templates/` liegt, iteriert lediglich durch seine Unterseiten.

In seiner einfachsten Form:

[prism classes="language-twig line-numbers"]
{% set collection = page.collection() %}

{% for child in collection %}
        {% include 'partials/blog_item.html.twig' with {'blog':page, 'page':child, 'truncate':true} %}
{% endfor %}
[/prism]

page.collection() übernimmt standardmäßig aus dem YAML-Frontmatter der Seite die Eigenschaft `content.items` und gibt ein Array mit den Elementen zurück, die dieser Definition entsprechen.

Enthält die Seite:

[prism classes="language-yaml line-numbers"]
---
content:
    items: '@self.children'
---
[/prism]

dann ist `collection` das Array aus den Unterseiten der aktuellen Seite.

In diesem Beispiel enthält das Theme `partials/blog_item.html.twig`, das für das Rendern des einzelnen Blog-Posts verantwortlich ist, und übergibt ihm das Objekt `child`, das den aktuellen Blog-Beitrag enthält, zum Rendern.

### ergänzende Informationen

- Collections: [https://learn.getgrav.org/content/collections](https://learn.getgrav.org/content/collections)
- Listing Page: [https://learn.getgrav.org/content/content-pages#listing-page](https://learn.getgrav.org/content/content-pages#listing-page)
- Folders: [https://learn.getgrav.org/content/content-pages#folders](https://learn.getgrav.org/content/content-pages#folders)
- Taxonomy: [https://learn.getgrav.org/content/taxonomy#taxonomy-example](https://learn.getgrav.org/content/taxonomy#taxonomy-example)
