``inline_css``
==============

Фильтр ``inline_css`` фильтрует встроенные CSS-стили в HTML-документах:

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

Вы также можете добавить некоторые таблицы стилей, передав их в качестве аргументов в фильтр:

.. code-block:: html+twig

    {% apply inline_css(source("some_styles.css"), source("another.css")) %}
        <html>
            <body>
                <p>Hello CSS!</p>
            </body>
        </html>
    {% endapply %}

Стили, загруженные через фильтр, переопределяют стили, определенные в теге ``<style>``.
HTML-документа.

Вы также можете использовать фильтр во включенном файле:

.. code-block:: twig

    {{ include('some_template.html.twig')|inline_css }}

    {{ include('some_template.html.twig')|inline_css(source("some_styles.css")) }}

Обратите внимание, что CSS-инлайнер работает с целым HTML-документом, а не с его фрагментом.

.. note::

    Фильтр ``inline_css`` является частью расширения ``CssInlinerExtension``, которое не
    установлен по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/cssinliner-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\CssInliner\CssInlinerExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CssInlinerExtension());
