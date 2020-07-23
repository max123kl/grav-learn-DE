---
title: Frontend Formulare
taxonomy:
    category: docs
---

Das Plugin **Form** ermöglicht Ihnen die Entwicklung praktisch aller Arten von Frontend-Formularen. Im Grunde genommen ist es ein Formularbaukasten, der Ihnen für Ihre eigenen Seiten zur Verfügung steht. Bevor Sie fortfahren, vergessen Sie nicht, das [**Form-Plugin**](https://github.com/getgrav/grav-plugin-form) mit dem Befehl `bin/gpm install form` zu installieren, falls es noch nicht installiert ist.

Um zu verstehen, wie das Plugin **Form** funktioniert, beginnen wir damit, wie man ein einfaches Formular erstellt.

!!!! Seit der Version **Form 2.0** ist es nun obligatorisch, den **Namen des Formulars** als verborgenes Feld zu übergeben.  Wenn Sie die vom Formular-Plugin bereitgestellte `forms.html.twig` verwenden, wird das automatisch übernommen. Wenn Sie jedoch die Standardeinstellung `forms.html.twig` in Ihrem Theme oder Plugin überschrieben haben, sollten Sie manuell `{% include "forms/fields/formname/formname.html.twig" %}` in Ihre Twig-Datei, die das Formular rendert, einfügen.

## Ein einfaches, einzelnes Formular erstellen

Um ein Formular auf eine Seite Ihrer Website einzubinden, erstellen Sie eine Seite und stellen Sie deren Seiten-Datei auf "Form" ein. Sie können dies über das Admin-Panel oder direkt über das Dateisystem tun, indem Sie die Seite `form.md` benennen.

So zum Beispiel `user/pages/03.your-form/form.md`.

Der Inhalt dieser Seite soll wie folgt sein:

[prism classes="language-yaml line-numbers"]
---
title: A Page with an Example Form
form:
    name: contact-form
    fields:
        - name: name
          label: Name
          placeholder: Enter your name
          autofocus: on
          autocomplete: on
          type: text
          validate:
            required: true

        - name: email
          label: Email
          placeholder: Enter your email address
          type: email
          validate:
            required: true

    buttons:
        - type: submit
          value: Submit
        - type: reset
          value: Reset

    process:
        - email:
            from: "{{ config.plugins.email.from }}"
            to:
              - "{{ config.plugins.email.to }}"
              - "{{ form.value.email }}"
            subject: "[Feedback] {{ form.value.name|e }}"
            body: "{% include 'forms/data.html.twig' %}"
        - save:
            fileprefix: feedback-
            dateformat: Ymd-His-u
            extension: txt
            body: "{% include 'forms/data.txt.twig' %}"
        - message: Thank you for your feedback!
        - display: thankyou

---

# My Form

Regular **markdown** content goes here...
[/prism]

!!! This is the content of the `form.md` file, when viewed via file-system. To do this via Admin Plugin, open the page in **Expert Mode**, copy the part between the triple dashes `---`, and paste it in the Frontmatter field.

This is enough to show a form in the page, below the page's content. It is a simple form with a name, email field, two buttons: one to submit the form and one to reset the fields. For more information on the available fields that are provided by the Form plugin, [check out the next section](fields-available).

What happens when you press the `Submit` button?  It executes the `process` actions in series. To find out about other actions, [check out the available options](reference-form-actions) or [create your own](reference-form-actions#add-your-own-custom-processing-to-a-form).

1. An email is sent to the email entered, with the subject `[Feedback] [name entered]`. The body of the email is defined in the `forms/data.html.twig` file of the theme in use.

2. A file is created in `user/data` to store the form input data. The template is defined in `forms/data.txt.twig` of the theme in use.

3. The `thankyou` subpage is shown, along with the passed message. The `thankyou` page must be a subpage of the page containing the form.

!!! Make sure you configured the **Email** plugin to ensure it has the correct configuration in order to send email successfully.

## Multiple Forms

With the release of **Form Plugin v2.0**, you are now able to define multiple forms in a single page.  The syntax is similar but each form is differentiated by the name of the form, in this case `contact-form` and `newsletter-form`:

[prism classes="language-yaml line-numbers"]
forms:
    contact-form:
        fields:
            ...
        buttons:
            ...
        process:
            ...

    newsletter-form:
        fields:
            ...
        buttons:
            ...
        process:
            ...
[/prism]

You can even use this format for single forms, by just providing one form under `forms:`:

[prism classes="language-yaml line-numbers"]
forms:
    contact-form:
        fields:
            ...
        buttons:
            ...
        process:
            ...
[/prism]

## Displaying Forms from Twig

The easiest way to include a form is to simply include a Twig file in the template that renders the page where the form is defined.  For example:

[prism classes="language-twig line-numbers"]
{% include "forms/form.html.twig" %}
[/prism]

This will use the Twig template provided by the Form plugin itself.  In turn, it will render the form as you have defined in the page, and handle displaying a success message, or errors, when the form is submitted.

There is however a more powerful method of displaying forms that can take advantage of the new multi-forms support.  With this method you actually pass a `form:` parameter to the Twig template specifying the form you wish to display:

[prism classes="language-twig line-numbers"]
{% include "forms/form.html.twig" with { form: forms('contact-form') } %}
[/prism]

Using this method, you can choose a specific name of a form to display.  You can even provide the name of a form defined in other pages.  As long as all your form names are unique throughout your site, Grav will find and render the correct form!

You can even display multiple forms in one page:

[prism classes="language-twig line-numbers"]
# Contact Form
{% include "forms/form.html.twig" with { form: forms('contact-form') } %}

# Newsletter Signup
{% include "forms/form.html.twig" with { form: forms('newsletter-form') } %}
[/prism]

An alternative way to display a form is to reference the page route rather than the form name using an array, for example:

[prism classes="language-twig line-numbers"]
# Contact Form
{% include "forms/form.html.twig" with { form: forms( {route:'/forms/contact'} ) } %}
[/prism]

This will find the first form from the page with route `/forms/contact`

## Displaying Forms in Page Content

You can also display a form from within your page content (for example `default.md`) directly without that page even having a form defined within it. Simply pass the name or route to the form.

!!  **Twig processing** should be enabled and **page cache** should be disabled to ensure the form is dynamically processed on the page and not statically cached and form handling can occur.

[prism classes="language-twig line-numbers"]
---
title: Page with Forms
process:
  twig: true
cache_enable: false
---

# Contact Form
{% include "forms/form.html.twig" with {form: forms('contact-form')} %}

# Newsletter Signup
{% include "forms/form.html.twig" with {form: forms( {route: '/newsletter-signup'} ) } %}
[/prism]

## Modular Forms

With previous versions of the Form plugin, to get a form to display in a modular sub-page of your overall **modular** page, you had to define the form in the **top-level modular page**.  This way the form would be processed and available to display in the modular sub-page.

In **Form v2.0**, you can now define the form directly in the modular sub-page just like any other form.  However, if not found, the form plugin will look in the 'current page', i.e. the top-level modular page for a form, so it's fully backwards compatible with the 1.0 way of doing things.

You can also configure your Modular sub-page's Twig template to use a form from another page, like the examples above.

! When using a form defined in a modular sub-page you should set the **action:** to the parent modular page and configure your form with a **redirect:** or **display:** action, as this modular sub-page is not a suitable page to load on form submission because it is **not routable**, and therefore not reachable by a browser.  

Here's an example that exists at `form/modular/_form/form.md`:

[prism classes="language-yaml line-numbers"]
---
title: Modular Form

form:
  action: '/form/modular'
  inline_errors: true
  fields:
    person.name:
      type: text
      label: Name
      validate:
        required: true

  buttons:
    submit:
      type: submit
      value: Submit

  process:
    message: "Thank you from your submission <b>{{ form.value('person.name') }}</b>!"
    reset: true
    display: '/form/modular'  
---

## Modular Form
[/prism]
