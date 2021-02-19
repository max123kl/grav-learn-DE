---
title: Asset-Manager
page-toc:
  active: true
taxonomy:
    category: docs
---

In Grav 1.6 wurde der **Asset-Manager** vollständig neu programmiert, um einen flexibleren Mechanismus für die Verwaltung von **CSS**- und **JavaScript**-Assets in Themes zu ermöglichen. Der Hauptzweck des Asset-Managers besteht darin, den Prozess des Hinzufügens von Assets aus Themes und Plugins zu vereinfachen und gleichzeitig erweiterte Funktionen wie Priorität und eine **Asset-Pipeline** bereitzustellen, mit der Assets **minimiert**, **komprimiert** und **inline** verwaltet werden können, um die Anzahl der Browser-Anfragen und auch die Gesamtgröße der Assets zu reduzieren.

Er ist viel flexibler und zuverlässiger als früher. Außerdem ist er wesentlich übersichtlicher und leichter verständlich, wenn man anfängt, sich durch den Code zu kämpfen. Der Asset Manager ist in ganz Grav verfügbar und kann in Plugin-Event-Hooks, aber auch direkt in Themes über Twig-Calls angesprochen werden.

! **Technische Einzelheiten**: Die primäre Asset-Klasse wurde stark vereinfacht und reduziert. Ein Großteil der Logik wurde in 3 Merkmale zerlegt. Eine _Test-Eigenschaft_, die Funktionen enthält, die in erster Linie in unserer Testsuite verwendet werden, eine _Utils-Eigenschaft_, die Methoden enthält, die von den regulären Asset-Typen (js, inline_js, css, inline_css) und der Asset-Pipeline gemeinsam genutzt werden und die minimiert und komprimiert werden können. Schließlich eine _Legacy-Eigenschaft_, die Methoden enthält, bei denen es sich um Abkürzungen oder Workarounds handelt und die künftig nicht mehr verwendet werden sollten.

!!! Der Asset Manager ist vollständig abwärtskompatibel mit der Syntax, die in Versionen vor Grav 1.6 verwendet wurde. Die untenstehende Dokumentation behandelt jedoch die neue **bevorzugte Syntax**.

## Konfiguration

Der Grav Asset-Manager verfügt über einen einfachen Satz von Konfigurations-Optionen.  Die Standardwerte befinden sich in der Systemdatei `system.yaml`, aber Sie können einige oder alle dieser Werte in Ihrer Datei `user/config/system.yaml` überschreiben:

[prism classes="language-yaml line-numbers"]
assets:                                # Configuration for Assets Manager (JS, CSS)
  css_pipeline: false                  # The CSS pipeline is the unification of multiple CSS resources into one file
  css_pipeline_include_externals: true # Include external URLs in the pipeline by default
  css_pipeline_before_excludes: true   # Render the pipeline before any excluded files
  css_minify: true                     # Minify the CSS during pipelining
  css_minify_windows: false            # Minify Override for Windows platforms, also applies to js. False by default due to ThreadStackSize
  css_rewrite: true                    # Rewrite any CSS relative URLs during pipelining
  js_pipeline: false                   # The JS pipeline is the unification of multiple JS resources into one file
  js_pipeline_include_externals: true  # Include external URLs in the pipeline by default
  js_pipeline_before_excludes: true    # Render the pipeline before any excluded files
  js_minify: true                      # Minify the JS during pipelining
  enable_asset_timestamp: false        # Enable asset timestamps
  collections:
    jquery: system://assets/jquery/jquery-2.x.min.js
[/prism]

## Struktur

Es gibt mehrere Ebenen der Positionskontrolle, wie in der folgenden Abbildung dargestellt.  In der Reihenfolge ihres Anwendungsbereichs sind dies:

* **Group** – erlaubt die Gruppierung von Assets wie `head` (Vorgabe) und `bottom`
* **Position** – `before`, `pipeline`(Vorgabe) und `after`.  Im Prinzip erlaubt das Ihnen die Angabe, wo in der Gruppe die Assets geladen werden sollen.
* **Priority** – steuert die **Reihenfolge**. Dabei werden größere Integerzahlen (z.B. `100`) vor kleineren Integerzahlen ausgegeben. `10` ist die Standardeinstellung.

[prism classes="language-text"]
 CSS
┌───────────────────────┐
│ Group (head)          │
│┌─────────────────────┐│        ┌──────────────────┐
││ Position            ││        │   priority 100   │─────┐     ┌──────────────────┐
││┌───────────────────┐││        ├──────────────────┤     ├────▶│       CSS        │
│││                   │││        │   priority 99    │─────┤     └──────────────────┘
│││      before       │├┼──┬────▶├──────────────────┤     │
│││                   │││  │     │    priority 1    │─────┤     ┌──────────────────┐
││├───────────────────┤││  │     ├──────────────────┤     ├────▶│    inline CSS    │
│││                   │││  │     │    priority 0    │─────┘     └──────────────────┘
│││     pipeline      │├┼──┤     └──────────────────┘
│││                   │││  │
││├───────────────────┤││  │
│││                   │││  │
│││       after       │├┼──┘
│││                   │││
││└───────────────────┘││
│└─────────────────────┘│
└───────────────────────┘


  JS
┌───────────────────────┐
│ Group (head)          │
│┌─────────────────────┐│        ┌──────────────────┐
││ Position            ││        │   priority 100   │─────┐     ┌──────────────────┐
││┌───────────────────┐││        ├──────────────────┤     ├────▶│        JS        │
│││                   │││        │   priority 99    │─────┤     └──────────────────┘
│││      before       │├┼──┬────▶├──────────────────┤     │
│││                   │││  │     │    priority 1    │─────┤     ┌──────────────────┐
││├───────────────────┤││  │     ├──────────────────┤     ├────▶│    inline JS     │
│││                   │││  │     │    priority 0    │─────┘     └──────────────────┘
│││     pipeline      │├┼──┤     └──────────────────┘
│││                   │││  │
││├───────────────────┤││  │
│││                   │││  │
│││       after       │├┼──┘
│││                   │││
││└───────────────────┘││
│└─────────────────────┘│
└───────────────────────┘
[/prism]

Standardmäßig werden `CSS` und `JS` in der Position `pipeline` angezeigt, wenn sie erzeugt werden.  Indessen werden `InlineCSS` und `InlineJS` standardmäßig in der Position `after` angezeigt.  Dies ist allerdings konfigurierbar und Sie können jedes Asset in jeder beliebigen Position anzeigen lassen.

## Assets in Themes

### Beschreibung

Im Allgemeinen fügen Sie CSS-Assets nacheinander mit `assets.addCss()` oder `assets.addInlineCss()` Calls hinzu und rendern diese Assets dann über `assets.css()`. Optionen, welche die Priorität, das Pipelining oder Inlining steuern, können pro Asset beim Hinzufügen oder zur Renderzeit für eine Gruppe von Assets angegeben werden.

JS-Assets werden mit den Aufrufen `assets.addJs()` und `assets.addInlineJs()` auf ähnliche Weise verarbeitet. Es gibt auch eine generische Methode `assets.add()`, die versucht, den Typ des Assets zu ermitteln, das Sie hinzufügen, aber es wird empfohlen, die spezifischeren Methodenaufrufe zu verwenden.

Der Asset-Manager unterstützt auch:

* das Hinzufügen von Assets zu angegebenen Gruppen mit dem Ziel, solche Gruppen an verschiedenen Orten und/oder mit verschiedenen Optionen zu rendern
* die Konfiguration von angegebenen Asset-Kollektionen, die mit einem einzigen Aufruf von `assets.add*()` hinzugefügt werden können

### Beispiel

Ein Beispiel dafür, wie Sie CSS-Dateien in Ihr Theme einfügen können, finden Sie im Standardtheme **quark**, das mit Grav zusammen angeboten wird. Wenn Sie einen Blick auf die Datei [`templates/partials/base.html.twig`](https://github.com/getgrav/grav-theme-quark/blob/develop/templates/partials/base.html.twig) werfen, sehen Sie etwas Vergleichbares wie das Folgende:

[prism classes="language-twig line-numbers"]
<!DOCTYPE html>
<html>
    <head>
    ...

    {% block stylesheets %}
        {% do assets.addCss('theme://css-compiled/spectre.css') %}
        {% do assets.addCss('theme://css-compiled/theme.css') %}
        {% do assets.addCss('theme://css/custom.css') %}
        {% do assets.addCss('theme://css/line-awesome.min.css') %}
    {% endblock %}

    {% block javascripts %}
        {% do assets.addJs('jquery', 101) %}
        {% do assets.addJs('theme://js/jquery.treemenu.js', {group:'bottom'}) %}
        {% do assets.addJs('theme://js/site.js', {group:'bottom'}) %}
    {% endblock %}

    {% block assets deferred %}
        {{ assets.css()|raw }}
        {{ assets.js()|raw }}
    {% endblock %}
    </head>

    <body>
    ...

    {% block bottom %}
        {{ assets.js('bottom')|raw }}
    {% endblock %}
    </body>
</html>
[/prism]

Der Twig-Tag `block stylesheets` definiert lediglich einen Template-Bereich, die diesen Bereich erweitern, ersetzen oder angehängt werden können. Innerhalb des Blocks sehen Sie eine Reihe von `do assets.addCss()` Aufrufen.

Der Tag `{% do %}` ist eigentlich [in Twig integriert](http://twig.sensiolabs.org/doc/tags/do.html) und erlaubt Ihnen, Variablen zu verändern, ohne eine Ausgabe zu generieren.

Die Methode `addCss()` fügt dem Asset-Manager CSS-Assets hinzu. Wenn Sie einen zweiten numerischen Parameter angeben, legt dieser die Priorität des Stylesheets fest. Wenn Sie keine Priorität angeben, bestimmt die Reihenfolge, mit der die Assets hinzugefügt werden, deren Abfolge beim Rendern. Sie werden bemerkt haben, dass ein **PHP-Stream-Wrapper** `theme://` verwendet wird, um Grav eine einfache Möglichkeit zu bieten, den relativen Pfad des aktuellen Themes zu bestimmen.

!! Die Option `assets.addJs('jquery', 101)` wird die Kollektion `jquery` enthalten, die in der globalen Asset-Konfiguration definiert ist. Der optionale Parameter `101` setzt die Priorität ziemlich hoch, damit sie vorrangig gerendert wird.  Die Standardpriorität ist `10`, wenn nichts angegeben ist. Eine etwas flexiblere Schreibweise wäre `assets.addJs('jquery', {priority: 101})`.  Das würde Ihnen erlauben, neben der Priorität weitere Parameter hinzuzufügen.

Der Aufruf `assets.css()|raw` rendert die CSS-Assets als HTML-Tags. Da bei dieser Methode kein Parameter angegeben ist, wird die Gruppe standardmäßig auf `head` gesetzt. Beachten Sie, dass diese Gruppe in einen `assets deferred`-Block eingeschlossen wurde. Das ist eine neue Funktion in Grav 1.6, die es Ihnen erlaubt, Assets aus anderen Twig-Templates hinzuzufügen, die weiter unten auf der Seite (oder irgendwo anders auf der Seite) enthalten sind. Trotzdem können Sie so sicherstellen, dass sie bei Bedarf in diesem `head`-Block gerendert werden können.

Der Block `bottom` ganz am Ende der Theme-Ausgabe rendert JavaScript, das der Gruppe `bottom` zugewiesen wurde.

## Assets hinzufügen

#### add(asset, [Optionen])

Die Add-Methode versucht bestmöglich, ein Asset anhand der Dateierweiterung zuzuordnen. Es ist eine bequeme Methode, es ist aber besser, eine der direkten Methoden für CSS oder JS aufzurufen.  Details finden Sie bei den direkten Methoden.

!! Das Optionen-Array ist der bevorzugte Ansatz für die Übergabe mehrerer Optionen. Wie im vorherigen Beispiel mit `jquery` können Sie jedoch eine Abkürzung verwenden und eine Ganzzahl für das **zweite Argument** in der Methode übergeben, wenn Sie nur die **Priorität** setzen wollen.

#### addCss(asset, [Optionen])

Mit dieser Methode werden Assets in die Liste der CSS-Assets aufgenommen. Die Priorität ist standardmäßig auf 10 gesetzt, wenn sie nicht angegeben wird.  Eine höhere Zahl bedeutet, dass sie vor den Assets mit geringerer Priorität dargestellt werden. Die Option `pipeline` steuert, ob dieses Asset in die Kombination/Minify-Pipeline aufgenommen werden soll oder nicht. Falls keine Pipeline vorhanden ist, steuert die Option `loading`, ob das Asset als Link zu einem externen Style-Sheet dargestellt werden soll oder ob sein Inhalt innerhalb eines Inline-Style-Tags eingefügt werden soll.

#### addInlineCss(css, [Optionen])

Erlaubt das Hinzufügen eines CSS-Strings innerhalb eines Inline-Style-Tags. Sinnvoll für die Initialisierung oder für alles, was dynamisch ist.  Um den Inhalt einer regulären Asset-Datei einzufügen, siehe die Option `{'loading': 'inline'}` der Methoden `addCss()` und `css()`.

#### addJs(asset, [Optionen])

This method will add assets to the list of JavaScript assets.  The priority defaults to 10 if not provided.  A higher number means it will display before lower priority assets.  The `pipeline` option controls whether or not this asset should be included in the combination/minify pipeline. If not pipelined, the `loading` option controls whether the asset should be rendered as a link to an external script file or whether its contents should be inlined inside an inline script tag.

#### addInlineJs(javascript, [Optionen])

Lets you add a string of JavaScript inside an inline script tag. Useful for initialization or anything dynamic.  To inline a regular asset file's content, see the `{'loading': 'inline'}` option of the `addJs()` and `js()` methods.

#### registerCollection(name, array)

Allows you to register an array of CSS and JavaScript assets with a name for later use by the `add()` method. Particularly useful if you want to register a collection that may be used by multiple themes or plugins, such as jQuery or Bootstrap.

## Optionen

Where appropriate, you can pass in an array of asset options. The core options are:

#### For CSS

* **priority**: Integer value (default value is `10`)

* **position**: `pipeline` is default but can also be `before` or `after` the assets in `pipeline` position.

* **loading**: `inline` if this asset should be output inline rather (default: referenced via a link to the stylesheet). Should be used in conjunction with `position: before` or `position: after` as it will have no effect with `position: pipeline` (default).

* **group**: string to specify a unique group name for asset (default is `head`)

#### For JS

* **priority**: Integer value (default value is `10`)

* **position**: `pipeline` is default but can also be `before` or `after` the assets in `pipeline` position.

* **loading**: supports any loading type such as, `async`, `defer`, `async defer` or `inline`. Should be used in conjunction with `position: before` or `position: after` as it will have no effect with `position: pipeline` (default).

* **group**: string to specify a unique group name for asset (default is `head`)

#### Other Attributes

You can also pass anything else you like in the options array, and if they are not these standard types, they will simply be rendered as attributes such as `{id: 'custom-id'}` will render as `id="custom-id"` in the HTML tag. This can be also used to include structured data such as json-ld via `addInlineJs()` by using `{type: 'application/ld+json'}`.

#### Examples

For example:

[prism classes="language-twig"]
{% do assets.addCss('page://01.blog/assets-test/example.css?foo=bar', { priority: 20, loading: 'inline', position: 'before'}) %}
[/prism]

Will render as:

[prism classes="language-html line-numbers"]
<style>
h1.blinking {
    text-decoration: underline;
}
</style>
<link.....
[/prism]

Another example:

[prism classes="language-twig"]
{% do assets.addJs('page://01.blog/assets-test/example.js', {loading: 'async', id: 'custom-css'}) %}
[/prism]

Will render as:

[prism classes="language-html"]
<script src="/grav/user/pages/01.blog/assets-test/example.js" async id="custom-css"></script>
[/prism]

## Rendering Assets

The following allow you to render the current state of the CSS and JavaScript assets.

#### css([group, [options]])

Renders CSS assets that have been added to an Asset Manager's group (default is `head`). Options are

* **loading**: `inline` if **all** assets in this group should be inlined (default: render each asset according to its `position` option)

* **_link attributes_**, see below (default: `{'type': 'text/css', 'rel': 'stylesheet'}`). Effective only if `inline` is **not** used as this group's rendering option

If pipelining is turned **off** in the configuration, the group's assets are rendered individually, ordered by asset priority (high to low), followed by the order in which assets were added.

If pipelining is turned **on** in the configuration, assets in the pipeline position are combined in the order in which assets were added, then processed according to the pipeline configuration.

Each asset is rendered either as a stylesheet link or inline, depending on the asset's `loading` option and whether `{'loading': 'inline'}` is used for this group's rendering. CSS added by `addInlineCss()` will be rendered in the `after` position by default, but you can configure it to render before the pipelined output with `position: before`

#### js([group, [options]])

Renders JavaScript assets that have been added to an Asset Manager's group (default is `head`). Options are

* **loading**: `inline` if **all** assets in this group should be inlined (default: render each asset according to its `position` option)

* **_script attributes_**, see below (default: `{'type': 'text/javascript'}`). Effective only if `inline` is **not** used as this group's rendering option

If pipelining is turned **off** in the configuration, the group's assets are rendered individually, ordered by asset priority (high to low), followed by the order in which assets were added.

If pipelining is turned **on** in the configuration, assets in the pipeline position are combined in the order in which assets were added, then processed according to the pipeline configuration. The combined pipeline result is then rendered before or after non-pipelined assets depending on the setting of `js_pipeline_before_excludes`.

Each asset is rendered either as a script link or inline, depending on the asset's `loading` option and whether `{'loading': 'inline'}` is used for this group's rendering. Note that the only way to inline a JS pipeline is to use inline loading as an option of the `js()` method. JS added by `addInlineJs()` will be rendered in the `after` position by default, but you can configure it to render before the pipelined output with `position: before`

## Named Assets

Grav now has a powerful feature called **named assets** that allows you to register a collection of CSS and JavaScript assets with a name.  Then you can simply **add** those assets to the Asset Manager via the name you registered the collection with.  Grav comes preconfigured with **jQuery** but has the ability to define custom collections in the `system.yaml` to be used by any theme or plugin:

[prism classes="language-yaml line-numbers"]
assets:
  collections:
    jquery: system://assets/jquery/jquery-2.1.3.min.js
    bootstrap:
        - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css
        - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css
        - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js
[/prism]

You can also use the `registerCollection()` method programmatically.

[prism classes="language-yaml line-numbers"]
$assets = $this->grav['assets'];
$bootstrapper_bits = [https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css,
                      https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css,
                      https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js];
$assets->registerCollection('bootstrap', $bootstrap_bits);
$assets->add('bootstrap', 100);
[/prism]

An example of this action can be found in the [**bootstrapper** plugin](https://github.com/getgrav/grav-plugin-bootstrapper/blob/develop/bootstrapper.php#L51-L71).

## Grouped Assets

The Asset manager lets you pass an optional `group` as part of an options array when adding assets.  While this is of marginal use for CSS, it is especially useful for JavaScript where you may need to have some JS files or Inline JS referenced in the header, and some at the bottom of the page.

To take advantage of this capability you must specify the group when adding the asset, and should use the options syntax:

[prism classes="language-twig"]
{% do assets.addJs('theme://js/example.js', {'priority':102, 'group':'bottom'}) %}
[/prism]

Then for these assets in the bottom group to render, you must add the following to your theme:

[prism classes="language-twig"]
{{ assets.js('bottom')|raw }}
[/prism]

If no group is defined for an asset, then `head` is the default group.  If no group is set for rendering, the `head` group will be rendered. This ensures the new functionality is 100% backwards compatible with existing themes.

The same goes for CSS files:

[prism classes="language-twig"]
{% do assets.addCss('theme://css/ie8.css', {'group':'ie'}) %}
[/prism]

and to render:


[prism classes="language-twig"]
{{ assets.css('ie')|raw }}
[/prism]

## Change attribute of the rendered CSS/JS assets

CSS is by default added using the `rel="stylesheet"` attribute, and `type="text/css"` , while JS has `type="text/javascript"`.

To change the defaults, or to add new attributes, you need to create a new group of assets, and tell Grav to render it with that attribute.

Example of editing the `rel` attribute on a group of assets:

[prism classes="language-twig line-numbers"]
{% do assets.addCSS('theme://whatever.css', {'group':'my-alternate-group'}) %}
...
{{ assets.css('my-alternate-group', {'rel': 'alternate'})|raw }}
[/prism]

## Inlining Assets

Inlining allows the placing critical CSS (and JS) code directly into the HTML document enables the browser to render a page immediately without waiting for external stylesheet or script downloads. This can improve site performance noticeably for users, particularly over mobile networks. Details can be found in [this article on optimizing CSS delivery](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery).

However, directly inserting CSS or JavaScript code into a page template is not always feasible, for example, where Sass-complied CSS is used. Keeping CSS and JS assets in separate files also simplifies maintenance. Using the Asset Manager's inline capability enables you to optimize for speed without changing the way your assets are stored. Even entire pipelines can be inlined.

To inline an asset file's content, use the option `{'loading': 'inline'}` with `addCss()` or `addJs()`. You can also inline all assets when rendering a group with `js()` or `css()`, which provide the same option.

Example of using `system.yaml` to define asset collections named according to asset loading requirements, with `app.css` being a [Sass](http://sass-lang.com/)-generated CSS file:

[prism classes="language-yaml line-numbers"]
assets:
  collections:
    css-inline:
      - 'http://fonts.googleapis.com/css?family=Ubuntu:400|Open+Sans:400,400i,700'
      - 'theme://css-compiled/app.css'
    js-inline:
      - 'https://use.fontawesome.com/<embedcode>.js'
    js-async:
      - 'theme://foundation/dist/assets/js/app.js'
      - 'theme://js/header-display.js'
[/prism]

The template inserts each collection into its corresponding group, namely `head` and `head-link` for CSS, `head` and `head-async` for JS. The default group `head` is used for inline loading in each case:

[prism classes="language-twig line-numbers"]
{% block stylesheets %}
    {% do assets.addCss('css-inline') %}
    {% do assets.addCss('css-link', {'group': 'head-link'}) %}
{% endblock %}
{{ assets.css('head-link')|raw }}
{{ assets.css('head', {'loading': 'inline'})|raw }}
{% block javascripts %}
    {% do assets.addJs('js-inline') %}
    {% do assets.addJs('js-async', {'group': 'head-async'}) %}
{% endblock %}
{{ assets.js('head-async', {'loading': 'async'})|raw }}
{{ assets.js('head', {'loading': 'inline'})|raw }}
[/prism]


## Static Assets

Sometimes there is a need to reference assets without using the Asset Manager.  There is a `url()` helper method available to achieve this.  An example of this could be if you wanted to reference an image from the theme. The syntax for this is:

[prism classes="language-twig"]
<img src="{{ url("theme://" ~ widget.image)|e }}" alt="{{ widget.text|e }}" />
[/prism]

The `url()` method takes an optional second parameter of `true` or `false` to enable the URL to include the schema and domain. By default this value is assumed `false` resulting in just the relative URL.  For example:

[prism classes="language-twig"]
<script src="{{ url('theme://some/extra.css', true)|e }}"></script>
[/prism]
