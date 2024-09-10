``format_currency``
===================

Фильтр ``format_currency`` форматирует число в валюту:

.. code-block:: twig

    {# €1,000,000.00 #}
    {{ '1000000'|format_currency('EUR') }}

Вы можете передать атрибуты, чтобы настроить вывод:

.. code-block:: twig

    {# €12.34 #}
    {{ '12.345'|format_currency('EUR', {rounding_mode: 'floor'}) }}

    {# €1,000,000.0000 #}
    {{ '1000000'|format_currency('EUR', {fraction_digit: 4}) }}

Список поддерживаемых опций:

* ``grouping_used``;
* ``decimal_always_shown``;
* ``max_integer_digit``;
* ``min_integer_digit``;
* ``integer_digit``;
* ``max_fraction_digit``;
* ``min_fraction_digit``;
* ``fraction_digit``;
* ``multiplier``;
* ``grouping_size``;
* ``rounding_mode``;
* ``rounding_increment``;
* ``format_width``;
* ``padding_position``;
* ``secondary_grouping_size``;
* ``significant_digits_used``;
* ``min_significant_digits_used``;
* ``max_significant_digits_used``;
* ``lenient_parse``.

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# 1.000.000,00 € #}
    {{ '1000000'|format_currency('EUR', locale: 'de') }}

.. note::

    Фильтр ``format_currency`` является частью ``IntlExtension``, которое не
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

* ``currency``: Валюта
* ``attrs``: Карта атрибутов
* ``locale``: Локаль
