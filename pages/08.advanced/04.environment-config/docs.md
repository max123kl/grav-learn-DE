---
title: Umgebungs-Konfiguration
taxonomy:
    category: docs
---

Grav hat nun die Fähigkeit, die [leistungsstarken Konfigurationsmöglichkeiten](../../basics/grav-configuration) für verschiedene Umgebungen zu erweitern, um für **Entwicklungs-, Staging- und Produktionsszenarien** unterschiedliche Konfigurationen zu erstellen.

### Automatische Umgebungs-Konfiguration

Das bedeutet, dass Sie so wenig oder so viele Konfigurationsänderungen pro Umgebung vornehmen können, wie erforderlich sind. Ein gutes Beispiel dafür ist der Debug-Block. Standardmäßig ist der neue [Debugger-Block](../debugging) in der Core-Datei `system/config/system.yaml` und auch in der Benutzer-Override-Datei deaktiviert:

[prism classes="language-bash command-line"]
user/config/system.yaml
[/prism]

Wenn Sie es aktivieren möchten, können Sie es leicht in Ihrer Datei `user/config/system.yaml` einrichten. Eine bessere Lösung besteht aber darin, es in Ihrer Entwicklungsumgebung zu aktivieren (_enabled_), wenn Sie über **localhost** zugreifen, aber auf Ihrem **Produktionsserver** zu deaktivieren (_disabled_).

Dies lässt sich leicht durch einfaches Übersteuern der Einstellung in dieser Datei erreichen:

[prism classes="language-bash command-line"]
user/localhost/config/system.yaml
[/prism]

wobei `localhost` der Hostname der jeweiligen Arbeitsumgebung ist (das ist der Host, den Sie in Ihrem Browser eingeben, z.B. http://localhost/your-site) und in der Ihre Konfigurationsdatei liegt:

[prism classes="language-yaml line-numbers"]
debugger:
  enabled: true
[/prism]

Ebenso kann es sinnvoll sein, CSS und Js Asset Pipelining (Kombination + Minifikation) nur für Ihre Produktions-Site (`user/www.mysite.com/config/system.yaml`) zu aktivieren:

[prism classes="language-yaml line-numbers"]
assets:
  css_pipeline: true
  js_pipeline: true
[/prism]

Sollte Ihr Produktionsserver über `http://www.mysite.com` erreichbar sein, könnten Sie auch eine für diesen Produktionsstandort spezifische Konfiguration mit einer Datei unter `user/www.mysite.com/config/system.yaml` bereitstellen.

Natürlich sind Sie nicht auf Änderungen an der `system.yaml` beschränkt. Sie können eigentlich für jede Grav-Einstellung in der `site.yaml` oder sogar in jeder [Plugin-Konfiguration](../../plugins/plugin-basics) Overrides vornehmen!

#### Plugin-Overrides

Das Übersteuern einer YAML-Datei für eine Plugin-Konfiguration ist genauso einfach wie das Übersteuern einer regulären Datei. Befindet sich die Standard-Konfigurationsdatei in:

[prism classes="language-bash command-line"]
user/config/plugins/email.yaml
[/prism]

Dann können Sie diese mit einer Einstellung übersteuern, die nur bestimmte Optionen aufhebt, die Sie für lokale Tests verwenden möchten:

[prism classes="language-bash command-line"]
user/localhost/config/plugins/email.yaml
[/prism]

beispielsweise mit der Einstellung:

[prism classes="language-yaml line-numbers"]
mailer:
  engine: smtp
  smtp:
    server: smtp.mailtrap.io
    port: 2525
    encryption: none
    user: '9a320798e65135'
    password: 'a13e6e27bc7205'
[/prism]

#### Theme-Overrides

Sie können Themes auf nahezu die gleiche Weise übersteuern:

[prism classes="language-bash command-line"]
user/config/themes/antimatter/antimatter.yaml
[/prism]

Kann für jede beliebige Umgebung übersteuert werden, z.B. für eine Produktions-Site (`http://www.mysite.com`):

[prism classes="language-bash command-line"]
user/www.mysite.com/config/themes/antimatter/antimatter.yaml
[/prism]
