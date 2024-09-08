``inline_css``
==============

Фільтр ``inline_css`` фільтрує вбудовані CSS-стилі в HTML-документах:

.. code-block:: html+twig

    {% apply inline_css %}
        <html>
            <head>
                <style>
                    p { color: red; }
                </style>
            </head>
            <body>
                <p>Hello CSS!</p>
            </body>
        </html>
    {% endapply %}

Ви також можете додати деякі таблиці стилів, передавши їх як аргументи до фільтра:

.. code-block:: html+twig

    {% apply inline_css(source("some_styles.css"), source("another.css")) %}
        <html>
            <body>
                <p>Hello CSS!</p>
            </body>
        </html>
    {% endapply %}

Стилі, завантажені через фільтр, перевизначають стилі, визначені в тегу ``<style>``
HTML-документа.

Ви також можете використовувати фільтр у включеному файлі:

.. code-block:: twig

    {{ include('some_template.html.twig')|inline_css }}

    {{ include('some_template.html.twig')|inline_css(source("some_styles.css")) }}

Зверніть увагу, що CSS-інлайнер працює з цілим HTML-документом, а не з його фрагментом.

.. note::

    Фільтр ``inline_css`` є частиною розширення ``CssInlinerExtension``, яке не
    встановлений за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/cssinliner-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\CssInliner\CssInlinerExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CssInlinerExtension());
