``currency_symbol``
===================

Фільтр ``currency_symbol`` повертає символ валюти за його трибуквеним кодом:

.. code-block:: twig

    {# € #}
    {{ 'EUR'|currency_symbol }}

    {# ¥ #}
    {{ 'JPY'|currency_symbol }}

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# ¥ #}
    {{ 'JPY'|currency_symbol('fr') }}

.. note::

    Фільтр ``currency_symbol`` є частиною ``IntlExtension``, яке не
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
