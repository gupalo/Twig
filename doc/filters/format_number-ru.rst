``format_number``
=================

Фильтр ``format_number`` форматирует число:

.. code-block:: twig

    {{ '12.345'|format_number }}

Вы можете передать атрибуты, чтобы настроить вывод:

.. code-block:: twig

    {# 12.34 #}
    {{ '12.345'|format_number({rounding_mode: 'floor'}) }}

    {# 1000000.0000 #}
    {{ '1000000'|format_number({fraction_digit: 4}) }}

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

Помимо простых чисел, фильтр также может форматировать числа в различных стилях:

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_number(style: 'percent') }}

    {# двенадцать точка три четыре пять #}
    {{ '12.345'|format_number(style: 'spellout') }}

    {# 12 сек. #}
    {{ '12'|format_duration_number }}

Список поддерживаемых стилей:

* ``decimal``;
* ``currency``;
* ``percent``;
* ``scientific``;
* ``spellout``;
* ``ordinal``;
* ``duration``.

В качестве сокращения, вы можете использовать фильтры ``format_*_number``, заменив ``*``
на стиль:

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_percent_number }}

    {# двенадцать точка три четыре пять #}
    {{ '12.345'|format_spellout_number }}

Вы можете передать атрибуты, чтобы настроить вывод:

.. code-block:: twig

    {# 12.3% #}
    {{ '0.12345'|format_percent_number({rounding_mode: 'floor', fraction_digit: 1}) }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# 12,345 #}
    {{ '12.345'|format_number(locale: 'fr') }}

.. note::

    Фильтр ``format_number`` является частью ``IntlExtension``, которое не
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
* ``attrs``: Карта атрибутов
* ``style``: Стиль вывода числа
