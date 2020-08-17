---
title: Seiten-Kollektionen
page-toc:
  active: true
taxonomy:
    category: docs
---

Die Kollektionen sind seit den ersten Betas von Grav erheblich umfangreicher geworden. Wir haben mit einer sehr begrenzten Anzahl von seitenbasierten Kollektionen begonnen, aber mit der Hilfe unserer Community haben wir diese Fähigkeiten erweitert, um sie noch leistungsfähiger zu machen!  So sehr, dass sie jetzt ein eigenes Kapitel in der Dokumentation haben.

## Grundlagen der Grav-Kollektionen

In Grav ist die häufigste Art der Kollektion eine Liste von Seiten, die entweder im Frontmatter der Seite oder in Twig selbst definiert werden kann. Der gebräuchlichste Typ ist die Definition einer Kollektion in den Kopfzeilen der Seite. Wenn eine Kollektion definiert ist, steht sie im Twig der Seite zur Verfügung, mit dem Sie nach Belieben arbeiten können. Sie sehr können sehr mächtige Funktionen realisieren mit Hilfe der Page-Collection-Methoden, der Verwendung von Loops für ein [Page-Objekt](/themes/theme-vars#page-object) und mit Hilfe der Page-Methoden oder -Eigenschaften. Gängige Beispiele hierfür sind die Anzeige einer Liste von Blog-Einträgen oder die Anzeige modularer Unterseiten zur Visualisierung eines komplexen Seitendesigns.

## Collection-Objekt

Wenn Sie eine Kollektion im Page-Header definieren, erstellen Sie dynamisch eine [Grav-Kollektion](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Page/Collection.php), die im Twig der Seite verfügbar ist. Dieses Collection-Objekt ist **iterierbar** und kann wie ein **Array** behandelt werden, mit dem Sie u.a. Aktionen wie die Folgende ausführen können:

[prism classes="language-twig line-numbers"]
{{ dump(page.collection[page.path]) }}
[/prism]

## Beispiel-Definition einer Kollektion

Eine Beispiel-Kollektion, die im Frontmatter der Seite definiert ist:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.children'
    order:
        by: date
        dir: desc
    limit: 10
    pagination: true
[/prism]

Der Wert `content.items` im Frontmatter der Seite weist Grav an, eine Kollektion von Elementen (items) zusammenzustellen. Diese Informationen definieren, wie die Kollektion aufgebaut werden soll.

Diese Definition erstellt eine Kollektion für die Seite. Sie enthält alle untergeordneten Seiten (**child pages**), sortiert nach Datum (**date**) in absteigender Reihenfolge (**descending**), wobei 10 Artikel (**10 items**) pro Seite anzeigt werden und die Seitennummerierung **pagination** aktiv ist.

## Zugriff auf Kollektionen in Twig

Sobald diese Kollektion in den Kopfzeilen definiert ist, erstellt Grav eine Kollektion **page.collection**, auf die Sie in einem Twig-Template zugreifen können:

[prism classes="language-twig line-numbers"]
{% for p in page.collection %}
<h2>{{ p.title }}</h2>
{{ p.summary }}
{% endfor %}
[/prism]

Dadurch wird einfach eine Schleife (loop) über die Seiten ([pages](/themes/theme-vars#page-object)) in der Kollektion gelegt, auf denen Titel und Zusammenfassung angezeigt werden.

Sie können auch einen Parameter für die Reihenfolge angeben, um die voreingestellte Reihenfolge der Seiten zu ändern:

[prism classes="language-twig line-numbers"]
{% for p in page.collection.order('folder','asc') %}
<h2>{{ p.title }}</h2>
{{ p.summary }}
{% endfor %}
[/prism]

## Kopfzeilen der Kollektion

Um Grav mitzuteilen, dass eine bestimmte Seite eine Auflistung sein soll und Unterseiten enthält, gibt es eine Reihe von Variablen, die verwendet werden können:

### Summary of collection options

[div class="table-keycol"]
|                   String                  |                           Result                          |
|-------------------------------------------|-----------------------------------------------------------|
| `'@root'`                                   | Abrufen der Unterseiten von root                        |
| `'@root.children'`                          | Abrufen der Unterseiten von root (Alternative)          |
| `'@root.descendants'`                       | Abrufen von root und Erfassen **aller** Unterseiten     |
|                                             |                                                           |
| `'@self.parent'`                            | Abrufen der Eltern-Seite der aktuellen Seite              |
| `'@self.siblings'`                          | Eine Kollektion aller anderen Seiten dieser Ebene         |
| `'@self.modular'`                           | Nur die modularen Unterseiten abrufen                     |
| `'@self.children'`                          | Nur die *nicht* modularen Unterseiten abrufen             |
| `'@self.descendants'`                       | Erfassen aller nicht-modularen Unterseiten                |
|                                           |                                                           |
| `'@page': '/fruit'`                         | Abrufen aller Unterseiten der Seite `/fruit`              |
| `'@page.children': '/fruit'`                | Alternative zu oben                                       |
| `'@page.self': '/fruit'`                    | Eine Kollektion nur der Seite `/fruit` abrufen            |
| `'@page.page': '/fruit'`                    | Alternative zu oben                                       |
| `'@page.descendants': '/fruit'`             | Abrufen und Erfassen aller Unterseiten der Seite `/fruit` |
| `'@page.modular': '/fruit'`                 | Eine Kollektion aller modularen Unterseiten der Seite `/fruit` abrufen |
|                                           |                                                           |
| `'@taxonomy.tag': photography`              | Taxonomy mit dem Tag=`photography`                        |
| `'@taxonomy': {tag: birds, category: blog}` | Taxonomy mit dem Tag `birds` und der Kategorie `blog`     |
[/div]

! In diesem Dokument wird die Verwendung von `@page`, `@taxonomy.category` usw. skizziert, aber ein YAML-sichereres Alternativformat ist `page@`, `taxonomy@.category`.  Alle Kommandos mit `@` können entweder im Präfix- oder Postfix-Format eingegeben werden.

Wir wollen uns detaillierter damit befassen.

## Root-Kollektionen

##### @root – Unterseiten der obersten Ebene

Dies kann verwendet werden, um die Top/Root-Ebene der **veröffentlichten, nicht-modularen Unterseiten** einer Site abzurufen. Dies ist zum Beispiel besonders nützlich, um die Elemente abzurufen, aus denen sich die Primärnavigation zusammensetzt:

[prism classes="language-yaml line-numbers"]
content:
    items: '@root'
[/prism]

ein gültiger Alias ist:

[prism classes="language-yaml line-numbers"]
content:
    items: '@root.children'
[/prism]

##### @root – Top-Level Unterseiten + alle Nachfolge-Seiten

Auf diese Weise wird jede Seite Ihrer Website effektiv erreicht, da sie rekursiv durch alle Unterseiten von der Root-Seite abwärts navigiert und eine Kollektion von **allen** der **veröffentlichten, nicht-modularen Unterseiten** einer Website aufbaut:

[prism classes="language-yaml line-numbers"]
content:
    items: '@root.descendants'
[/prism]

## Self-Kollektionen

##### @self.children – Children of the current page

Dies wird verwendet, um die **veröffentlichten, nicht-modularen Unterseiten** der aktuellen Seite aufzulisten:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.children'
[/prism]

##### @self.descendants - Non-modular children + all descendants of the current page

Similar to `.children`, the `.descendants` collection will retrieve all the **published non-modular children** but continue to recurse through all their children:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.descendants'
[/prism]

##### @self.modular - Modular children of the current page

The inverse of `.children`, this method retrieves only **published modular children** of the current page (`_features`, `_showcase`, etc.):

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.modular'
[/prism]

##### @self.parent - The parent page of the current page

This is a special case collection because it will always return just the one **parent** of the current page:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.parent'
[/prism]

##### @self.siblings - All the sibling pages

This collection will collect all the **published** Pages at the same level of the current page, excluding the current page:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.siblings'
[/prism]

## Page Collections

##### @page or @page.children - Collection of children of a specific page

This collection takes a slug route of a page as an argument and will return all the **published non-modular** children of that page:

[prism classes="language-yaml line-numbers"]
content:
    items:
      '@page': '/blog'
[/prism]

alternatively:

[prism classes="language-yaml line-numbers"]
content:
    items:
      '@page.children': '/blog'
[/prism]

##### @page.self or @page.page - Collection of just the specific page

This collection takes a slug route of a page as an argument and will return collection containing that page (if it is **published and non-modular**):

[prism classes="language-yaml line-numbers"]
content:
    items:
      '@page.self': '/blog'
[/prism]

##### @page.descendants - Collection of children + all descendants of a specific page

This collection takes a slug route of a page as an argument and will return all the **published non-modular** children and all their descendants of that page:

[prism classes="language-yaml line-numbers"]
content:
    items:
      '@page.descendants': '/blog'
[/prism]

##### @page.modular - Collection of modular children of a specific page

This collection takes a slug route of a page as an argument and will return all the **published modular** children of that page:

[prism classes="language-yaml line-numbers"]
content:
    items:
      '@page.modular': '/blog'
[/prism]


## Taxonomy Collections

[prism classes="language-yaml line-numbers"]
content:
   items:
      '@taxonomy.tag': foo
[/prism]

Using the `@taxonomy` option, you can utilize Grav's powerful taxonomy functionality.  This is where the `taxonomy` variable in the [Site Configuration](../../basics/grav-configuration#site-configuration) file comes into play. There **must** be a definition for the taxonomy defined in that configuration file for Grav to interpret a page reference to it as valid.

By setting `@taxonomy.tag: foo`, Grav will find all the **published pages** in the `/user/pages` folder that have themselves set `tag: foo` in their taxonomy variable:

[prism classes="language-yaml line-numbers"]
content:
    items:
       '@taxonomy.tag': [foo, bar]
[/prism]

The `content.items` variable can take an array of taxonomies and it will gather up all pages that satisfy these rules. Published pages that have **both** `foo` **and** `bar` tags will be collected.  The [Taxonomy](../taxonomy) chapter will cover this concept in more detail.

!! If you wish to place multiple variables inline, you will need to separate sub-variables from their parents with `{}` brackets. You can then separate individual variables on that level with a comma. For example: `'@taxonomy': {category: [blog, featured], tag: [foo, bar]}`. In this example, the `category` and `tag` sub-variables are placed under `@taxonomy` in the hierarchy, each with listed values placed within `[]` brackets. Pages must meet **all** these requirements to be found.

If you have multiple variables in a single parent to set, you can do this using the inline method, but for simplicity, we recommend using the standard method. Here is an example:

[prism classes="language-yaml line-numbers"]
content:
  items:
    '@taxonomy':
      category: [blog, featured]
      tag: [foo, bar]
[/prism]

Each level in the hierarchy adds two whitespaces before the variable. YAML will allow you to use as many spaces as you want here, but two is standard practice. In the above example, both the `category` and `tag` variables are set under `@taxonomy`.

Page collections will automatically be filtered by taxonomy when one has been set in the URL (e.g. /archive/category:news). This enables you to build a single blog archive template but filter the collection dynamically using the URL. If your page collection should ignore the taxonomy set in the URL use the flag `url_taxonomy_filters:false` to disable this feature.

### Complex Collections

You can also provide multiple complex collection definitions and the resulting collection will be the sum of all the pages found from each of the collection definitions. For example:

[prism classes="language-yaml line-numbers"]
content:
  items:
    - '@self.children'
    - '@taxonomy':
         category: [blog, featured]
[/prism]

Additionally, you can filter the collection by using `filter: type: value`. The type can be any of the following: `published`, `non-published`, `visible`, `non-visible`, `modular`, `non-modular`, `routable`, `non-routable`, `type`, `types`, `access`. These correspond to the [Collection-specific methods](#collection-object-methods), and you can use several to filter your collection. They are all either `true` or `false`, except for `type` which takes a single template-name, `types` which takes an array of template-names, and `access` which takes an array of access-levels. For example:

[prism classes="language-yaml line-numbers"]
 content:
  items: '@self.siblings'
  filter:
    published: true
    type: 'blog'
[/prism]

### Ordering Options

[prism classes="language-yaml line-numbers"]
content:
    order:
        by: date
        dir: desc
    limit: 5
    pagination: true
[/prism]

Ordering of sub-pages follows the same rules as ordering of folders, the available options are:

[div class="table-keycol"]
| Ordering     | Details                                                                                                                                            |
| :----------  | :----------                                                                                                                                        |
| `default`    | The order is based on the file system, i.e. `01.home` before `02.advark`                                                                              |
| `title`      | The order is based on the title as defined in each page                                                                                            |
| `basename`   | The order is based on the alphabetic folder name after it has been processed by the `basename()` PHP function                                      |
| `date`       | The order is based on the date as defined in each page                                                                                                |
| `modified`   | The order is based on the modified timestamp of the page                                                                                              |
| `folder`     | The order is based on the folder name with any numerical prefix, i.e. `01.`, removed                                                                  |
| `header.x`   | The order is based on any page header field. i.e. `header.taxonomy.year`. Also a default can be added via a pipe. i.e. `header.taxonomy.year|2015`    |
| `manual`     | The order is based on the `order_manual` variable                                                                                                     |
| `random`     | The order is randomized                                                                                                                            |
| `custom`     | The order is based on the `content.order.custom` variable                                                                                                                             |
| `sort_flags` | Allow to override sorting flags for page header-based or default ordering. If the `intl` PHP extension is loaded, only [these flags](https://secure.php.net/manual/en/collator.asort.php) are available. Otherwise, you can use the PHP [standard sorting flags](https://secure.php.net/manual/en/array.constants.php). |
[/div]

The `content.order.dir` variable controls which direction the ordering should be in. Valid values are either `desc` or `asc`.

[prism classes="language-yaml line-numbers"]
content:
    order:
        by: default
        custom:
            - _showcase
            - _highlights
            - _callout
            - _features
    limit: 5
    pagination: true
[/prism]

In the above configuration, you can see that `content.order.custom` is defining a **custom manual ordering** to ensure the page is constructed with the **showcase** first, **highlights** section second etc. Please note that if a page is not specified in the custom ordering list, then Grav falls back on the `content.order.by` for the unspecified pages.

If a page has a custom slug, you must use that slug in the `content.order.custom` list.

The `content.pagination` is a simple boolean flag to be used by plugins etc to know if **pagination** should be initialized for this collection. `content.limit` is the number of items displayed per-page when pagination is enabled.

### Date Range

New as of **Grav 0.9.13** is the ability to filter by a date range:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.children'
    dateRange:
        start: 1/1/2014
        end: 1/1/2015
[/prism]

You can use any string date format supported by [strtotime()](https://php.net/manual/en/function.strtotime.php) such as `-6 weeks` or `last Monday` as well as more traditional dates such as `01/23/2014` or `23 January 2014`. The dateRange will filter out any pages that have a date outside the provided dateRange.  Both **start** and **end** dates are optional, but at least one should be provided.

### Multiple Collections

When you create a collection with `content: items:` in your YAML, you are defining a single collection based on several conditions.  However, Grav does let you create an arbitrary set of collections per page, you just need to create another one:

[prism classes="language-yaml line-numbers"]
content:
    items: '@self.children'
    order:
        by: date
        dir: desc
    limit: 10
    pagination: true

fruit:
    items:
       '@taxonomy.tag': [fruit]
[/prism]

This sets up **2 collections** for this page, the first uses the default `content` collection, but the second one defines a taxonomy-based collection called `fruit`.  To access these two collections via Twig you can use the following syntax:

[prism classes="language-yaml line-numbers"]
{% set default_collection = page.collection %}
{% set fruit_collection = page.collection('fruit') %}
[/prism]

## Collection Object Methods

Iterable methods include:

[div class="table-keycol"]
| Property | Description |
| -------- | ----------- |
| `Collection::append($items)` | Add another collection or array |
| `Collection::first()` | Get the first item in the collection |
| `Collection::last()` | Get the last item in the collection |
| `Collection::random($num)` | Pick `$num` random items from the collection |
| `Collection::reverse()` | Reverse the order of the collection |
| `Collection::shuffle()` | Randomize the entire collection |
| `Collection::slice($offset, $length)` | Slice the list |
[/div]

Also has several useful Collection-specific methods:

[div class="table-keycol"]
| Property | Description |
| -------- | ----------- |
| `Collection::addPage($page)` | You can append another page to this collection |
| `Collection::copy()` | Creates a copy of the current collection |
| `Collection::current()` | Gets the current item in the collection |
| `Collection::key()` | Returns the slug of the current item |
| `Collection::remove($path)` | Removes a specific page in the collection, or current if `$path = null` |
| `Collection::order($by, $dir, $manual)` | Orders the current collection |
| `Collection::intersect($collection2)` | Merge two collections, keeping items that occur in both collections (like an "AND" condition) |
| `Collection::merge($collection2)` | Merge two collections, keeping items that occur in either collection (like an "OR" condition) |
| `Collection::isFirst($path)` | Determines if the page identified by path is first |
| `Collection::isLast($path)` | Determines if the page identified by path is last |
| `Collection::prevSibling($path)` | Returns the previous sibling page if possible |
| `Collection::nextSibling($path)` | Returns the next sibling page if possible |
| `Collection::currentPosition($path)` | Returns the current index |
| `Collection::dateRange($startDate, $endDate, $field)` | Filters the current collection with dates |
| `Collection::visible()` | Filters the current collection to include only visible pages |
| `Collection::nonVisible()` | Filters the current collection to include only non-visible pages |
| `Collection::modular()` | Filters the current collection to include only modular pages |
| `Collection::nonModular()` | Filters the current collection to include only non-modular pages |
| `Collection::published()` | Filters the current collection to include only published pages |
| `Collection::nonPublished()` | Filters the current collection to include only non-published pages |
| `Collection::routable()` | Filters the current collection to include only routable pages |
| `Collection::nonRoutable()` | Filters the current collection to include only non-routable pages |
| `Collection::ofType($type)` | Filters the current collection to include only pages where template = `$type` |
| `Collection::ofOneOfTheseTypes($types)` | Filters the current collection to include only pages where template is in the array `$types` |
| `Collection::ofOneOfTheseAccessLevels($levels)` | Filters the current collection to include only pages where page access is in the array of `$levels` |
[/div]

Here is an example taken from the **Learn2** theme's **docs.html.twig** that defines a collection based on taxonomy (and optionally tags if they exist) and uses the `Collection::isFirst` and `Collection::isLast` methods to conditionally add page navigation:

[prism classes="language-twig line-numbers"]
{% set tags = page.taxonomy.tag %}
{% if tags %}
    {% set progress = page.collection({'items':{'@taxonomy':{'category': 'docs', 'tag': tags}},'order': {'by': 'default', 'dir': 'asc'}}) %}
{% else %}
    {% set progress = page.collection({'items':{'@taxonomy':{'category': 'docs'}},'order': {'by': 'default', 'dir': 'asc'}}) %}
{% endif %}

{% block navigation %}
        <div id="navigation">
        {% if not progress.isFirst(page.path) %}
            <a class="nav nav-prev" href="{{ progress.nextSibling(page.path).url }}"> <i class="fa fa-chevron-left"></i></a>
        {% endif %}

        {% if not progress.isLast(page.path) %}
            <a class="nav nav-next" href="{{ progress.prevSibling(page.path).url }}"><i class="fa fa-chevron-right"></i></a>
        {% endif %}
        </div>
{% endblock %}
[/prism]

`nextSibling()` is up the list and `prevSibling()` is down the list, this is how it works:

Assuming you have the pages:

[prism classes="language-txt line-numbers"]
Project A
Project B
Project C
[/prism]

You are on Project A, the previous page is Project B.
If you are on Project B, the previous page is Project C and next is Project A


## Programmatic Collections

You can take full control of collections directly from PHP in Grav plugins, themes, or even from Twig.  This is a more hard-coded approach compared to defining them in your page frontmatter, but it also allows for more complex and flexible collections logic.

### PHP Collections

You can perform advanced collection logic with PHP, for example:

[prism classes="language-php line-numbers"]
$collection = new Collection($pages);
$collection->setParams(['taxonomies' => ['tag' => ['dog', 'cat']]])->dateRange('01/01/2016', '12/31/2016')->published()->ofType('blog-item')->order('date', 'desc');

$titles = [];

foreach ($collection as $page) {
    $titles[] = $page->title();
}
[/prism]

The `order()`-function can also, in addition to the `by`- and `dir`-parameters, take a `manual`- and `sort_flags`-parameter. These are [documented above](#ordering-options). You can also use the same `evaluate()` method that the frontmatter-based page collections make use of:

[prism classes="language-php line-numbers"]
$page = Grav::instance()['page'];
$collection = $page->evaluate(['@page.children' => '/blog', '@taxonomy.tag' => 'photography']);
$ordered_collection = $collection->order('date', 'desc');
[/prism]

And another example of custom ordering would be:

[prism classes="language-php line-numbers"]
$ordered_collection = $collection->order('header.price','asc',null,SORT_NUMERIC);
[/prism]

You can also do similar directly in **Twig Templates**:

[prism classes="language-twig line-numbers"]
{% set collection = page.evaluate([{'@page.children':'/blog', '@taxonomy.tag':'photography'}]) %}
{% set ordered_collection = collection.order('date','desc') %}
[/prism]

#### Advanced Collections

By default when you call `page.collection()` in the Twig of a page that has a collection defined in the header, Grav looks for a collection called `content`.  This allows the ability to define [multiple collections](#multiple-collections), but you can even take this a step further.

If you need to programmatically generate a collection, you can do so by calling `page.collection()` and passing in an array in the same format as the page header collection definition.  For example:

[prism classes="language-twig line-numbers"]
{% set options = { items: {'@page.children': '/my/pages'}, 'limit': 5, 'order': {'by': 'date', 'dir': 'desc'}, 'pagination': true } %}
{% set my_collection = page.collection(options) %}

<ul>
{% for p in my_collection %}
<li>{{ p.title }}</li>
{% endfor %}
</ul>
[/prism]

Generating menu for the whole site (you need to set *menu* property in the page's frontmatter):


[prism classes="language-yaml line-numbers"]
---
title: Home
menu: Home
---
[/prism]

[prism classes="language-twig line-numbers"]
{% set options = { items: {'@root.descendants':''}, 'order': {'by': 'folder', 'dir': 'asc'}} %}
{% set my_collection = page.collection(options) %}

{% for p in my_collection %}
{% if p.header.menu %}
	<ul>
	{% if page.slug == p.slug %}
		<li class="{{ p.slug }} active"><span>{{ p.menu }}</span></li>
	{% else %}
		<li class="{{ p.slug }}"><a href="{{ p.url }}">{{ p.menu }}</a></li>
	{% endif %}
	</ul>
{% endif %}
{% endfor %}
[/prism]

#### Pagination with Advanced Collections

A common question we hear is regarding how to enable pagiation for custom collections.  Pagination is a plugin that can be installed via GPM with the name `pagination`.  Once installed it works "out-of-the-box" with page configured collections, but knows nothing about custom collections created in Twig.  To make this process easier, pagination comes with it's own Twig function called `paginate()` that will provide the pagination functionality we need.

After we pass the collection and the limit to the `paginate()` function, we also need to pass the pagination information directly to the `partials/pagination.html.twig` template to render properly.

[prism classes="language-twig line-numbers"]
{% set options = { items: {'@root.descendants':''}, 'order': {'by': 'folder', 'dir': 'asc'}} %}
{% set my_collection = page.collection(options) %}
{% do paginate( my_collection, 5 ) %}

{% for p in my_collection %}
    <ul>
        {% if page.slug == p.slug %}
            <li class="{{ p.slug }} active"><span>{{ p.menu }}</span></li>
        {% else %}
            <li class="{{ p.slug }}"><a href="{{ p.url }}">{{ p.menu }}</a></li>
        {% endif %}
    </ul>
{% endfor %}

{% include 'partials/pagination.html.twig' with {'base_url':page.url, 'pagination':my_collection.params.pagination} %}
[/prism]


[version=16]
#### Custom Collection Handling with `onCollectionProcessed()` Event

There are times when the event options are just not enough.  Times when you want to get a collection but then further manipulate the collection based on something very custom.  Imagine if you will, a use case where you have what seems like a rather bog-standard blog listing, but your client wants to have fine grain control over what displays in the listing.  They want to have a custom toggle on every blog item that lets them remove it from the listing, but still have it published and available via a direct link.

To make this happen, we can simply add a custom `display_in_listing: false` option in the page header for the item:

[prism classes="language-yaml line-numbers"]
---
title: 'My Story'
date: '13:34 04/14/2020'
taxonomy:
    tag:
        - journal
display_in_listing: false
---
...
[/prism]

The problem is that there is no way to define or include this filter when defining a collection in the listing page.  It probably is defined something like this:

[prism classes="language-yaml line-numbers"]
---
menu: News
title: 'My Blog'
content:
    items:
        - self@.children
    order:
        by: date
        dir: desc
    limit: 8
    pagination: true
    url_taxonomy_filters: true
---
...
[/prism]

So the collection is simply defined by the `self@.chidren` directive to get all the published children of the current page. So what about those pages that have the `display_in_listing: false` set? We need to do some extra work on that collection before it is returned to ensure we remove any items that we don't want to see.  To do this we can use the `onCollectionProcessed()` event in a custom plugin.  We need to add the listener:

[prism classes="language-php line-numbers"]
    public static function getSubscribedEvents()
    {
        return [
            ['autoload', 100000],
            'onPluginsInitialized' => ['onPluginsInitialized', 0],
            'onCollectionProcessed' => ['onCollectionProcessed', 10]
        ];
    }
[/prism]

Then, we need to define the method and loop over the collection items, looking for any pages with that `display_in_listing:` field set, then remove it if it is `false`:

[prism classes="language-php line-numbers"]
    /**
     * Remove any page from collection with display_in_listing: false|0
     *
     * @param Event $event
     */
    public function onCollectionProcessed(Event $event)
    {
        /** @var Collection $collection */
        $collection = $event['collection'];

        foreach ($collection as $item) {
            $display_in_listing = $item->header()->display_in_listing ?? true;
            if ((bool) $display_in_listing === false) {
                $collection->remove($item->path());
            }
        }

    }
[/prism]

Now your collection has the correct items, and all other plugins or Twig templates that rely on that collection will see this modified collection so things like pagination will work as expected.
[/version]
