---
title: Umgebungs-Konfiguration
taxonomy:
    category: docs
---

Grav hat nun die Fähigkeit, die [leistungsstarken Konfigurationsmöglichkeiten](../../basics/grav-configuration) für verschiedene Umgebungen zu erweitern, um für **Entwicklungs-, Staging- und Produktionsszenarien** unterschiedliche Konfigurationen zu erstellen.

[version=17]
!! Bis zu Grav 1.6 wurden Environments im Ordner `user/` gespeichert. Mit Grav 1.7 werden diese nun in den Ordner `user/env/` verschoben, um die Wartung der Environments zu vereinfachen. Es wird dringend empfohlen, dass Sie alle Environments in diesen neuen Speicherort auf Ihren bereits vorhandenen Websites verschieben.
[/version]

### Automatische Umgebungs-Konfiguration

Das bedeutet, dass Sie so wenig oder so viele Konfigurationsänderungen pro Umgebung vornehmen können, wie erforderlich sind. Ein gutes Beispiel dafür ist der Debug-Block. Standardmäßig ist der neue [Debugger-Block](../debugging) in der Core-Datei `system/config/system.yaml` und auch in der Benutzer-Override-Datei deaktiviert:

[prism classes="language-bash command-line"]
user/config/system.yaml
[/prism]

Wenn Sie es aktivieren möchten, können Sie es leicht in Ihrer Datei `user/config/system.yaml` einrichten. Eine bessere Lösung besteht aber darin, es in Ihrer Entwicklungsumgebung zu aktivieren (_enabled_), wenn Sie über **localhost** zugreifen, aber auf Ihrem **Produktionsserver** zu deaktivieren (_disabled_).

Dies lässt sich leicht durch einfaches Übersteuern der Einstellung in dieser Datei erreichen:

[version=15,16]
[prism classes="language-bash command-line"]
user/localhost/config/system.yaml
[/prism]
[/version]
[version=17]
[prism classes="language-bash command-line"]
user/env/localhost/config/system.yaml
[/prism]
[/version]

wobei `localhost` der Hostname der jeweiligen Arbeitsumgebung ist (das ist der Host, den Sie in Ihrem Browser eingeben, z.B. http://localhost/your-site) und in der Ihre Konfigurationsdatei liegt:

[prism classes="language-yaml line-numbers"]
debugger:
  enabled: true
[/prism]

Ebenso kann es sinnvoll sein, CSS und Js Asset Pipelining (Kombination + Minifikation) nur für Ihre Produktions-Site
([version=15,16]`user/www.mysite.com/config/system.yaml`[/version][version=17]`user/env/www.mysite.com/config/system.yaml`[/version]) zu aktivieren:

[prism classes="language-yaml line-numbers"]
assets:
  css_pipeline: true
  js_pipeline: true
[/prism]

Sollte Ihr Produktionsserver über `http://www.mysite.com` erreichbar sein, könnten Sie auch eine für diesen Produktionsstandort spezifische Konfiguration mit einer Datei unter
[version=15,16]`user/www.mysite.com/config/system.yaml`[/version][version=17]`user/env/www.mysite.com/config/system.yaml`[/version] bereitstellen.

Natürlich sind Sie nicht auf Änderungen an der `system.yaml` beschränkt. Sie können eigentlich für jede Grav-Einstellung in der `site.yaml` oder sogar in jeder [Plugin-Konfiguration](../../plugins/plugin-basics) Overrides vornehmen!

!! Wenn Sie den Grav-[Scheduler](/advanced/scheduler) verwenden, beachten Sie, dass er die Systemumgebung `localhost` verwendet und somit seine Konfiguration.

#### Plugin-Overrides

Das Übersteuern einer YAML-Datei für eine Plugin-Konfiguration ist genauso einfach wie das Übersteuern einer regulären Datei. Befindet sich die Standard-Konfigurationsdatei in:

[prism classes="language-bash command-line"]
user/config/plugins/email.yaml
[/prism]

Dann können Sie diese mit einer Einstellung übersteuern, die nur bestimmte Optionen aufhebt, die Sie für lokale Tests verwenden möchten:

[version=15,16]
[prism classes="language-bash command-line"]
user/localhost/config/plugins/email.yaml
[/prism]
[/version]
[version=17]
[prism classes="language-bash command-line"]
user/env/localhost/config/plugins/email.yaml
[/prism]
[/version]

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

[version=15,16]
[prism classes="language-bash command-line"]
user/www.mysite.com/config/themes/antimatter/antimatter.yaml
[/prism]
[/version]
[version=17]
[prism classes="language-bash command-line"]
user/env/www.mysite.com/config/themes/antimatter/antimatter.yaml
[/prism]

### Server Based Environment Configuration

Starting from Grav 1.7, it is possible to set the environment by using server configuration. In this use scenario, you set environment variables from the server or from a script that runs before Grav to select the environment to be used.

The simplest way to set environment is by using `GRAV_ENVIRONMENT`. Value of `GRAV_ENVIRONMENT` has to be a valid server name with or without domain.

The following example selects **development** environment for the localhost:

[ui-tabs]
[ui-tab title="Apache 2"]
[prism classes="language-apacheconf line-numbers"]
<VirtualHost 127.0.0.1:80>
    ...

    SetEnv GRAV_ENVIRONMENT development
</VirtualHost>
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-fpm"]
[prism classes="language-nginx line-numbers"]
location / {
    ...

   fastcgi_param GRAV_ENVIRONMENT development;
}
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-cgi"]
[prism classes="language-nginx line-numbers"]
location / {
   ...

    env[GRAV_ENVIRONMENT] = development
}
[/prism]
[/ui-tab]
[ui-tab title="Docker"]
[prism classes="language-yaml line-numbers"]
web:
  environment:
    - GRAV_ENVIRONMENT=development
[/prism]
[/ui-tab]
[ui-tab title="PHP"]
[prism classes="language-php line-numbers"]
// Set environment in setup.php or make sure it runs before Grav.
define('GRAV_ENVIRONMENT', 'development');
[/prism]
[/ui-tab]
[/ui-tabs]

### Custom Environment Paths

Starting from Grav 1.7, you can also change the location of the environments. There are two possibilities: either you configure a common location for all the environments or you define them one by one.

#### Custom location for all the environments

If for some reason you are not happy with the default `user/env` location for your environments, it can be changed by using `GRAV_ENVIRONMENTS_PATH` environment variable.

Value of `GRAV_ENVIRONMENTS_PATH` has to be existing path under `GRAV_ROOT`. Do not use trailing slash.

In the next example, all the environments will be located in `user/sites/GRAV_ENVIRONMENT`, where `GRAV_ENVIRONMENT` is either automatically detected or manually set in the server configuration:

[ui-tabs]
[ui-tab title="Apache 2"]
[prism classes="language-apacheconf line-numbers"]
<VirtualHost 127.0.0.1:80>
...

    SetEnv GRAV_ENVIRONMENTS_PATH user://sites
</VirtualHost>
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-fpm"]
[prism classes="language-nginx line-numbers"]
location / {
    ...

fastcgi_param GRAV_ENVIRONMENTS_PATH user://sites;
}
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-cgi"]
[prism classes="language-nginx line-numbers"]
location / {
...

    env[GRAV_ENVIRONMENTS_PATH] = user://sites
}
[/prism]
[/ui-tab]
[ui-tab title="Docker"]
[prism classes="language-yaml line-numbers"]
web:
  environment:
    - GRAV_ENVIRONMENTS_PATH=user://sites
[/prism]
[/ui-tab]
[ui-tab title="PHP"]
[prism classes="language-php line-numbers"]
// Set environments path in setup.php or make sure that the following code runs before Grav.
define('GRAV_ENVIRONMENTS_PATH', 'user://sites');
[/prism]
[/ui-tab]
[/ui-tabs]

#### Custom location for the current environment

Sometimes it may be useful to have a custom location for your environment

Value of `GRAV_ENVIRONMENT_PATH` has to be existing path under `GRAV_ROOT`. Do not use trailing slash.

In the next example, only the current environment will be located in `user/development`:

[ui-tabs]
[ui-tab title="Apache 2"]
[prism classes="language-apacheconf line-numbers"]
<VirtualHost 127.0.0.1:80>
...

    SetEnv GRAV_ENVIRONMENT_PATH user://development
</VirtualHost>
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-fpm"]
[prism classes="language-nginx line-numbers"]
location / {
    ...

fastcgi_param GRAV_ENVIRONMENT_PATH user://development;
}
[/prism]
[/ui-tab]
[ui-tab title="NGINX php-cgi"]
[prism classes="language-nginx line-numbers"]
location / {
...

    env[GRAV_ENVIRONMENT_PATH] = user://development
}
[/prism]
[/ui-tab]
[ui-tab title="Docker"]
[prism classes="language-yaml line-numbers"]
web:
  environment:
    - GRAV_ENVIRONMENT_PATH=user://development
[/prism]
[/ui-tab]
[ui-tab title="PHP"]
[prism classes="language-php line-numbers"]
// Set environment path in setup.php or make sure that the following code runs before Grav.
define('GRAV_ENVIRONMENT_PATH', 'user://development');
[/prism]
[/ui-tab]
[/ui-tabs]

Note that `GRAV_ENVIRONMENT_PATH` is separate from `GRAV_ENVIRONMENT`, so you may also want to set the environment name if you don't want it to be automatically set to match the current domain name.

### Further Customization

Environments can be customized far further than described in this page.

For more information, please continue to the next page: [Multisite Setup](/advanced/multisite-setup).
