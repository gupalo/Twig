``country_name``
================

Фільтр ``country_name`` повертає назву країни за її  двобуквеним кодом ISO-3166:

.. code-block:: twig

    {# France #}
    {{ 'FR'|country_name }}

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# États-Unis #}
    {{ 'US'|country_name('fr') }}

.. note::

    Фільтр ``country_name`` є частиною ``IntlExtension``, який не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно в середовищі Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументи
---------

* ``locale``: Локаль
