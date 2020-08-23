---
title: Multisite-Setup
taxonomy:
    category: docs
---

!! Erste Unterstützung für den Multisite-Support ist seit Grav 1.0 verfügbar.  Allerdings müssen sowohl die CLI-Befehle als auch das Admin-Plugin noch aktualisiert werden, um Multisite-Konfigurationen vollständig zu unterstützen.  Wir werden in nachfolgenden Grav-Versionen weiter daran arbeiten.

### What is a Multisite Setup?

> Eine Multisite-Konfiguration ermöglicht es Ihnen, ein System aus mehreren Websites zu erstellen und zu verwalten, die alle mit einer einzigen Installation laufen.

Grav hat eine integrierte Multisite-Unterstützung. Im Gegensatz zur [automatischen Umgebungs-Konfiguration](../environment-config), bei der Sie benutzerdefinierte Umgebungen definieren können, um verschiedene Konfigurationen und Szenarien zu unterstützen, gibt Ihnen eine Multisite-Konfiguration die Möglichkeit, die Art und Weise zu ändern, wie und von wo Grav alle seine Dateien lädt.

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

Die Zuordnung von physischen Verzeichnissen zu einem logischen Device kann auf zwei Arten erfolgen, entweder durch die Einrichtung von „Pfaden“ (`paths`) oder „Präfixen“ (`prefixes`). Erstere kann als eine 1-zu-1 Zuordnung verstanden werden, während letztere, wie es der Name andeutet, Ihnen erlaubt, mehrere physische Pfade zu einem logischen Stream zu kombinieren. Nehmen wir an, Sie möchten einen Stream mit dem Namen "image" definieren. Sie können dann mit dem Stream `images://` mit

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
