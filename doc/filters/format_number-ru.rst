``format_number``
=================

Фільтр ``format_number`` форматує число:

.. code-block:: twig

    {{ '12.345'|format_number }}

Ви можете передати атрибути, щоб налаштувати виведення:

.. code-block:: twig

    {# 12.34 #}
    {{ '12.345'|format_number({rounding_mode: 'floor'}) }}

    {# 1000000.0000 #}
    {{ '1000000'|format_number({fraction_digit: 4}) }}

Список підтримуваних опцій:

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

Крім простих чисел, фільтр також може форматувати числа в різних стилях:

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_number(style: 'percent') }}

    {# дванадцять точка три чотири пʼять #}
    {{ '12.345'|format_number(style: 'spellout') }}

    {# 12 сек. #}
    {{ '12'|format_duration_number }}

Список підтримуваних стилів:

* ``decimal``;
* ``currency``;
* ``percent``;
* ``scientific``;
* ``spellout``;
* ``ordinal``;
* ``duration``.

Як скорочення, ви можете використовувати фільтри ``format_*_number``, замінивши ``*``
на стиль:

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_percent_number }}

    {# дванадцять точка три чотири пʼять #}
    {{ '12.345'|format_spellout_number }}

Ви можете передати атрибути, щоб налаштувати виведення:

.. code-block:: twig

    {# 12.3% #}
    {{ '0.12345'|format_percent_number({rounding_mode: 'floor', fraction_digit: 1}) }}

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# 12,345 #}
    {{ '12.345'|format_number(locale: 'fr') }}

.. note::

    Фільтр ``format_number`` є частиною ``IntlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументи
---------

* ``locale``: Локаль
* ``attrs``: Мапа атрибутів
* ``style``: Стиль виведення числа
