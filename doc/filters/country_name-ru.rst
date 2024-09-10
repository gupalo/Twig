``country_name``
================

Фильтр ``country_name`` возвращает название страны по ее двухбуквенному коду ISO-3166:

.. code-block:: twig

    {# France #}
    {{ 'FR'|country_name }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# États-Unis #}
    {{ 'US'|country_name('fr') }}

.. note::

    Фильтр ``country_name`` является частью ``IntlExtension``, которое не
    установлен по умолчанию. Сначала установите его:

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
