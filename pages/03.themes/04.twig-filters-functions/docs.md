---
title: Twig-Filter und -Funktionen
page-toc:
  active: true
  depth: 3
process:
    twig: true
taxonomy:
    category: docs
---

Obwohl Twig bereits eine umfangreiche Liste von [Filtern, Funktionen und Tags](http://twig.sensiolabs.org/documentation) bereitstellt, bietet auch Grav eine Auswahl nützlicher Ergänzungen, um den Prozess der Theme-Gestaltung zu erleichtern.

!! Wenn Sie Informationen zur Entwicklung eigener [benutzerdefinierter Twig-Filter](/cookbook/twig-recipes/#custom-twig-filter-function) benötigen, sehen Sie sich das Beispiel für benutzerdefinierte Twig-Filter/Funktionen im Abschnitt **Twig-Rezepte** im **Kochbuch-Kapitel** an.


## Grav Twig-Filter

Twig-Filter werden in Twig-Variablen eingesetzt. Dabei wird das Zeichen `|` gefolgt vom Filternamen benutzt.  Parameter können genau wie Twig-Funktionen mit Hilfe von geschweiften Klammern übergeben werden.

#### Absolute URL

Nehmen Sie einen relativen Pfad und konvertieren Sie ihn in ein absolutes URL-Format einschließlich des Hostnamens.

`'<img src="/some/path/to/image.jpg" />'|absolute_url` <i class="fa fa-long-arrow-right"></i> `{{ '<img src="/some/path/to/image.jpg" />'|absolute_url }}`

#### Array Unique

Wrapper für PHP `array_unique()`, der Duplikate aus einem Array entfernt.

`['foo', 'bar', 'foo', 'baz']|array_unique` <i class="fa fa-long-arrow-right"></i> **{{ print_r(['foo', 'bar', 'foo', 'baz']|array_unique) }}**

#### Base32 Encode

Durchführung einer base32-Kodierung auf der Variable
`'some variable here'|base32_encode` <i class="fa fa-long-arrow-right"></i> **{{ 'some variable here'|base32_encode }}**

#### Base32 Decode

Durchführung einer base32-Dekodierung auf der Variable
`'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGK'|base32_decode` <i class="fa fa-long-arrow-right"></i> **{{ 'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGK'|base32_decode }}**

#### Base64 Encode

Durchführung einer base64-Kodierung auf der Variable
`'some variable here'|base64_encode` <i class="fa fa-long-arrow-right"></i> **{{ 'some variable here'|base64_encode }}**

#### Base64 Decode

Durchführung einer base64-Decodierung auf der Variable
`'c29tZSB2YXJpYWJsZSBoZXJl'|base64_decode` <i class="fa fa-long-arrow-right"></i> **{{ 'c29tZSB2YXJpYWJsZSBoZXJl'|base64_decode }}**

#### Basename

Den Basis-Namen eines Pfades zurückgeben.

`'/etc/sudoers.d'|basename` <i class="fa fa-long-arrow-right"></i> **{{ '/etc/sudoers.d'|basename }}**

#### Camelize

Konvertiert eine Zeichenfolge in das "CamelCase"-Format

`'send_email'|camelize` <i class="fa fa-long-arrow-right"></i> **{{ 'send_email'|camelize }}**

[version=16]
#### Chunk Split

Teilt eine Zeichenkette in kleinere Stücke einer vorgegebenen Größe auf.

`'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGKA'|chunk_split(6, '-')` <i class="fa fa-long-arrow-right"></i> **{{ 'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGKA'|chunk_split(6, '-') }}**
[/version]

#### Contains

Feststellen, ob eine bestimmte Zeichenfolge eine andere Zeichenfolge enthält.

`'some string with things in it'|contains('things')` <i class="fa fa-long-arrow-right"></i> **{{ 'some string with things in it'|contains('things') }}**

#### Werte übergeben

In PHP 7 werden strengere Typprüfungen eingeführt, was bedeutet, dass das Übergeben eines Wertes vom falschen Typ nun eine Exception auslösen kann. Um dies zu vermeiden, sollten Sie Filter verwenden, die sicherstellen, dass der an eine Variable übergebene Wert gültig ist:

**|string**

Wert als Zeichenfolge übergeben

**|int**

Wert als Integer übergeben

**|bool**

Den Wert auf boolean setzen

**|float**

Wert als Fließkommazahl übergeben

**|array**

Wert in ein Array übertragen

#### Defined

Manchmal will man überprüfen, ob eine Variable definiert ist, und wenn dies nicht der Fall ist, einen Standardwert angeben.  Zum Beispiel:

`set header_image_width  = page.header.header_image_width|defined(900)`

Das wird die Variable `header_image_width` auf den Wert `900` setzen, wenn sie nicht im Page-Header definiert ist.

#### Dirname

Gibt den Verzeichnisnamen eines Pfades zurück

`'/etc/sudoers.d'|dirname` <i class="fa fa-long-arrow-right"></i> **{{ '/etc/sudoers.d'|dirname }}**


#### Ends-With

Man nimmt eine Nadel und einen Heuhaufen und ermittelt, ob der Heuhaufen mit der Nadel abschließt. Das funktioniert jetzt auch mit einem Array aus Nadeln und gibt `true` zurück, wenn **ein beliebiger** Heuhaufen mit der Nadel endet.

`'the quick brown fox'|ends_with('fox')` <i class="fa fa-long-arrow-right"></i> {{  'the quick brown fox'|ends_with('fox') ? 'true' : 'false' }}

#### FieldName

Filtert Feldnamen durch Umwandlung von Punktnotation in Array-Notation

`'field.name|fieldName`


[version=16]
#### Get Type

Ruft den Typ einer Variablen ab:

`page|get_type` <i class="fa fa-long-arrow-right"></i> **{{ page|get_type }}**
[/version]

#### Humanize

Konvertiert eine Zeichenkette in ein „menschenlesbareres“ Format.

`'something_text_to_read'|humanize` <i class="fa fa-long-arrow-right"></i> **{{ 'something_text_to_read'|humanize }}**

#### Hyphenize

Konvertiert eine Zeichenkette in eine Fassung mit Bindestrich.

`'Something Text to Read'|hyphenize` <i class="fa fa-long-arrow-right"></i> **{{ 'Something Text to Read'|hyphenize }}**

#### JSON Decode

Sie können JSON dekodieren, indem Sie einfach diesen Filter anwenden:

`{"first_name": "Guido", "last_name":"Rossum"}|json_decode`

#### Ksort

Eine Array-Map nach den verschiedenen Schlüsseln sortieren

`array|ksort` {% verbatim %}
[prism classes="language-twig line-numbers"]
{% set ritems = {'orange':1, 'apple':2, 'peach':3}|ksort %}
{% for key, value in ritems %}{{ key }}:{{ value }}, {% endfor %}
[/prism]
{% endverbatim %}

{% set ritems = {'orange':1, 'apple':2, 'peach':3}|ksort %}
[prism classes="language-twig line-numbers"]
{% for key, value in ritems %}{{ key }}:{{ value }}, {% endfor %}
[/prism]

#### Left Trim

`'/strip/leading/slash/'|ltrim('/')` <i class="fa fa-long-arrow-right"></i> {{ '/strip/leading/slash/'|ltrim('/') }}

Entfernt Leerzeichen am Anfang einer Zeichenfolge. Er kann auch andere Zeichen entfernen, indem die Zeichen-Maske definiert wird (siehe [https://php.net/manual/de/function.ltrim.php](https://php.net/manual/de/function.ltrim.php))

#### Markdown

Nehmen Sie eine beliebige Zeichenfolge, die Markdown enthält, und konvertieren Sie sie mit dem Markdown-Parser von Grav in HTML. Optionaler `boolean` Parameter:

* `true` (Standard): Verarbeitung als Block (Textmodus, Inhalt wird in `<p>` Tags eingebettet).
* `false`: Verarbeitung als Zeile (Inhalt wird nicht eingebettet)

```
string|markdown($is_block)
```

{% verbatim %}
[prism classes="language-twig line-numbers"]
<div class="div">
{{ 'A paragraph with **markdown** and [a link](http://www.cnn.com)'|markdown }}
</div>

<p class="paragraph">{{'A line with **markdown** and [a link](http://www.cnn.com)'|markdown(false) }}</p>
[/prism]
{% endverbatim %}

<div class="div">
{{ 'A paragraph with **markdown** and [a link](http://www.cnn.com)'|markdown }}
</div>

<p class="paragraph">{{'A line with **markdown** and [a link](http://www.cnn.com)'|markdown(false) }}</p>

#### MD5

Erzeugt einen md5-Hash für die Zeichenkette

`'anything'|md5` <i class="fa fa-long-arrow-right"></i> **{{ 'anything'|md5 }}**

#### Modulus

Führt die gleiche Funktionalität wie das Modulus `%`-Symbol in PHP aus. Es arbeitet mit einer Zahl, indem es einen numerischen Teiler und ein optionales Array von Elementen zur Auswahl übergibt.

`7|modulus(3, ['red', 'blue', 'green'])` <i class="fa fa-long-arrow-right"></i> **{{ 7|modulus(3, ['red', 'blue', 'green']) }}**

#### Monthize

Wandelt eine ganzzahlige Anzahl von Tagen in eine Anzahl von Monaten um

`'181'|monthize` <i class="fa fa-long-arrow-right"></i> **{{ '181'|monthize }}**

[version=16]
#### NiceCron

Ruft eine menschenlesbare Ausgabe für die Cron-Syntax ab

`"2 * * * *"|nicecron` <i class="fa fa-long-arrow-right"></i> **{{ '2 * * * *'|nicecron }}**

#### NiceFilesize

Dateigröße in einem menschenlesbaren Nice-Size-Format ausgeben

`612394|nicefilesize` <i class="fa fa-long-arrow-right"></i> **{{ 612394|nicefilesize }}**

#### NiceNumber

Ausgabe einer Zahl in einem menschenlesbaren, ansprechenden Zahlenformat

`12430|nicenumber` <i class="fa fa-long-arrow-right"></i> **{{ 12430|nicenumber }}**
[/version]

#### NiceTime

Ausgabe eines Datums in einem menschenlesbaren, ansprechenden Zeitformat

`page.date|nicetime(false)` <i class="fa fa-long-arrow-right"></i> **{{ page.date|nicetime(false) }}**

[version=16]
#### Of Type

Prüft den Typ einer Variablen zum Parameter

`page|of_type('string')` <i class="fa fa-long-arrow-right"></i> **{{ page|of_type('string') ? 'true' : 'false' }}**
[/version]

#### Ordinalize

Wandelt eine Ganzzahl in eine Ordnungszahl (wie 1st, 2nd, 3rd, 4th)

`'10'|ordinalize` <i class="fa fa-long-arrow-right"></i> **{{ '10'|ordinalize }}**

#### Pad

Ergänzt eine Zeichenfolge bis zu einer angebenen Länge mit einem anderen Zeichen. Das ist ein Wrapper für die PHP-Funktion [str_pad()](https://php.net/manual/de/function.str-pad.php)

`'foobar'|pad(10, '-')` <i class="fa fa-long-arrow-right"></i> **{{ 'foobar'|pad(10, '-') }}**

#### Pluralize

Konvertiert eine Zeichenfolge in die englische Plural-Version

`'person'|pluralize` <i class="fa fa-long-arrow-right"></i> **{{ 'person'|pluralize }}**

[version=17]
#### Preg Match

Wrapper für die PHP-Funktion [preg_match()](https://www.php.net/manual/de/function.preg-match.php), die einen Text durch ein Pattern abgleicht, das als regulärer Ausdruck angegeben ist. Gibt die Übereinstimmung(en) zurück, wenn es mindestens eine Übereinstimmung gibt oder andernfalls `false`.
[/version]

[version=17]
#### Preg Split

Wrapper für die PHP-Funktion [preg_split()](https://www.php.net/manual/en/function.preg-split.php), die einen Text anhand eines regulären Ausdrucks zerlegt.
[/version]

[version=16]
#### Print Variable

Gibt menschenlesbare Informationen über eine Variable aus

`page.header|print_r`

[prism classes="language-text"]
{{ page.header|print_r }}
[/prism]
[/version]

#### Randomize

Gibt die bereitgestellte Liste nach dem Zufallsprinzip aus.  Wenn ein Wert als Parameter angegeben wird, überspringt er diese Werte und hält die Reihenfolge ein.

`array|randomize` {% verbatim %}
[prism classes="language-twig line-numbers"]
{% set ritems = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten']|randomize(2) %}
{% for ritem in ritems %}{{ ritem }}, {% endfor %}
[/prism]
{% endverbatim %}

<strong>
{% set ritems = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten']|randomize(2) %}
{% for ritem in ritems %}{{ ritem }}, {% endfor %}
</strong>

#### Regex Replace

Als hilfreicher Wrapper für die PHP-Funktion [preg_replace()](https://php.net/manual/en/function.preg-replace.php) können Sie über diesen Filter komplexe Regex-Ersetzungen an Text vornehmen:

`'The quick brown fox jumps over the lazy dog.'|regex_replace(['/quick/','/brown/','/fox/','/dog/'], ['slow','black','bear','turtle'])` <i class="fa fa-long-arrow-right"></i> **{{ 'The quick brown fox jumps over the lazy dog.'|regex_replace(['/quick/','/brown/','/fox/','/dog/'], ['slow','black','bear','turtle']) }}**

! Verwenden Sie nach Möglichkeit den `~`-Begrenzer statt des `/`-Begrenzers. Andernfalls müssen Sie höchstwahrscheinlich [<u>bestimmte Zeichen doppelt escapen</u>](https://github.com/getgrav/grav/issues/833). z.B. `~\/\#.*~` statt `'/\\/\\#.*/'`, was besser zu der von PHP verwendeten [PCRE-Syntax](https://www.php.net/manual/de/regexp.reference.delimiters.php) passt.

#### Right Trim

`'/strip/trailing/slash/'|rtrim('/')` <i class="fa fa-long-arrow-right"></i> {{ '/strip/trailing/slash/'|rtrim('/') }}

Entfernt Leerzeichen am Ende eines Strings. Sie kann auch andere Zeichen entfernen, indem sie die Zeichenmaske einstellen (siehe [https://php.net/manual/de/function.rtrim.php](https://php.net/manual/de/function.rtrim.php))

#### Singularize

Konvertiert eine Zeichenfolge in die englische Singularversion

`'shoes'|singularize` <i class="fa fa-long-arrow-right"></i> **{{ 'shoes'|singularize }}**

#### Safe Email

Der Safe E-Mail-Filter wandelt eine E-Mail-Adresse in ASCII-Zeichen um, um es für E-Mail-Spam-Bots schwieriger zu machen, sie zu erkennen und zu sammeln.

`"someone@domain.com"|safe_email` <i class="fa fa-long-arrow-right"></i> {{ "someone@domain.com"|safe_email }}

Anwendungsbeispiel mit einem mailto-Link:

[prism classes="language-html line-numbers"]
<a href="mailto:{{'your.email@server.com'|safe_email}}">
  Email me
</a>
[/prism]

Möglicherweise bemerken Sie zunächst keinen Unterschied, aber wenn Sie den Seitenquelltext untersuchen (nicht über die Browser-Entwickler-Tools, sondern den eigentlichen Seitenquelltext), können Sie die darunter liegende Zeichenkodierung erkennen.

#### Sort by Key

Sortieren einer Array-Map nach einem bestimmten Schlüssel

`array|sort_by_key` {% verbatim %}
[prism classes="language-twig line-numbers"]
{% set people = [{'email':'fred@yahoo.com', 'id':34}, {'email':'tim@exchange.com', 'id':21}, {'email':'john@apple.com', 'id':2}]|sort_by_key('id') %}
{% for person in people %}{{ person.email }}:{{ person.id }}, {% endfor %}
[/prism]
{% endverbatim %}

<strong>
{% set people = [{'email':'fred@yahoo.com', 'id':34}, {'email':'tim@exchange.com', 'id':21}, {'email':'john@apple.com', 'id':2}]|sort_by_key('id') %}
{% for person in people %}{{ person.email }}:{{ person.id }}, {% endfor %}
</strong>

#### Starts-With

Man nehme eine Nadel und einen Heuhaufen und fragt ob der Heuhaufen mit der Nadel startet. Das funktioniert jetzt auch mit einer Gruppe von Nadeln und gibt `true` zurück, wenn **irgendein** Heuhaufen mit der Nadel beginnt.

`'the quick brown fox'|starts_with('the')` <i class="fa fa-long-arrow-right"></i> {{  'the quick brown fox'|starts_with('the') ? 'true' : 'false' }}

#### Titleize

Konvertiert eine Zeichenfolge in das Format „Title Case“ (Großschreibung am Anfang aller Wörter)

`'welcome page'|titleize` <i class="fa fa-long-arrow-right"></i> **{{ 'welcome page'|titleize }}**


#### Übersetzung

Eine Zeichenfolge in die aktuelle Sprache übersetzen

`'MY_LANGUAGE_KEY_STRING'|t` <i class="fa fa-long-arrow-right"></i> 'Some Text in English'

Dies setzt voraus, dass Sie diese Sprachstrings auf Ihrer Website übersetzt haben und die Mehrsprachigkeit aktiviert haben.  Ausführlichere Informationen finden Sie in der [Dokumentation zur Mehrsprachigkeit](../../content/multi-language).

#### Admin-Übersetzung

Übersetzt eine Zeichenfolge in die aktuelle Sprache, die in den Benutzer-Einstellungen der Admin-Oberfläche eingestellt ist

`'MY_LANGUAGE_KEY_STRING'|tu` <i class="fa fa-long-arrow-right"></i> 'Some Text in English'

Dabei wird das Sprachfeld ausgelesen, das in der yaml-Datei des Benutzers gesetzt ist.

#### Array übersetzen

Übersetzt ein Array mit einer Sprache mit Hilfe des `|ta` Filters. Ein ausführliches Beispiel finden Sie in der [Dokumentation für mehrsprachige Sites](../../content/multi-language).

`'MONTHS_OF_THE_YEAR'|ta(post.date|date('n') - 1)` <i class="fa fa-long-arrow-right"></i> **{{ now|date('F') }}**

#### Sprache übersetzen

Übersetzt eine Zeichenfolge in eine bestimmte Sprache. Weitere Einzelheiten finden Sie in der [Dokumentation für mehrsprachige Sites](../../content/multi-language#Komplexe-Übersetzungen).

`'SIMPLE_TEXT'|tl(['fr'])`

#### Einen String beschneiden

Mit diesem Filter können Sie leicht eine verkürzte, abgeschnittene Version einer Zeichenfolge erzeugen.  Er nimmt eine Anzahl von Zeichen als einziges erforderliches Feld, hat aber noch einige andere Optionen:

`'one sentence. two sentences'|truncate(5)` <i class="fa fa-long-arrow-right"></i> {{ 'one sentence. two sentences'|truncate(5) }}

Wird einfach auf 5 Zeichen gekürzt

`'one sentence. two sentences'|truncate(5, true)` <i class="fa fa-long-arrow-right"></i> {{ 'one sentence. two sentences'|truncate(5, true) }}

Wird nach 5 Zeichen zum nächsten Satzende gekürzt.

Sie können HTML-Text auch abschneiden, sollten aber zuerst den Filter `striptags` verwenden, um alle HTML-Formatierungen zu entfernen, die defekt werden könnten, wenn Sie zwischen Tags enden:

`'<p>one <strong>sentence</strong>. two sentences</p>'|striptags|truncate(5)` <i class="fa fa-long-arrow-right"></i> {{ '<p>one <strong>sentence</strong>. two sentences</p>'|striptags|truncate(5) }}

##### Spezielle Versionen:

**|safe_truncate**

Beschneidet den Text nach der Anzahl der Zeichen in einer „wortsicheren“ Form.

**|truncate_html**

HTML nach Anzahl der Zeichen kürzen, nicht „wortsicher“!

**|safe_truncate_html**

HTML nach der Anzahl der Zeichen in einer „wortsicheren“ Form kürzen.

#### Underscoreize

Konvertiert einen String in ein Format mit „Unterstrichen“

`'CamelCased'|underscorize` <i class="fa fa-long-arrow-right"></i> **{{ 'CamelCased'|underscorize }}**

[version=16]
#### Yaml Encode (Yaml kodieren)

Dump/Kodieren einer Variablen in die YAML-Syntax

`{foo: [0,1,2,3], baz: 'qux' }|yaml_encode`

[prism classes="language-twig"]
{{ {foo: [0,1,2,3], baz: 'qux' }|yaml_encode }}
[/prism]

#### Yaml Decode (Yaml dekodieren)

Dekodieren/Parsen einer Variablen aus der YAML-Syntax

{% verbatim %}
`{% set yaml = "foo: [0, 1, 2, 3]\nbaz: qux" %}`

`yaml|yaml_decode`
{% endverbatim %}

{% set yaml = "foo: [0, 1, 2, 3]\nbaz: qux" %}
[prism classes="language-twig"]
{{ yaml|yaml_decode|var_dump}}
[/prism]
[/version]


## Grav-Twig-Funktionen

Twig-Funktionen werden direkt aufgerufen, wobei alle Parameter durch Klammern übergeben werden.

#### Array

Einen Wert an ein Array übergeben

`array(value)`

#### Array Key Value

Die Funktion `array_key_value` erlaubt es Ihnen, ein Schlüssel/Werte-Paar einem verknüpften Array beizufügen

{% verbatim %}
[prism classes="language-twig line-numbers"]
{% set my_array = {fruit: 'apple'} %}
{% set my_array = array_key_value('meat','steak', my_array) %}
{{ print_r(my_array)}}
[/prism]
{% endverbatim %}

{% set my_array = {fruit: 'apple'} %}
{% set my_array = array_key_value('meat','steak', my_array) %}
outputs: ** {{ print_r(my_array) }} **

#### Array Key Exists

Wrapper für die PHP-Funktion `array_key_exists`, die zurückliefert, ob ein Schlüssel in einem zugehörigen Array existiert oder nicht.

{% verbatim %}
[prism classes="language-twig line-numbers"]
{% set my_array = {fruit: 'apple', meat: 'steak'} %}
{{ array_key_exists('meat', my_array) }}
[/prism]
{% endverbatim %}

{% set my_array = {fruit: 'apple', meat: 'steak'} %}
Ausgabe: **{{ array_key_exists('meat', my_array) }}**

#### Array-Schnittmenge

Die Funktion `array_intersect` liefert die Schnittmenge von zwei Arrays oder Grav-Kollektionen.

{% verbatim %}
[prism classes="language-twig line-numbers"]
{% set array_1 = {fruit: 'apple', meat: 'steak'} %}
{% set array_2 = {fish: 'tuna', meat: 'steak'} %}
{{ print_r(array_intersect(array_1, array_2)) }}
[/prism]
{% endverbatim %}

{% set array_1 = {fruit: 'apple', meat: 'steak'} %}
{% set array_2 = {fish: 'tuna', meat: 'steak'} %}

Ausgabe: **{{ print_r(array_intersect(array_1, array_2)) }}**

#### Eindeutiges Array

Wrapper für die PHP-Funktion `array_unique()`, der Duplikate aus einem Array entfernt

`array_unique(['foo', 'bar', 'foo', 'baz'])` <i class="fa fa-long-arrow-right"></i> **{{ print_r(array_unique(['foo', 'bar', 'foo', 'baz'])) }}**

#### Authorize (Berechtigung)

Berechtigt einen authentifizierten Benutzer, eine Ressource aufzurufen. Akzeptiert einen einzelnen Berechtigungs-String oder ein Array von Berechtigungs-Strings

`authorize(['admin.statistics', 'admin.super'])`

[version=16]
#### Body-Klasse

Erfordert ein Array von Klassen und falls sie nicht auf `body_classes` gesetzt sind, wird überprüft ob diese in der aktuellen Theme-Konfiguration definiert sind.

`set body_classes = body_class(['header-fixed', 'header-animated', 'header-dark', 'header-transparent', 'sticky-footer'])`

#### Cron

Erstellt ein „Cron“-Objekt aus der Cron-Syntax

`cron("3 * * * *").getNextRunDate()|date(config.date_format.default)`

[/version]


#### Dump

Nimmt eine gültige Twig-Variable und gibt sie im [Grav-Debugger-Panel](../../advanced/debugging) aus. Der Debugger muss **aktiviert** sein, um die Werte in der Nachrichten-Registerkarte sehen zu können.

`dump(page.header)`

#### Debug

Das Gleiche wie `dump()`

#### Evaluate (Auswertung)

Die Auswertefunktion kann verwendet werden, um eine Zeichenfolge als Twig-Code auszuwerten:

`evaluate('grav.language.getLanguage')`

#### Evaluate Twig

Ähnlich wie `evaluate`, wird aber mit Twig ausgewertet und verarbeitet

{% verbatim %}
`evaluate_twig('This is a twig variable: {{ foo }}', {foo: 'bar'})`)  <i class="fa fa-long-arrow-right"></i> **This is a twig variable: bar**
{% endverbatim %}

#### EXIF

Ausgabe der EXIF-Daten eines Bildes, basierend auf dessen Dateipfad. Dies erfordert, dass in der Datei `system.yaml` die Option `media: auto_metadata_exif: true` gesetzt ist. Zum Beispiel in einem Twig-Template:

{% verbatim %}
[prism classes="language-twig line-numbers"]
{% set image = page.media['sample-image.jpg'] %}
{% set exif = exif(image.filepath, true) %}
{{ exif.MaxApertureValue }}
[/prism]
{% endverbatim %}

Dadurch würde der in der Kamera vorhandene `MaxApertureValue`-Wert (größte Blende, Lichtstärke des Oblektivs) ausgegeben, zum Beispiel „4.0“. Sie können immer `{% verbatim %}{{ dump(exif)}}{% endverbatim %}` verwenden, um alle verfügbaren Daten im Debugger anzuzeigen.

#### Get Cookie

Mit dieser Funktion können Sie den Wert eines Cookies abfragen:

`get_cookie('your_cookie_key')`

[version=16]
#### Funktion „Typ“ abrufen

Ruft den Typ einer Variablen ab:

`get_type(page)` <i class="fa fa-long-arrow-right"></i> **{{ get_type(page) }}**
[/version]

#### Gist

Übernimmt eine Github Gist-ID und erstellt den entsprechenden Gist-Einbettungscode

`gist('bc448ff158df4bc56217')` <i class="fa fa-long-arrow-right"></i> {{ gist('bc448ff158df4bc56217')}}

[version=16]
#### HTTP-Response-Code

Wenn response_code vorhanden ist, dann wird der vorherige Statuscode zurückgegeben. Wenn response_code nicht angegeben wird, dann wird der aktuelle Statuscode zurückgegeben. Beide Werte werden standardmäßig auf einen 200-Statuscode gesetzt, wenn sie in einer Webserver-Umgebung verwendet werden.

`http_response_code(404)`

[/version]

#### Is Ajax Request (Ajax-Abfrage)

Man kann die Funktion `isajaxrequest()` verwenden, um zu prüfen, ob die Header-Option `HTTP_X_REQUESTED_WITH` gesetzt ist:


#### JSON Decode-Funktion

Man kann JSON dekodieren, indem man diesen Filter anwendet:

`json_decode({"first_name": "Guido", "last_name":"Rossum"})`

#### Medien-Verzeichnis

Gibt ein Medienobjekt für ein beliebiges Verzeichnis zurück.  Einmal erhalten, können Bilder auf ähnliche Weise wie Seiten manipuliert werden.

`media_directory('theme://images')['some-image.jpg'].cropResize(200,200).html`

[version=16]
#### NiceFilesize-Funktion

Ausgabe einer Dateigröße in einem menschenlesbaren Nice-Size-Format

`nicefilesize(612394)` <i class="fa fa-long-arrow-right"></i> **{{ nicefilesize(612394) }}**

#### NiceNumber-Funktion

Ausgabe einer Zahl in einem menschenlesbaren, ansprechenden Zahlenformat

`nicenumnber(12430)` <i class="fa fa-long-arrow-right"></i> **{{ nicenumber(12430)}}**

#### NiceTime-Funktion

Ausgabe eines Datums in einem menschenlesbaren, ansprechenden Zeitformat

`nicetime(page.date)` <i class="fa fa-long-arrow-right"></i> **{{ nicetime(page.date) }}**
[/version]

#### Nonce-Feld

Generiert das Grav-Sicherheitsfeld __nonce__ für ein Formular mit einer erforderlichen `action`:

`nonce_field('action')` <i class="fa fa-long-arrow-right"></i> **{{ nonce_field('action')|e }}**

[version=16]
#### Of-Type-Funktion

Prüft den Typ einer Variablen zum Parameter:

`of_type(page, 'string')` <i class="fa fa-long-arrow-right"></i> **{{ of_type(page, 'string') ? 'true' : 'false' }}**
[/version]

#### Pathinfo

Parst einen Pfad in einem Array

{% verbatim %}
[prism classes="language-twig"]
{% set parts = pathinfo('/www/htdocs/inc/lib.inc.php') %}
{{ print_r(parts) }}
[/prism]
{% endverbatim %}

{% set parts = pathinfo('/www/htdocs/inc/lib.inc.php') %}

outputs: **{{ print_r(parts) }}**

[version=16]
#### Print-Variable-Funktion

Gibt eine Variable in einem lesbaren Format aus

`print_r(page.header)`

[prism classes="language-twig"]
{{ print_r(page.header) }}
[/prism]

[/version]

#### Erzeugung zufälliger Strings

Generiert einen zufälligen String mit der erforderlichen Anzahl von Zeichen.  Besonders nützlich bei der Erstellung einer eindeutigen ID oder eines Schlüssels.

`random_string(10)` <i class="fa fa-long-arrow-right"></i> **{{ random_string(10) }}**

#### Range (Bereich)

Erzeugt ein Array, das eine Gruppe von Elementen enthält, optional abgestuft

`range(25, 300, 50)` <i class="fa fa-long-arrow-right"></i> **{{ print_r(range(25, 300, 50)) }}**

[version=16]
#### Read File (Datei auslesen)

Einfache Funktion, um eine Datei anhand eines Dateipfades zu lesen und auszugeben.

`read_file('plugins://admin/README.md')|markdown`

[prism classes="language-markdown line-numbers"]
# Grav Standard Administration Panel Plugin

This **admin plugin** for [Grav](https://github.com/getgrav/grav) is an HTML user interface that provides a convenient way to configure Grav and easily create and modify pages...
[/prism]

[/version]

#### Redirect Me

Leitet zu einer URL Ihrer Wahl weiter

`redirect_me('http://google.com', 304)`

[version=16]
#### Regex-Filter-Funktion

Führt ein `preg_grep` in einem Array mit einem RegEx-Pattern aus

`regex_filter(['pasta', 'fish', 'steak', 'potatoes'], "/p.*/")`

[prism classes="language-twig"]
{{ var_dump(regex_filter(['pasta', 'fish', 'steak', 'potatoes'], "/p.*/")) }}
[/prism]

#### Regex-Ersetzen-Funktion

Ein nützlicher Wrapper für die PHP-Funktion [`preg_replace()`](https://php.net/manual/en/function.preg-replace.php). Mit diesem Filter können Sie mit regulären Ausdrücken komplexe Ersetzungen am Text vornehmen:

`regex_replace('The quick brown fox jumps over the lazy dog.', ['/quick/','/brown/','/fox/','/dog/'], ['slow','black','bear','turtle'])`

[prism classes="language-twig"]
{{ regex_replace('The quick brown fox jumps over the lazy dog.', ['/quick/','/brown/','/fox/','/dog/'], ['slow','black','bear','turtle']) }}
[/prism]

[/version]

#### Repeat (Wiederholen)

Wird alles wiederholen, was in einer angegebenen Zeitspanne durchlaufen wird.

`repeat('blah ', 10)` <i class="fa fa-long-arrow-right"></i> **{{ repeat('blah ', 10) }}**

#### String

Gibt einen String aus einem Wert zurück. Wenn der Wert ein Array ist, wird er json-kodiert zurückgegeben

`string(23)` => `"23"`
`string(['test' => 'x'])` => `{"test":"x"}`

[version=16]
#### Theme-Variable

Liest eine Theme-Variable aus dem Seitenkopf aus, falls sie existiert, ansonsten wird die Theme-Konfiguration verwendet:

`theme_var('grid-size')`

Dies wird zuerst `page.header.grid-size` testen. Falls das nicht gesetzt ist, wird `theme.grid-size` aus der Konfigurations-Datei des Themes verwendet. Optional kann eine Voreinstellung gewählt werden:

`theme_var('grid-size', 1024)`

[/version]

#### Übersetzen-Funktion

Übersetzt einen String, wie der Filter `|t`

`t('SITE_NAME')` <i class="fa fa-long-arrow-right"></i> **Site Name**

#### Array übersetzen Funktion

Diese Funktion bezieht sich auf den `|ta` Filter

#### Sprache übersetzen Funktion

Übersetzt einen String in eine bestimmte Sprache. Weitere Einzelheiten finden Sie in der [Dokumentation für mehrsprachige Sites](../../content/multi-language#Komplexe-Übersetzungen).

`tl('SIMPLE_TEXT', ['fr'])`

#### Url

Erstellt eine URL und wandelt alle PHP-URL-Streams in gültige HTML-Ressourcen um. Für den Fall, dass die URL nicht aufgelöst werden kann, kann ein Standardwert übergeben werden.

`url('theme://images/logo.png')|default('http://www.placehold.it/150x100/f4f4f4')` <i class="fa fa-long-arrow-right"></i> **{{ url('theme://images/logo.png')|default('http://www.placehold.it/150x100/f4f4f4') }}**

#### VarDump

Die Funktion `vardump()` gibt die aktuelle Variable auf dem Bildschirm aus (und nicht im Debugger wie bei `dump()`)

{% verbatim %}
[prism classes="language-twig line-numbers"]
{% set my_array = {foo: 'bar', baz: 'qux'} %}
{{ vardump(my_array)}}
[/prism]
{% endverbatim %}

{% set my_array = {foo: 'bar', baz: 'qux'} %}
{{ vardump(my_array)}}

[version=16]
#### XSS

Ermöglicht eine manuelle Prüfung eines Strings auf XSS-Sicherheitslücken

`xss('this string contains a <script>alert("hello");</script> XSS vulnerability')` <i class="fa fa-long-arrow-right"></i> **{{ xss('this string contains a <script>alert("hello");</script> XSS vulnerability') }}**

[/version]
