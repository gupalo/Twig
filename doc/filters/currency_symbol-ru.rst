``currency_symbol``
===================

Фильтр ``currency_symbol`` возвращает символ валюты по его трехбуквенному коду:

.. code-block:: twig

    {# € #}
    {{ 'EUR'|currency_symbol }}

    {# ¥ #}
    {{ 'JPY'|currency_symbol }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# ¥ #}
    {{ 'JPY'|currency_symbol('fr') }}

.. note::

    Фильтр ``currency_symbol`` является частью ``IntlExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/intl-extra

   Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументы
---------

* ``locale``: Локаль
