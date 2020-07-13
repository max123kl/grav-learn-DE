---
title: Konfiguration
page-toc:
  active: true
taxonomy:
    category: docs
---

Alle Grav-Konfigurationsdateien sind mit der [YAML-Syntax](../../advanced/yaml) geschrieben und haben die Dateierweiterung `.yaml`. YAML ist sehr intuitiv, was sowohl das Lesen als auch das Schreiben sehr einfach macht. Sie können jedoch die [YAML-Seite im Kapitel Erweitert](../../advanced/yaml) durchlesen, um ein besseres Gefühl für die zur Auswahl stehende YAML-Syntax zu bekommen.

## System-Konfiguration

Grav legt großen Wert darauf, es dem Benutzer so einfach wie möglich zu machen. Das gilt auch für die Konfiguration. Grav kommt mit einigen vernünftigen Standard-Optionen. Diese sind in der Datei `system/config/system.yaml` zusammengefasst.

Allerdings sollten Sie **diese Datei niemals ändern**, stattdessen sollten alle Konfigurationsänderungen, die Sie vornehmen müssen, in der Datei namens `user/config/system.yaml` gespeichert werden.  Jede Einstellung in dieser Datei, mit der gleichen Struktur und Benennung, überschreibt die Einstellung in der Konfigurationsdatei des Standardsystems.

!!!! Grundsätzlich sollten Sie **NIEMALS** irgendetwas im Ordner `system/` ändern.  Alle Handlungen die der Benutzer vornimmt (Inhalte erstellen, Plugins installieren, Konfiguration bearbeiten, usw.) sollten im Ordner `user/` erfolgen.  So können Sie leichter ein Upgrade durchführen und Ihre Modifikationen an einem einzigen Platz ablegen, um sie zu sichern, zu synchronisieren usw.

Nachfolgend sind die Variablen aufgelistet, die sich in der standardmäßigen Datei `system/config/system.yaml` befinden:

### Allgemeine Optionen

[prism classes="language-yaml line-numbers"]
absolute_urls: false
timezone: ''
default_locale:
param_sep: ':'
wrapped_site: false
reverse_proxy_setup: false
force_ssl: false
force_lowercase_urls: true
custom_base_url: ''
username_regex: '^[a-z0-9_-]{3,16}$'
pwd_regex: '(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}'
intl_enabled: true
[/prism]

Diese Optionen der Konfiguration erscheinen nicht innerhalb ihrer eigenen untergeordneten Abschnitte. Es handelt sich um allgemeine Optionen, die sich auf die Funktionsweise der Site, ihre Zeitzone und die Ursprungs-URL auswirken.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **absolute_urls:** | absolute oder relative URLs für `base_url` |
| **timezone:** | gültige Werte können [hier](https://php.net/manual/en/timezones.php) gefunden werden. |
| **default_locale:** | Standard-Lokalisierung (Standard-Einstellung des Systems) |
| **param_sep:** | Diese Option wird für Grav-Parameter in der URL verwendet.  Ändern Sie das nicht, es sei denn Sie wissen genau, was Sie tun.  Grav > `1.1.16` setzt das automatisch auf den Wert `;` (für Anwender, die den Apache-Webserver unter Windows betreiben). |
| **wrapped_site:** | Für Themes/Plugins, um zu erkennen, ob Grav von einer anderen Plattform eingebunden wird. Der Wert kann `true` oder `false` sein. |
| **reverse_proxy_setup:** | Wird in einem Reverse-Proxy-Szenario mit anderen Webserver-Ports als Proxy verwendet. Kann den Wert `true` oder `false` annehmen. |
| **force_ssl:** | Falls aktiviert, kann damit der Zugang zu Grav über HTTPS erzwungen werden (HINWEIS: Keine ideale Lösung). Der Wert kann `true` oder `false` sein. |
| **force_lowercase_urls:** | Wenn Sie URLs mit gemischter Schreibweise (groß und klein) unterstützen möchten, setzen Sie dies auf `false`. |
| **custom_base_url:** | Hier wird die base_url manuell gesetzt |
| **username_regex:** | erlaubt sind nur Kleinbuchstaben, Ziffern, Bindestriche und Unterstriche, 3 - 16 Zeichen |
| **pwd_regex:** | muss mindestens eine Zahl, einen Groß- und Kleinbuchstaben und mindestens 8+ Zeichen enthalten |
| **intl_enabled:** | Spezielle Algorithmen für die internationale PHP-Erweiterung (mod_intl) |
[/div]

### Languages

[prism classes="language-yaml line-numbers"]
languages:
  supported: []
  include_default_lang: true
  pages_fallback_only: false
  translations: true
  translations_fallback: true
  session_store_active: false
  http_accept_language: false
  override_locale: false
[/prism]

Der Abschnitt **Languages** der Datei legt die Spracheinstellungen der Site fest. Dazu gehört, welche Sprache(n) unterstützt werden, die Bezeichnung der Standardsprache in den URLs und die Übersetzungen. Hier folgt die Übersicht für den **Languages**-Abschnitt der System-Konfigurationsdatei:

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **supported:** | Liste der unterstützten Sprachen, z.B: `[en, fr, de]` |
| **include_default_lang:** | Fügt das Präfix für die Standardsprache in alle URLs ein, kann `true` oder `false` sein. |
| **pages_fallback_only:** | Nur Fallback zum Auffinden von Seiteninhalten durch unterstützte Sprachen, kann `true` oder `false` sein. |
| **translations:** | Übersetzungen standardmäßig aktivieren, kann `true` oder `false` sein. |
| **translations_fallback:** | Fallback mit Hilfe von unterstützten Übersetzungen, wenn active lang nicht existiert, kann `true` oder `false` sein. |
| **session_store_active:** | Aktive Sprache in der Sitzung speichern, kann `true` oder `false` sein. |
| **http_accept_language:** | Versucht die Sprache, basierend auf dem Header http_accept_language im Browser, einzustellen, kann `true` oder `false` sein. |
| **override_locale:** | Überschreibt die Standard- oder System-Locales mit einer sprachspezifischen Version, kann `true` oder `false` sein. |
[/div]

### Home

[prism classes="language-yaml line-numbers"]
home:
  alias: '/home'
  hide_in_urls: false
[/prism]

Im Abschnitt **Home** legen Sie den Standardpfad für die Homepage der Site fest. Sie können auch die Home-Route in den URLs ausblenden.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **alias:** | Standardpfad zur Homepage, d.h.: `/home` oder `/` |
| **hide_in_urls:** | Ausblenden der Home-Route in URLs, kann `true` oder `false` sein. |
[/div]

### Pages

[prism classes="language-yaml line-numbers"]
pages:
  theme: quark
  order:
    by: default
    dir: asc
  list:
    count: 20
  dateformat:
    default:
    short: 'jS M Y'
    long: 'F jS \a\t g:ia'
  publish_dates: true
  process:
    markdown: true
    twig: false
  twig_first: false
  never_cache_twig: false
  events:
    page: true
    twig: true
  markdown:
    extra: false
    auto_line_breaks: false
    auto_url_links: false
    escape_markup: false
    special_chars:
      '>': 'gt'
      '<': 'lt'
  types: [txt,xml,html,htm,json,rss,atom]
  append_url_extension: ''
  expires: 604800
  cache_control:
  last_modified: false
  etag: false
  vary_accept_encoding: false
  redirect_default_route: false
  redirect_default_code: 302
  redirect_trailing_slash: true
  ignore_files: [.DS_Store]
  ignore_folders: [.git, .idea]
  ignore_hidden: true
  hide_empty_folders: false
  url_taxonomy_filters: true
  frontmatter:
    process_twig: false
    ignore_fields: ['form','forms']
[/prism]

Im Abschnitt **Pages** der Datei `system/config/system.yaml` legen Sie viele der wichtigsten Einstellungen zum Theme fest. Hier legen Sie z.B. das Theme fest, das zum Rendern der Site verwendet wird, die Seitenreihenfolge, die Voreinstellungen für die Verarbeitung von Twig und Markdown und vieles mehr. HHier werden die meisten Einstellungen vorgenommen, die sich auf die Art und Weise auswirken, wie Ihre Seiten gerendert werden.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **theme:** | Hier wird das Standard Theme festgelegt. Voreingestellt ist `quark` |
| **order:** | |
| ... **by:** | Seiten sortieren nach `default`, `alpha` oder `date` |
| ... **dir:** | Voreingestellte Reihenfolge, `asc` oder `desc` (auf- oder absteigend) |
| **list:** | |
| ... **count:** | Voreingestellte Anzahl von Elementen pro Seite |
| **dateformat:** | |
| ... **default:** | Das von Grav erwartete, voreingestellte Datumsformat im Feld `date: |
| ... **short:** | Kurzes Datumsformat; zum Beispiel: `'jS M Y'` |
| ... **long:** | Langes Datumsformat; zum Beispiel: `'F jS \a\t g:ia'` |
| **publish_dates:** | Automatische Veröffentlichung/Nichtveröffentlichung auf der Basis vom Datum. Kann `true` oder `false` sein. |
| **process:** | |
| ... **markdown:** | Aktivieren oder deaktivieren der Verarbeitung von Markdown im Frontend. Kann `true` oder `false` sein. |
| ... **twig:** | Aktiviert oder deaktiviert die Verarbeitung von Twig im Frontend. Kann `true` oder `false` sein. |
| **twig_first:** | Verarbeitung von Twig vor Markdown, wenn beide auf einer Seite verwendet werden. Kann `true` oder `false` sein. |
| **never_cache_twig:** | Wenn Sie diese Option aktivieren, können Sie eine Verarbeitungslogik hinzufügen, die sich bei jedem Seitenaufruf dynamisch ändern kann, anstatt die Ergebnisse zwischenzuspeichern und für jeden Seitenaufruf zu speichern. Dies kann Site-weit in der **system.yaml** oder auf einer bestimmten Seite aktiviert/deaktiviert werden. Kann `true` oder `false` sein. |
| **events:** | |
| ... **page:** | Aktiviert Events auf Seitenebene. Kann `true` oder `false` sein. |
| ... **twig:** | Aktiviert Ereignisse auf Twig-Ebene. Kann `true` oder `false` sein. |
| **markdown:** | |
| ... **extra:** | Aktiviert die Unterstützung für Markdown Extra (standardmäßig GitHub-flavored Markdown (GFM)). Kann `true` oder `false` sein. |
| ... **auto_line_breaks:** | Aktiviert automatische Zeilenumbrüche. Kann `true` oder `false` sein. |
| ... **auto_url_links:** | Aktiviert automatische HTML-Links. Kann `true` oder `false` sein. |
| ... **escape_markup:** | Umgehen von Markup-Tags in Entitäten. Kann `true` oder `false` sein. |
| ... **special_chars:** | Liste der Sonderzeichen, die automatisch in Entitäten umgewandelt werden sollen. Jedes Zeichen beansprucht eine eigene Zeile unterhalb dieser Variable. Zum Beispiel: `'>': 'gt'` |
| **types:** | Liste der zulässigen Page-Typen. Zum Beispiel: `[txt,xml,html,htm,json,rss,atom]` |
| **append_url_extension:** | Anhängen der Endung in Seiten-URLs (z.B. `.html` ergibt **/pfad/seite.html**) |
| **expires:** | Laufzeit der Seite in Sekunden (604800 Sekunden = 7 Tage) (`no cache` ist auch möglich) |
| **cache_control:** | Kann leer sein für keine Einstellung oder einen [gültigen Cache-Kontrolltextwert](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) `cache-control` enthalten. |
| **last_modified:** | Legt den Header für das zuletzt geänderte Datum basierend auf dem Zeitstempel der Veränderung der Datei fest. Kann `true` oder `false` sein. |
| **etag:** | Setzt den etag-Header-Tag. Kann `true` oder `false` sein. |
| **vary_accept_encoding:** | Hinzufügen des `Vary: Accept-Encoding` Headers. Kann `true` oder `false` sein. |
| **redirect_default_route:** | Automatische Umleitung auf die Standardroute der Seite. Kann `true` oder `false` sein. |
| **redirect_default_code:** | Standardcode, der für Weiterleitungen zu verwenden ist. Zum Beispiel: `302` |
| **redirect_trailing_slash:** | Automatisches Handling oder 302 Umleitung eines an die URL angehängten / |
| **ignore_files:** | Bei Pages zu ignorierende Dateien. Zum Beispiel: `[.DS_Store] ` |
| **ignore_folders:** | Bei Pages zu ignorierende Ordner. Zum Beispiel: `[.git, .idea]` |
| **ignore_hidden:** | Ignoriert alle versteckten Dateien und Ordner. Kann `true` oder `false` sein. |
| **hide_empty_folders:** | Wenn der Ordner keine .md-Datei enthält, sollte er versteckt werden. Kann `true` oder `false` sein. |
| **url_taxonomy_filters:** | Aktiviert auto-magische URL-basierte Taxonomiefilter für Seitenkollektionen. Kann `true` oder `false` sein. |
| **frontmatter:** | |
| ... **process_twig:** | Soll der Frontmatter verarbeitet werden, um die Twig-Variablen zu ersetzen? Kann `true` oder `false` sein. |
| ... **ignore_fields:** | Felder, die Twig-Variablen enthalten könnten, sollten nicht verarbeitet werden. Zum Beispiel: `['form','forms']` |
[/div]

### Cache

[version=15]
[prism classes="language-yaml line-numbers"]
cache:
  enabled: true
  check:
    method: file
  driver: auto
  prefix: 'g'
  clear_images_by_default: true
  cli_compatibility: false
  lifetime: 604800
  gzip: false
  allow_webserver_gzip: false
  redis:
    socket: false
[/prism]
[/version]

[version=16]
[prism classes="language-yaml line-numbers"]
cache:
  enabled: true
  check:
    method: file
  driver: auto
  prefix: 'g'
  purge_at: '0 4 * * *'
  clear_at: '0 3 * * *'
  clear_job_type: 'standard'
  clear_images_by_default: true
  cli_compatibility: false
  lifetime: 604800
  gzip: false
  allow_webserver_gzip: false
  redis:
    socket: false
[/prism]
[/version]

Im Abschnitt **Cache** können Sie die Cache-Einstellungen der Website konfigurieren. Sie können sie aktivieren, deaktivieren, die Methode wählen und noch einiges mehr.

[version=15]
[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enabled:** |Zum Aktivieren der Caching-Funktion auf `true` setzen. Kann auf `true` oder `false` gesetzt werden. |
| **check:** | |
| ... **method:** | Prüfmethode zum Suchen nach Updates in Pages. Optionen: `file`, `folder`, `hash` und `none`. [Weitere Details](../../advanced/performance-and-caching#grav-core-caching) |
| **driver:** | Einen Cache-Treiber auswählen. Optionen sind: `auto`, `file`, `apcu`, `redis`, `memcache` und `wincache` |
| **prefix:** | Cache-Präfix-String (verhindert Cache-Konflikte). Zum Beispiel: `g` |
| **clear_images_by_default:** | Standardmäßig bindet Grav verarbeitete Bilder mit ein, wenn der Cache gelöscht wird; kann deaktiviert werden, durch Einstellen auf `false`. |
| **cli_compatibility:** | Stellt sicher, dass nur nicht permanente Treiber verwendet werden (file, redis, memcache, usw.) |
| **lifetime:** | Lebensdauer der zwischengespeicherten Daten in Sekunden (`0` = unendlich). `604800` entspricht 7 Tagen |
| **gzip:** | GZip komprimiert die Seitenausgabe. Kann auf `true` oder `false` gesetzt werden. |
| **allow_webserver_gzip:** | Diese Option ändert den Header zu `Content-Encoding: identity`, wodurch gzip vom Webserver zuverlässiger eingerichtet werden kann, obwohl dies normalerweise die Out-of-Process Fähigkeit `onShutDown()` unterbricht.  Das Event wird immer noch abgearbeitet, aber es wird nicht außer Funktion sein und kann die Seite anhalten, bis das Event abgeschlossen ist. |
| **redis:** | |
| **... socket:** | Der Pfad zur Redis-Socket-Datei |
[/div]
[/version]

[version=16]
[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enabled:** |Zum Aktivieren der Caching-Funktion auf `true` setzen. Kann auf `true` oder `false` gesetzt werden. |
| **check:** | |
| ... **method:** | Prüfmethode zum Suchen nach Updates in Pages. Optionen: `file`, `folder`, `hash` und `none`. [Weitere Details](../../advanced/performance-and-caching#grav-core-caching) |
| **driver:** | Einen Cache-Treiber auswählen. Optionen sind: `auto`, `file`, `apcu`, `redis`, `memcache` und `wincache` |
| **prefix:** | Cache-Präfix-String (verhindert Cache-Konflikte). Zum Beispiel: `g` |
| **purge_at:** | Scheduler: Wie oft soll der alte Cache unter Verwendung der Cron-Syntax `at` entleert werden? |
| **clear_at:** | Scheduler: Wie oft soll der Cache unter Verwendung der Cron-Syntax `at` entleert werden? |
| **clear_job_type:** | Der Typ, der bei der Verarbeitung des geplanten Löschjobs gelöscht werden soll. Optionen: `standard` \| `all` |
| **clear_images_by_default:** | Standardmäßig bindet Grav verarbeitete Bilder mit ein, wenn der Cache gelöscht wird; kann deaktiviert werden, durch Einstellen auf `false`. |
| **cli_compatibility:** | Stellt sicher, dass nur nicht permanente Treiber verwendet werden (file, redis, memcache, usw.) |
| **lifetime:** | Lebensdauer der zwischengespeicherten Daten in Sekunden (`0` = unendlich). `604800` entspricht 7 Tagen |
| **gzip:** | GZip komprimiert die Seitenausgabe. Kann auf `true` oder `false` gesetzt werden. |
| **allow_webserver_gzip:** | Diese Option ändert den Header zu `Content-Encoding: identity`, wodurch gzip vom Webserver zuverlässiger eingerichtet werden kann, obwohl dies normalerweise die Out-of-Process Fähigkeit `onShutDown()` unterbricht.  Das Event wird immer noch abgearbeitet, aber es wird nicht außer Funktion sein und kann die Seite anhalten, bis das Event abgeschlossen ist. |
| **redis:** | |
| **... socket:** | Der Pfad zur Redis-Socket-Datei |
[/div]
[/version]

### Twig

[prism classes="language-yaml line-numbers"]
twig:
  cache: true
  debug: true
  auto_reload: true
  autoescape: false
  undefined_functions: true
  undefined_filters: true
  umask_fix: false
[/prism]

Der Abschnitt **Twig** gibt Ihnen einen kurzen Überblick über die Tools, mit denen Sie Twig auf Ihrer Website für Debugging, Caching und Optimierung konfigurieren können.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **cache:** | Setzt man auf `true`, wird das Twig-Caching aktiviert. Kann auf `true` oder `false` gesetzt werden. |
| **debug:** | Twig-Debug aktivieren. Kann auf `true` oder `false` gesetzt werden. |
| **auto_reload:** | Bei Änderungen den Cache aktualisieren. Kann auf `true` oder `false` gesetzt werden. |
| **autoescape:** | Autoescape Twig vars. Kann auf `true` oder `false` gesetzt werden. |
| **undefined_functions:** | Nicht definierte Funktionen zulassen. Kann auf `true` oder `false` gesetzt werden. |
| **undefined_filters:** | Zulassen von undefinierten Filtern. Kann auf `true` oder `false` gesetzt werden. |
| **umask_fix:** | Standardmäßig erstellt Twig Datei-Rechte im Cache mit 755, fix ändert diesen Wert auf 775. Kann auf `true` oder `false` gesetzt werden. |
[/div]

### Assets

[prism classes="language-yaml line-numbers"]
assets:
  css_pipeline: false
  css_pipeline_include_externals: true
  css_pipeline_before_excludes: true
  css_minify: true
  css_minify_windows: false
  css_rewrite: true
  js_pipeline: false
  js_pipeline_include_externals: true
  js_pipeline_before_excludes: true
  js_minify: true
  enable_asset_timestamp: false
  collections:
    jquery: system://assets/jquery/jquery-2.x.min.js
[/prism]

Der Abschnitt **Assets** ermöglicht es Ihnen, Optionen in Verbindung mit dem Asset Manager (JS, CSS) zu konfigurieren.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **css_pipeline:** | Die CSS-Pipeline ist die Zusammenführung mehrerer CSS-Ressourcen in einer Datei. Kann auf `true` oder `false` gesetzt werden. |
| **css_pipeline_include_externals:** | Externe URLs standardmäßig in die Pipeline einbinden. Kann auf `true` oder `false` gesetzt werden. |
| **css_pipeline_before_excludes:** | Die Pipeline wird gerendert, bevor Dateien ausgeschlossen werden. Kann auf `true` oder `false` gesetzt werden. |
| **css_minify:** | Das CSS während des Pipelinings verkleinern. Kann auf `true` oder `false` gesetzt werden. |
| **css_minify_windows:** | Überschreiben für Windows-Plattformen minimieren. Standardmäßig `false` aufgrund der ThreadStackSize. Kann auf `true` oder `false` gesetzt werden. |
| **css_rewrite:** | Beim Pipelining werden alle relativen CSS-URLs umgeschrieben. Kann auf `true` oder `false` gesetzt werden. |
| **js_pipeline:** | Die JS-Pipeline ist die Zusammenfassung mehrerer JS-Ressourcen in einer Datei. Kann auf `true` oder `false` gesetzt werden. |
| **js_pipeline_include_externals:** | Externe URLs standardmäßig in die Pipeline einbinden. Kann auf `true` oder `false` gesetzt werden. |
| **js_pipeline_before_excludes:** | Die Pipeline vor den ausgeschlossenen Dateien rendern. Kann auf `true` oder `false` gesetzt werden. |
| **js_minify:** | JS während des Pipelining minimieren. Kann auf `true` oder `false` gesetzt werden. |
| **enable_asset_timestamp:** | Assets Zeitstempel aktivieren. Kann auf `true` oder `false` gesetzt werden. |
| **collections:** | Darin sind Sammlungen enthalten, die als Sub-Items bezeichnet werden. Zum Beispiel: `jquery: system://assets/jquery/jquery-3.x.min.js` |
[/div]

### Errors

[prism classes="language-yaml line-numbers"]
errors:
  display: 0
  log: true
[/prism]

Der Abschnitt **Errors** bestimmt, wie Grav die Fehleranzeige und -protokollierung verarbeitet.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **display:** | Bestimmt, wie Fehler angezeigt werden. Geben Sie entweder `1` für eine komplette Rückverfolgung, `0` für einen einfachen Fehler oder `-1` für einen Systemfehler ein. |
| **log:** | Fehler im Ordner `/logs` aufzeichnen. Kann auf `true` oder `false` gesetzt werden. |
[/div]

### Log

[prism classes="language-yaml line-numbers"]
log:
  handler: file
  syslog:
    facility: local6
[/prism]

Der Abschnitt **Log** ermöglicht die Konfiguration wechselnder Protokollfunktionen für Grav.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **handler:** | Log-Handler. Derzeit wird unterstützt: `file` \| `syslog` |
| **syslog:** | |
| ... **facility:** | Ausgabe der Syslog-Facilities |
[/div]

### Debugger

[prism classes="language-yaml line-numbers"]
debugger:
  enabled: false
  shutdown:
    close_connection: true
[/prism]

Der Abschnitt **Debugger** gibt Ihnen die Möglichkeit, den Grav-Debugger zu aktivieren. Ein hilfreiches Werkzeug während der Entwicklungsphase.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enabled:** | Enable Grav debugger and following settings. Kann auf `true` oder `false` gesetzt werden. |
| **shutdown:** | |
| ... **close_connection:** | Die Verbindung vor dem Aufruf von `onShutdown()` schließen. `false` zum Debuggen |
[/div]

### Images

[prism classes="language-yaml line-numbers"]
images:
  default_image_quality: 85
  cache_all: false
  cache_perms: '0755'
  debug: false
  auto_fix_orientation: false
  seofriendly: false
[/prism]

Der Abschnitt **Images** gibt Ihnen die Möglichkeit, die Standard-Bildqualität einzustellen, mit der Bilder erneut gesamplet werden, sowie die Bildspeicher- und Debugging-Funktionen zu steuern.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **default_image_quality:** | Standard-Bildqualität, die beim Resampling von Bildern angewandt wird. Zum Beispiel: `85` = 85% |
| **cache_all:** | Alle Bilder standardmäßig in den Cache stellen. Kann auf `true` oder `false` gesetzt werden. |
| **cache_perms:** | ** Muss in Anführungszeichen stehen!** Standardmäßige Zugriffsrechte für Cache-Ordner. Normalerweise `'0755'` or `'0775'` |
| **debug:** | Beim Arbeiten mit Retina eine Einblendung über den Bildern zeigen, die z.B. die Auflösung des Bildes anzeigt. Kann auf `true` oder `false` gesetzt werden. |
| **auto_fix_orientation:** | Versucht automatisch Bilder zu korrigieren, die mit einer nicht-standardmäßigen Drehung hochgeladen wurden. |
| **seofriendly:** | SEO-freundlich aufbereitete Namen von Bildern |
[/div]


### Media

[prism classes="language-yaml line-numbers"]
media:
  enable_media_timestamp: false
  unsupported_inline_types: []
  allowed_fallback_types: []
  auto_metadata_exif: false
[/prism]

Der Abschnitt **Media** behandelt die Konfigurationsoptionen für Einstellungen im Zusammenhang mit dem Umgang mit Mediendateien. Dazu gehören die Anzeige von Zeitstempeln, Upload-Größe und mehr.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enable_media_timestamp:** | Aktivieren der Medienzeitstempel |
| **unsupported_inline_types:** | Array der unterstützten Medientypen, um zu versuchen sie Inline anzuzeigen. Diese Dateitypen werden in `[]` eckige Klammern gesetzt. |
| **allowed_fallback_types:** | Array der erlaubten Datei-Medientypen, die gefunden werden, wenn auf sie über die Page-Route zugegriffen wird. Diese Dateitypen werden in `[]` eckige Klammern gesetzt. |
| **auto_metadata_exif:** | Automatische Erstellung von Metadatendateien aus Exif-Daten, wo es möglich ist. |
[/div]

### Session

[prism classes="language-yaml line-numbers"]
session:
  enabled: true
  initialize: true
  timeout: 1800
  name: grav-site
  uniqueness: path
  secure: false
  httponly: true
  split: true
  path:
[/prism]

Diese Optionen bestimmen die Eigenschaften der Sitzung für Ihre Website.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **enabled:** | Unterstützung für Sitzungen aktivieren. Kann auf `true` oder `false` gesetzt werden. |
| **initialize:** | Sitzung von Grav aus initialisieren (wenn `false`, muss das Plugin die Sitzung starten). |
| **timeout:** | Timeout in Sekunden. Zum Beispiel: `1800` |
| **name:** | Namenspräfix des Session-Cookies. Verwenden Sie nur alphanumerische Zeichen, Bindestriche oder Unterstriche. Verwenden Sie keine Punkte im Sitzungsnamen. Zum Beispiel: `grav-site` |
| **uniqueness:** | Sollen die Sitzungen auf `path` based oder `security.salt` basieren? |
| **secure:** | Sitzungs-Sicherheit einstellen. Wenn `true`, bedeutet dies, dass die Kommunikation für dieses Cookie über eine verschlüsselte Übertragung erfolgen muss. Aktivieren Sie dies nur für Seiten, die ausschließlich über HTTPS laufen. Kann auf `true` oder `false` gesetzt werden. |
| **httponly:** | Ausschließlich HTTP-Sitzung aktivieren. `true` bedeutet, dass Cookies nur über HTTP verwendet werden sollen und eine JavaScript-Modifikation nicht erlaubt ist. Kann auf `true` oder `false` gesetzt werden. |
| **path:** | Speicherpfad für Sitzungen |
[/div]

### GPM

[prism classes="language-yaml line-numbers"]
gpm:
  releases: stable
  proxy_url:
  method: 'auto'
  verify_peer: true
  official_gpm_only: true
[/prism]

Optionen im Abschnitt **GPM** steuern den Grav GPM (Grav Package Manager). Beispielsweise können Sie GPM auf die Verwendung offizieller Quellen beschränken und die Methode auswählen, die GPM zum Herunterladen von Paketen verwendet. Sie können auch zwischen stabilen und Testversionen wählen und eine Proxy-URL einrichten.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **releases:** | Entweder auf `stable` oder `testing` setzen, um festzulegen, ob Sie auf den neuesten Stable- oder Testing-Build updaten wollen. |
| **proxy_url:** | Eine manuelle Proxy-URL für GPM konfigurieren. Zum Beispiel: `127.0.0.1:3128` |
| **method:** | Wahlweise `'curl'`, `'fopen'` oder `'auto'`. `'auto'` versucht zuerst fopen und falls nicht verfügbar cURL. |
| **verify_peer:** | Auf einigen Systemen (meistens Windows) ist GPM nicht in der Lage, eine Verbindung herzustellen, da das SSL-Zertifikat nicht verifiziert werden kann. Die Deaktivierung dieser Option könnte helfen. |
| **official_gpm_only:** | Erlaubt URLs für die GPM-Direktinstallation standardmäßig nur über den offiziellen GPM-Proxy, damit die Sicherheit gewährleistet ist; deaktivieren Sie dies, um andere Quellen zuzulassen. |
[/div]

### Strict Mode

[prism classes="language-yaml line-numbers"]
strict_mode:
  yaml_compat: true
  twig_compat: true
[/prism]

Der Strict-Modus ermöglicht eine sauberere Migration zu zukünftigen Versionen von Grav, indem auf die neueren Versionen von YAML- und Twig-Prozessoren umgestellt wird.  Diese sind möglicherweise nicht mit allen Erweiterungen von Drittanbietern kompatibel.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **yaml_compat:** | Ermöglicht die YAML-Rückwärtskompatibilität |
| **twig_compat:** | Ermöglicht die veraltete Twig-Autoescape-Einstellung. |
[/div]

[version=16]
### Accounts (Konten)

[prism classes="language-yaml line-numbers"]
accounts:
  type: data
  storage: file
[/prism]

Konten ist eine neue Einstellung für Version 1.6, die es Ihnen ermöglicht, die neuen experimentellen Flex-Benutzer auszuprobieren.  Das bedeutet im Wesentlichen, dass die Benutzer als Flex-Objekte gespeichert werden, was mehr Leistung und Performance ermöglicht.

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **type:** | Account-Typ: `data` oder `flex` |
| **storage:** | Flex-Speichertyp: `file` oder `folder` |
[/div]

!! Sie brauchen nicht die **vollständige** Konfigurationsdatei zu kopieren, um sie zu überschreiben. Sie können so wenig oder so viel überschreiben, wie Sie möchten.  Achten Sie nur darauf, dass Sie die **exakte gleiche Struktur zur Benennung** für die jeweilige Einstellung haben, die Sie überschreiben wollen.
[/version]

## Site-Konfiguration

Neben der Datei `system.yaml` bietet Grav auch eine Standardkonfigurationsdatei `site.yaml`, die verwendet wird, um einige spezifische Frontend-Konfigurationen wie Autorenname, Autoren-E-Mail sowie einige wichtige Taxonomie-Einstellungen festzulegen.  Sie können diese auf die gleiche Weise wie in der system.yaml überschreiben, indem Sie Ihre eigene Konfigurationsdatei in `user/config/site.yaml` erstellen. Sie können diese Datei auch dazu verwenden, beliebige eigene Konfigurations-Optionen einzufügen, auf die Sie von Ihrem Inhalt oder Ihren Templates aus verweisen möchten.

Die Standarddatei `system/config/site.yaml`, die mit Grav ausgeliefert wird, sieht in etwa so aus:

[prism classes="language-yaml line-numbers"]
title: Grav                                 # der Site-Name
default_lang: en                            # Standardsprache für die Website (wird möglicherweise auch vom Theme verwendet).

author:
  name: John Appleseed                      # Autorenname (voreingestellt)
  email: 'john@example.com'                 # E-Mail des Autors (voreingestellt)

taxonomies: [category,tag]                  # Beliebige Liste von Taxonomie-Typen

metadata:
  description: 'My Grav Site'               # Beschreibung der Website

summary:
  enabled: true                             # Zusammenfassung der Seite aktivieren oder deaktivieren
  format: short                             # long = das summarische Trennzeichen wird ignoriert; short = das erste vorkommnende Trennzeichen oder die Größenbegrenzung verwenden
  size: 300                                 # Maximale Länge der Zusammenfassung (Buchstaben/Zeichen)
  delimiter: ===                            # Das summarische Trennzeichen

redirects:
#  '/redirect-test': '/'                    # redirect-test wird zur Startseite umgeleitet
#  '/old/(.*)': '/new/$1'                   # Würde /old/my-page zu /new/my-page umleiten

routes:
#  '/something/else': '/blog/sample-3'      # Alias für /blog/sample-3
#  '/new/(.*)': '/blog/$1'                  # Jede Regex /neue/meine_Seite_URL wird zu /blog/meine_Seite weitergeleitet

blog:
  route: '/blog'                            # Individuellen Wert hinzufügen (verfügbar über site. blog. route)

#menu:                                      # Menu-Beispiel
#    - text: Source
#      icon: github
#      url: https://github.com/getgrav/grav
#    - icon: twitter
#      url: http://twitter.com/getgrav
[/prism]

Lassen Sie uns die Elemente dieser Beispieldatei analysieren:

[div class="table-keycol"]
| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **title:** | Der Titel ist eine einfache String-Variable, auf die immer dann verwiesen werden kann, wenn Sie den Namen dieser Website anzeigen möchten. |
| **author:** | |
| ... **name:** | Der Name des Autors der Website, auf den bei Bedarf verwiesen werden kann. |
| ... **email:** | Eine Standard-E-Mail zur Verwendung auf Ihrer Website. |
| **taxonomies:** | Eine beliebige Liste von High-Level-Typen, die Sie zum Organisieren Ihrer Inhalte verwenden können. Sie können Inhalte bestimmten Taxonomietypen zuordnen, z.B. Kategorien oder Tags. Sie können die Einträge bearbeiten oder Ihre eigenen hinzufügen. |
| **metadata:** | Festlegen von Standard-Metadaten für alle Ihre Seiten, siehe Abschnitt [Inhalt Seiten-Headers](../../content/headers) für weitere Einzelheiten. |
| **summary:** | |
| ... **size:** | Das ist ein einfaches URL-Mapping in Grav und bietet simple URL-Alias-Funktionen.  Wenn Sie zu `/etwas/irgendwo` browsen, werden Sie tatsächlich zu `/blog/Muster-3` geschickt.  Bei Bedarf können Sie dieses Mapping bearbeiten oder Ihr eigenes hinzufügen. **Regex-Ersetzungen** (`(.*) - $1`) am Ende von Route-Aliases werden jetzt unterstützt. Für eine optimale Performance sollten diese an das Ende der Liste gesetzt werden. |
| **(custom options)** | Sie können in dieser Datei jede beliebige Option erstellen. Ein gutes Beispiel ist die Option `blog: route: '/blog'`, die in Ihren Twig-Templates mit `site.blog.route` abrufbar ist. |
[/div]

!! Für die meisten Leute ist das wichtigste Element dieser Datei die `Taxonomie`-Liste.  Die Taxonomien in dieser Liste **müssen** hier definiert werden, wenn Sie sie in Ihren Inhalten verwenden wollen.

## Security

In Grav 1.5 haben wir eine neue Datei `system/config/security.yaml` eingeführt, die einige sinnvolle Standardeinstellungen festlegt und vom Admin-Plugin beim **Speichern** von Inhalten[version=16] verwendet wird, sowie im neuen Abschnitt **Berichte** unter **Werkzeuge**[/version].

Die Voreinstellung für die Konfiguration sieht wie folgt aus:

[prism classes="language-yaml line-numbers"]
xss_whitelist: [admin.super]
xss_enabled:
    on_events: true
    invalid_protocols: true
    moz_binding: true
    html_inline_styles: true
    dangerous_tags: true
xss_invalid_protocols:
    - javascript
    - livescript
    - vbscript
    - mocha
    - feed
    - data
xss_dangerous_tags:
    - applet
    - meta
    - xml
    - blink
    - link
    - style
    - script
    - embed
    - object
    - iframe
    - frame
    - frameset
    - ilayer
    - layer
    - bgsound
    - title
    - base
uploads_dangerous_extensions:
    - php
    - html
    - htm
    - js
    - exe
[/prism]

Wenn Sie Änderungen an diesen Einstellungen vornehmen möchten, sollten Sie diese Datei nach `user/config/security.yaml` kopieren und **dort** die Änderungen vornehmen.

## Andere Einstellungen und Dateien für die Konfiguration

Die Benutzerkonfiguration ist vollkommen optional. Die Standardeinstellungen können beliebig oft oder so wenig wie nötig verändert werden. Dies gilt sowohl für das System, die Site als auch für alle Plugin-Konfigurationen auf Ihrer Site.

Sie sind auch nicht, wie oben beschrieben, auf die Dateien `user/config/system.yaml` oder `user/config/site.yaml` beschränkt. Sie können jede beliebige `. yaml` Konfigurations-Datei im Ordner `user/config` erstellen, die Sie wünschen. Sie wird dann automatisch von Grav erkannt.

So könnte zum Beispiel die neue Konfigurationsdatei `user/config/data.yaml` heißen und eine yaml-Variable in dieser Datei count heißen:

```
count: 39
```

Auf die Variable würde in Ihrem Twig-Template mit Hilfe der folgenden Syntax zugegriffen werden:

```
{{ config.data.count }}
```

Es wäre auch über PHP aus jedem Plugin mit dem dazugehörigen Code zugänglich:

```
$count_var = Grav::instance()['config']->get('data.count');
```

! Sie können auch ein benutzerdefiniertes Blueprint erstellen, damit Ihre angepasste Datei im Admin-Plugin bearbeitet werden kann. Sehen Sie sich das entsprechende [Rezept im Abschnitt Admin-Kochbuch](/cookbook/admin-recipes#add-a-custom-yaml-file) an.

### Namensraum von Variablen konfigurieren

Die Pfade zu den Konfigurationsdateien werden als **Namensraum** für Ihre Einstellmöglichkeiten verwendet.

Alternativ können Sie alle Optionen in einer Datei zusammenfassen und mit Hilfe von YAML-Strukturen die Hierarchie für Ihre Konfigurationsparameter festlegen. Dieser Namensraum wird aus einer Kombination aus **Pfad + Dateiname + Optionsname** gebildet.

Ein Beispiel: Die Option, wie z.B. `author: Frank Smith` in der Datei `plugins/myplugin.yaml` könnte über `plugins.myplugin.autor` aufgerufen werden. Sie könnten jedoch auch eine Datei `plugins.yaml` verwenden und in dieser Datei eine Option namens `myplugin: author: Frank Smith` haben und sie wäre immer noch über den gleichen Namensraum `plugins.myplugin.author` erreichbar.

Einige Konfigurationsdateien könnten beispielsweise so strukturiert sein:

[div class="table-keycol"]
| Datei | Beschreibung |
| -------- | ----------- |
| **user/config/system.yaml**           | Globale Konfigurationsdatei für das System                |
| **user/config/site.yaml**             | Eine Konfigurationsdatei für die spezifische Website      |
| **user/config/plugins/myplugin.yaml** | Individuelle Konfigurationsdatei für das Plugin myplugin  |
| **user/config/themes/mytheme.yaml**   | Individuelle Konfigurationsdatei für das Theme Mytheme    |
[/div]

!! Eine Konfigurationsdatei im Namensraum überschreibt oder maskiert alle Optionen, die den gleichen Pfad in den Standard-Konfigurationsdateien angegeben haben.

### Plugins-Konfiguration

Die meisten **Plugins** werden mit einer eigenen YAML-Konfigurationsdatei installiert. Wir empfehlen, diese Datei in das Verzeichnis `user/config/plugins/` zu kopieren, anstatt die Konfigurationsoptionen direkt in der Datei im Verzeichnis des Plugins zu editieren. Auf diese Weise stellen Sie sicher, dass ein Update des Plugins Ihre Einstellungen nicht überschreibt, und behalten alle Ihre konfigurierbaren Optionen an einem geeigneten Speicherort.

Wenn Sie ein Plugin namens `user/plugins/myplugin` haben, das eine Konfigurationsdatei namens `user/plugins/myplugin/myplugin.yaml` hat, dann würden Sie diese Datei nach `user/config/plugins/myplugin.yaml` kopieren und die Datei dort bearbeiten.

Als Fallback dient die im Primärverzeichnis des Plugins vorhandene YAML-Datei. Alle Einstellungen, die dort und nicht in der Kopie des User-Ordners aufgeführt sind, werden von Grav übernommen und verwendet.

### Themes-Konfiguration

Für **Themes** gelten die gleichen Regeln wie für Plugins.  Wenn Sie also ein Theme mit dem Namen `user/themes/mytheme` haben, das eine Konfigurationsdatei mit Namen `user/themes/mytheme/mytheme.yaml` hat, dann würden Sie diese Datei nach `user/config/themes/mytheme.yaml` kopieren und die Datei dort editieren.
