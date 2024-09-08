``inky_to_html``
================

Фільтр ``inky_to_html`` обробляє `шаблон листа inky
<https://github.com/zurb/inky>`_:

.. code-block:: html+twig

    {% apply inky_to_html %}
        <row>
            <columns large="6"></columns>
            <columns large="6"></columns>
        </row>
    {% endapply %}

Ви також можете використовувати фільтр для включеного файлу:

.. code-block:: twig

    {{ include('some_template.inky.twig')|inky_to_html }}

.. note::

    Фільтр ``inky_to_html`` є частиною розширення ``InkyExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/inky-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Inky\InkyExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new InkyExtension());
