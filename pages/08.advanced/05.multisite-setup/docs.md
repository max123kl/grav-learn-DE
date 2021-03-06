---
title: Multisite-Setup
taxonomy:
    category: docs
---

!! In Grav ist eine vorläufige Multisite-Unterstützung verfügbar.  Allerdings müssen die CLI-Befehle sowie das Admin-Plugin noch aktualisiert werden, um Multisite-Konfigurationen vollständig zu unterstützen.  Wir werden in nachfolgenden Versionen von Grav weiter daran arbeiten.

### What is a Multisite Setup?

> Eine Multisite-Konfiguration ermöglicht es Ihnen, ein System aus mehreren Websites zu erstellen und zu verwalten, die alle mit einer einzigen Installation laufen.

Grav verfügt über integrierte Multisite-Unterstützung. Diese Funktionalität erweitert die [grundlegende Umgebungskonfiguration](../environment-config), mit der Sie benutzerdefinierte Umgebungen für Ihre Produktions- und Entwicklungssites definieren können.

Eine vollständige Multisite-Einrichtung gibt Ihnen die Kontrolle darüber, wie und von wo Grav alle seine Dateien lädt.

### Voraussetzungen für ein Grav-Multisite-Setup

Das Wichtigste für den Betrieb einer Multi-Website mit Grav ist ein gutes Website-Hosting. Wenn Sie nicht vorhaben, viele Websites zu erstellen und nicht mit vielen Besuchern rechnen, dann können Sie mit Shared Hosting auskommen. Aufgrund der Natur von Multisites werden Sie jedoch wahrscheinlich einen VPS oder einen dedizierten Server benötigen, wenn Ihre Sites größer werden.

### Setup und Installation

Bevor Sie beginnen, sollten Sie sicher sein, dass Ihr Webserver in der Lage ist, mehrere Websites zu betreiben, d.h. dass Sie Zugriff auf Ihr Grav-Root-Verzeichnis haben.

Das ist entscheidend, da die Unterstützung mehrerer Websites aus derselben Installation heraus auf der Datei `setup.php` basiert, die sich in Ihrer Grav-Root befindet.

#### Schnelleinstieg (für Anfänger)

Einmal angelegt, wird die `setup.php` jedes Mal aufgerufen, wenn ein Benutzer eine Seite aufruft. Um mehrere Websites von einer einzigen Installation aus bedienen zu können, muss dieses Skript (grob gesagt) Grav mitteilen, wo sich die Dateien (für die Konfigurationen, Themes, Plugins, Seiten usw.) für eine bestimmte Subsite befinden.

Die unten angegebenen Code-Schnipsel richten Ihre Grav-Installation so ein, dass eine Anfrage wie

[prism classes="language-text"]
http://<subsite>.example.com   -->   user/sites/<subsite>.example.com
[/prism]
or
[prism classes="language-text"]
http://example.com/<subsite>   -->   user/sites/<subsite>
[/prism]

das Verzeichnis `user/sites` als Basispfad für den „User“ anstelle des Verzeichnisses `user` verwendet.

Wenn Sie Unterverzeichnisse oder pfadbasierte URLs für Unterseiten wählen, dann ist das Einzige, was Sie brauchen, ein Verzeichnis für jede Unterseite im Verzeichnis `user/sites` zu erstellen, das die mindestens erforderlichen Ordner `config`, `pages`, `plugins` und `themes` enthält.

Wenn Sie Sub-Domains für die Strukturierung Ihres Website-Netzwerks wählen, dann müssen Sie zusätzlich zur Einrichtung Ihrer Sub-Sites in Ihrem Verzeichnis `user/sites` Sub-Domains auf Ihrem Server konfigurieren (Wildcard).

Wie auch immer, entscheiden Sie, welches der beiden Optionen für Sie am besten geeignet ist.

##### Snippets

Für Sub-Sites, die über Sub-Domains erreichbar sind, kopieren Sie die Datei `setup_subdomain.php`, andernfalls für Sub-Sites, die über Unterverzeichnisse erreichbar sind, die Datei `setup_subdirectory.php` in Ihre `setup.php`.

!!! Die Datei `setup.php` muss in das Grav-Wurzelverzeichnis gelegt werden, in das gleiche Verzeichnis, in dem sich `index.php`, `README.md` und die anderen Grav-Dateien befinden.

**setup_subdomain.php**:
[prism classes="language-php line-numbers"]
<?php
/**
 * Multisite setup for subsites accessible via sub-domains.
 *
 * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
 */

use Grav\Common\Utils;

// Get subsite name from sub-domain
$environment = isset($_SERVER['HTTP_HOST'])
    ? $_SERVER['HTTP_HOST']
    : (isset($_SERVER['SERVER_NAME']) ? $_SERVER['SERVER_NAME'] : 'localhost');
// Remove port from HTTP_HOST generated $environment
$environment = strtolower(Utils::substrToString($environment, ':'));
$folder = "sites/{$environment}";

if ($environment === 'localhost' || !is_dir(ROOT_DIR . "user/{$folder}")) {
    return [];
}

return [
    'environment' => $environment,
    'streams' => [
        'schemes' => [
            'user' => [
               'type' => 'ReadOnlyStream',
               'prefixes' => [
                   '' => ["user/{$folder}"],
               ]
            ]
        ]
    ]
];
[/prism]

**setup_subdirectory.php**:
[prism classes="language-php line-numbers"]
<?php
/**
 * Multisite setup for sub-directories or path based
 * URLs for subsites.
 *
 * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
 */

use Grav\Common\Filesystem\Folder;

// Get relative path from Grav root.
$path = isset($_SERVER['PATH_INFO'])
   ? $_SERVER['PATH_INFO']
   : Folder::getRelativePath($_SERVER['REQUEST_URI'], ROOT_DIR);

// Extract name of subsite from path
$name = Folder::shift($path);
$folder = "sites/{$name}";
$prefix = "/{$name}";

if (!$name || !is_dir(ROOT_DIR . "user/{$folder}")) {
    return [];
}

// Prefix all pages with the name of the subsite
$container['pages']->base($prefix);

return [
    'environment' => $name,
    'streams' => [
        'schemes' => [
            'user' => [
               'type' => 'ReadOnlyStream',
               'prefixes' => [
                   '' => ["user/{$folder}"],
               ]
            ]
        ]
    ]
];
[/prism]

Wenn Sie Unterverzeichnisse zum Umschalten von Sprachen benutzen, müssen Sie möglicherweise je nach Sprache unterschiedliche Konfigurationen laden.
Sie können Ihre sprachspezifischen Konfigurationen in `config/<lang-context>/site.yaml` platzieren, indem Sie das Beispiel für `setup_subdir_config_switch.php` unten verwenden.
Auf diese Weise würde `yoursite.com/de-AT/index.html` `config/de-AT/site.yaml` laden, `yoursite.com/de-CH/index.html` würde `config/de-CH/site.yaml` laden und so weiter.

**setup_subdir_config_switch.php**:
[prism classes="language-php line-numbers"]
<?php
/**
 * Switch config based on the language context subdir
 *
 * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
 */

use Grav\Common\Filesystem\Folder;

$languageContexts = [
    'de-AT',
    'de-CH',
    'de-DE',
];

// Relativen Pfad von der Grav-Root abfragen.
$path = isset($_SERVER['PATH_INFO'])
    ? $_SERVER['PATH_INFO']
    : Folder::getRelativePath($_SERVER['REQUEST_URI'], ROOT_DIR);

// Name des Unterverzeichnisses aus dem Pfad extrahieren
$name = Folder::shift($path);

if (in_array($name, $languageContexts)) {
    return [
        'streams' => [
            'schemes' => [
                'config' => [
                    'type' => 'ReadOnlyStream',
                    'prefixes' => [
                        '' => [
                            'environment://config',
                            'user://config/' . $name,
                            'user://config',
                            'system/config',
                        ],
                    ],
                ],
            ],
        ],
    ];
}

return [];
[/prism]

#### Erweiterte Konfiguration (für Experten)

Sobald eine `setup.php` erstellt wurde, haben Sie Zugriff auf zwei wichtige Variablen:  
1.) `$container`, welche die noch nicht vollständig initialisierte [Grav-Instanz](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Grav.php) ist, und  
2.) `$self`, welche eine Instanz der [Klasse „ConfigServiceProvider“](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Service/ConfigServiceProvider.php) ist.

Innerhalb dieses Skripts können Sie eigentlich alles verändern, aber beachten Sie bitte, dass die `setup.php` jedes Mal aufgerufen wird, wenn ein Benutzer eine Seite anfordert. Das bedeutet, dass speicherkritische oder zeitraubende Initialisierungsoperationen zu einer Verlangsamung Ihres gesamten Systems führen und deshalb vermieden werden sollten.

Am Ende muss die `setup.php` ein assoziatives Array mit dem optionalen Umgebungsnamen **environment** und die Stream-Kollektion **streams** zurückgeben
(mehr Informationen und für die korrekte Einrichtung siehe den Abschnitt [Streams](#streams)):

[prism classes="language-php line-numbers"]
return [
  'environment' => '<name>',            // A name for the environment
  'streams' => [
    'schemes' => [
      '<stream_name>' => [              // The name of the stream
        'type' => 'ReadOnlyStream',     // Stream object e.g. 'ReadOnlyStream' or 'Stream'
        'prefixes' => [
          '<prefix>' => [
            '<path1>',
            '<path2>',
            '<etc>'
          ]
        ],
        'paths' => [                    // Paths (optional)
          '<paths1>',
          '<paths2>',
          '<etc>'
        ]
      ]
    ]
  ]
]

[/prism]

!!!! Bitte beachten Sie, dass Sie in diesem sehr frühen Stadium weder Zugriff auf die Konfiguration noch auf die URI-Instanz haben und daher jeder Aufruf einer nicht initialisierten Klasse zu einem Einfrieren des Systems, zu unerwarteten Fehlern oder zu einem (vollständigen) Datenverlust führen kann.

#### Streams

In Grav sind Streams Objekte, die eine Gruppe von physikalischen Verzeichnissen des Systems auf ein logisches Device mappen. Sie werden über ihr `type`-Attribut klassifiziert. Für schreibgeschützte Streams ist das der Typ `ReadOnlyStream` und für schreibgeschützte Streams der Typ `Stream`. Sie können jeden benutzerdefinierten Streamtyp registrieren und auf ihn verweisen, solange es sich um eine Instanz der Schnittstellenklasse [StreamInterface](https://github.com/rockettheme/toolbox/blob/develop/StreamWrapper/src/StreamInterface.php) handelt.

By default, streams have been configured like this:

* `user://` - Benutzer-Verzeichnis z.B. `user/`
* `page://` - Seiten-Verzeichnis z.B. `user://pages/`
* `image://` - Bilder-Verzeichnis z.B. `user://images/`, `system://images/`
* `account://` - Accounts-Verzeichnis z.B. `user://accounts/`
* `environment://` - aktueller Multi-Site-Speicherort
* `asset://` - JS/CSS-Verzeichnis kompiliert z.B. `assets/`
* `blueprints://` - Blueprints-Verzeichnis z.B. `environment://blueprints/`, `user://blueprints/`, `system://blueprints/`
* `config://` - Konfigurations-Verzeichnis z.B. `environment://config/`, `user://config/`, `system://config/`
* `plugins://` - Plugins-Verzeichnis z.B. `user://plugins/`
* `themes://` - aktuelles Theme z.B. `user://themes/`
* `theme://` - aktuelles Theme z.B. `themes://antimatter/`
* `languages://` - Sprachen-Verzeichnis z.B. `environment://languages/`, `user://languages/`, `system://languages/`
* `user-data://` - Daten-Verzeichnis z.B. `user/data/`
* `system://` - System-Verzeichnis z.B. `system/`
* `cache://` - Cache-Verzeichnis z.B. `cache/`, `images/`
* `log://` - Logdateien-Verzeichnis z.B. `logs/`
* `backup://` - Backup-Verzeichnis z.B. `backups/`
* `tmp://` - Temp-Verzeichnis z.B. `tmp/`

Die Zuordnung von physischen Verzeichnissen zu einem logischen Device kann auf zwei Arten erfolgen, entweder durch die Einrichtung von „Pfaden“ (`paths`) oder „Präfixen“ (`prefixes`). Erstere kann als eine 1-zu-1 Zuordnung verstanden werden, während letztere, wie es der Name andeutet, Ihnen erlaubt, mehrere physische Pfade zu einem logischen Stream zu kombinieren. Nehmen wir an, Sie möchten einen Stream mit dem Namen „image“ definieren. Sie können dann mit dem Stream `images://` und

[prism classes="language-php line-numbers"]
'image' => [
    'type' => 'ReadOnlyStream',
    'paths' => [
        'user/images',
        'system/images'
    ]
];
[/prism]

alle Bilder auflisten, die sich in den Ordnern `user/images` und `system/images` befinden. Für **prefixes** betrachten Sie das Beispiel

[prism classes="language-php line-numbers"]
'cache' => [
    'type' => 'Stream',
    'prefixes' => [
        '' => ['cache'],
        'images' => ['images']
    ]
];
[/prism]

In diesem Beispiel entspricht `cache://` dem Begriff `cache`, während `cache://images` dem Begriff `images` entspricht.

Nicht zuletzt können Streams auch in anderen Streams eingesetzt werden. Wenn beispielsweise ein Stream `user` und ein Stream `system` existiert, kann der obige „image“-Stream auch wie folgt formuliert werden:

[prism classes="language-php line-numbers"]
'image' => [
    'type' => 'ReadOnlyStream',
    'paths' => [
        'user://images',
        'system://images'
    ]
];

[/prism]

[version=17]
### Server Based Multi-Site Configuration

Grav 1.7 adds support to customize initial environment from your server configuration.

This feature comes handy if you want to use for example docker containers and you want to make them independent of the domain you happen to use. Or if do not want to store secrets in the configuration, but to store them in your server setup.

The following environment variables can be used to customize the default paths which Grav uses to setup the environment. After initialization the streams may point to different location.

!!! **Note:** You can use either environment variables or PHP constants, but they need to be set before Grav runs.

[div class="table-keycol"]
| Variable | Default | Description |
| -------- | ------- | ----------- |
| **GRAV_SETUP_PATH** | AUTO DETECT | A custom path to `setup.php` file including the filename. By default Grav looks the file from `GRAV_ROOT/setup.php` and `GRAV_ROOT/GRAV_USER_PATH/setup.php`. |
| **GRAV_USER_PATH** | `user` | A relative path for `user://` stream. |
| **GRAV_CACHE_PATH** | `cache` | A relative path for `cache://` stream. |
| **GRAV_LOG_PATH** | `logs` | A relative path for `log://` stream. |
| **GRAV_TMP_PATH** | `tmp` | A relative path for `tmp://` stream. |
| **GRAV_BACKUP_PATH** | `backup` | A relative path for `backup://` stream. |
[/div]

In addition there are variables to customize the environments. Better documentation for these can be found in [Server Based Environment Configuration](/advanced/environment-config#server-based-environment-configuration).

!!! **Note:** These work also from `setup.php` file. You can either make them constants by using `define()` or environment variables with `putenv()`. Constants are preferred over environment variables.

[div class="table-keycol"]
| Variable | Default | Description |
| -------- | ------- | ----------- |
| **GRAV_ENVIRONMENT** | DOMAIN NAME | Environment name. Can be used for example in docker containers to set a custom environment which does not rely domain name, such as `production` and `develop`. |
| **GRAV_ENVIRONMENTS_PATH** | `user://env` | Lookup path for all environments if you do prefer something like `user://sites`. Can be either a stream or relative path from `GRAV_ROOT`. |
| **GRAV_ENVIRONMENT_PATH** | `user://env/ENVIRONMENT` | Sometimes it may be useful to have a custom location for your environment. |
[/div]

#### Server Based Configuration Overrides

If you do not wish to store secret credentials inside the configuration, you can also provide them by using environment variables from your server.

As environmental variables have strict naming requirements (they can only contain A-Z, a-z, 0-9 and _), some tricks are needed to get the configuration overrides to work.

Here is an example of a simple configuration override using YAML format for presentation:

```yaml
GRAV_CONFIG: true                           # If false, the configuration here will be ignored.

GRAV_CONFIG_ALIAS__GITHUB: plugins.github   # Create alias GITHUB='plugins.github' to shorten the variable names below

GRAV_CONFIG__GITHUB__auth__method: api      # Override config.plugins.github.auth.method = api
GRAV_CONFIG__GITHUB__auth__token: xxxxxxxx  # Override config.plugins.github.auth.token = xxxxxxxx
```

In above example `__` (double underscore) represents nested variable, which in twig is represented with `.` (dot).

You can also use environment variables in `setup.php`. This allows you for example to store secrets outside the configuration:

`user/setup.php`:
```php
<?php

// Use following environment variables in your server configuration:
//
// DYNAMODB_SESSION_KEY: DynamoDb server key for the PHP session storage
// DYNAMODB_SESSION_SECRET: DynamoDb server secret
// DYNAMODB_SESSION_REGION: DynamoDb server region
// GOOGLE_MAPS_KEY: Google Maps secret key

return [
    'plugins' => [
        // This plugin does not exist
        'dynamodb_session' => [
            'credentials' => [
                'key' => getenv('DYNAMODB_SESSION_KEY') ?: null,
                'secret' => getenv('DYNAMODB_SESSION_SECRET') ?: null
            ],
            'region' => getenv('DYNAMODB_SESSION_REGION') ?: null
        ],
        // This plugin does not exist
        'google_maps' => [
            'key' => getenv('GOOGLE_MAPS_KEY') ?: null
        ]
    ]
];
```

!! **WARNING:** `setup.php` is used to set initial configuration. If the plugin or your configuration later override these settings, the initial values get lost.

After defining the variables in `setup.php`, you can then set those in your server:

[ui-tabs]
[ui-tab title="Apache 2"]
[prism classes="language-apacheconf line-numbers"]
<VirtualHost 127.0.0.1:80>
    ...

    SetEnv GRAV_SETUP_PATH         user/setup.php
    SetEnv GRAV_ENVIRONMENT        production
    SetEnv DYNAMODB_SESSION_KEY    JBGARDQ06UNJV00DL0R9
    SetEnv DYNAMODB_SESSION_SECRET CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
    SetEnv DYNAMODB_SESSION_REGION us-east-1
    SetEnv GOOGLE_MAPS_KEY         XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
</VirtualHost>
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-fpm"]
[prism classes="language-nginx line-numbers"]
location / {
    ...

    fastcgi_param GRAV_SETUP_PATH         user/setup.php;
    fastcgi_param GRAV_ENVIRONMENT        production;
    fastcgi_param DYNAMODB_SESSION_KEY    JBGARDQ06UNJV00DL0R9;
    fastcgi_param DYNAMODB_SESSION_SECRET CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7;
    fastcgi_param DYNAMODB_SESSION_REGION us-east-1;
    fastcgi_param GOOGLE_MAPS_KEY         XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp;
}
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-cgi"]
[prism classes="language-nginx line-numbers"]
location / {
...

    env[GRAV_SETUP_PATH]          = user/setup.php
    env[GRAV_ENVIRONMENT]         = production
    env[DYNAMODB_SESSION_KEY]     = JBGARDQ06UNJV00DL0R9
    env[DYNAMODB_SESSION_SECRET]  = CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
    env[GDYNAMODB_SESSION_REGION] = us-east-1
    env[GGOOGLE_MAPS_KEY]         = XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
}
[/prism]
[/ui-tab]
[ui-tab title="Docker"]
[prism classes="language-yaml line-numbers"]
web:
  environment:
    - GRAV_SETUP_PATH=user/setup.php
    - GRAV_ENVIRONMENT=production
    - DYNAMODB_SESSION_KEY=JBGARDQ06UNJV00DL0R9
    - DYNAMODB_SESSION_SECRET=CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
    - DYNAMODB_SESSION_REGION=us-east-1
    - GOOGLE_MAPS_KEY=XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
[/prism]
[/ui-tab]
[ui-tab title="PHP"]
[prism classes="language-php line-numbers"]
putenv('GRAV_SETUP_PATH', 'user/setup.php');
putenv('GRAV_ENVIRONMENT', 'production');
putenv('DYNAMODB_SESSION_KEY', 'JBGARDQ06UNJV00DL0R9');
putenv('DYNAMODB_SESSION_SECRET', 'CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7');
putenv('DYNAMODB_SESSION_REGION', 'us-east-1');
putenv('GOOGLE_MAPS_KEY', 'XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp');
[/prism]
[/ui-tab]
[/ui-tabs]

In this example, server will also use `production` environment stored in `user/env/production` folder.

[/version]
