``currency_name``
=================

Фильтр ``currency_name`` возвращает название валюты по ее трехбуквенному коду:

.. code-block:: twig

    {# Euro #}
    {{ 'EUR'|currency_name }}

    {# Japanese Yen #}
    {{ 'JPY'|currency_name }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# yen japonais #}
    {{ 'JPY'|currency_name('fr_FR') }}

.. note::

    Фильтр ``currency_name`` является частью ``IntlExtension``, которое не установлено по
    по умолчанию. Сначала установите его:

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
