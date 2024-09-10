``inky_to_html``
================

Фильтр ``inky_to_html`` обрабатывает `шаблон письма inky
<https://github.com/zurb/inky>`_:

.. code-block:: html+twig

    {% apply inky_to_html %}
        <row>
            <columns large="6"></columns>
            <columns large="6"></columns>
        </row>
    {% endapply %}

Вы также можете использовать фильтр для включенного файла:

.. code-block:: twig

    {{ include('some_template.inky.twig')|inky_to_html }}

.. note::

    Фильтр ``inky_to_html`` является частью расширения ``InkyExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/inky-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\Inky\InkyExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new InkyExtension());
