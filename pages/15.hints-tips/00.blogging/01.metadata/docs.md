---
title: Blogging Metadata
visible: true
twig_first: true
process:
    twig: true
taxonomy:
    category: docs
---

Wenn Sie Grav als Blogging-Plattform einsetzen, werden Sie Metadaten einbinden wollen, die helfen, Beschreibungen und Bilder zu ergänzen, wenn andere Ihren Beitrag in sozialen Medien wie Facebook, Twitter usw. teilen wollen.

Sie sollten diese Informationen in den Bereich [Header](/content/headers) Ihrer Grav-Seite einfügen.

Innerhalb der Dokumentation gibt es einen Verweis auf die Metadaten, die Sie im Header unter [Meta Page Headers](/content/headers#meta-page-headers) hinzufügen müssen. Wenn Sie jedoch von einer Plattform wie WordPress umgestiegen sind, auf der Sie ein Plugin dafür einsetzen, erkennen Sie möglicherweise nicht die Bedeutung der Metadaten.

Am Anfang jeder Ihrer Blog-Beiträge werden Sie vermutlich folgende Informationen sehen wollen:

[prism classes="language-yaml line-numbers"]
---
title: Blog Post Titel
publish_date: Datum der Freischaltung des Blog-Eintrags
date: Erstellungsdatum des Blog-Eintrags
metadata:
    'og:title': Blog Post Titel
    'og:type': article
    'og:description': Beschreibung des in Ihrem Blog-Eintrag behandelten Themas.  Das wird sichtbar, wenn jemand Ihren Beitrag in sozialen Medien veröffentlicht.
    'og:url': Die URL des Blogbeitrags
    'og:site_name': Der Name der gesamten Website, zu der der Blog-Beitrag gehört.
    'og:locale': Die Sprache, in der Ihr Blog-Beitrag geschrieben ist
    'og:image': Das Bild, auf das Sie hier verweisen, wird sichtbar, wenn es in sozialen Medien freigegeben wird.
    'twitter:card' : Der Typ der Twitter-Karte, die zu verwenden ist.
    'twitter:site' : Ihr Twitter-Handle
    'twitter:title' : Blog Post Titel
    'twitter:description' : Beschreibung des in Ihrem Blog-Eintrag behandelten Themas. Das wird sichtbar, wenn jemand Ihren Beitrag in sozialen Medien veröffentlicht.
    'twitter:image' : Das Bild, auf das Sie hier verweisen, wird sichtbar, wenn es in sozialen Medien freigegeben wird.
    'twitter:creator': Der Twitter-Handle des Blog-Post-Autors
taxonomy:
    category: [Kategorie des Blog-Beitrags]
    tag: [Tag 1, Tag 2, Tag 3, Tag 4]
    author: Name das Autors
---
[/prism]

Mehr Informationen über [Twitter Cards](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started.html) finden

Weitere Informationen zu dem [Open Graph Protokoll](https://ogp.me/)
