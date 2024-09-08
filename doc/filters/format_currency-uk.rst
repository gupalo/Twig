``format_currency``
===================

Фільтр ``format_currency`` форматує число у валюту:

.. code-block:: twig

    {# €1,000,000.00 #}
    {{ '1000000'|format_currency('EUR') }}

Ви можете передати атрибути, щоб налаштувати виведення:

.. code-block:: twig

    {# €12.34 #}
    {{ '12.345'|format_currency('EUR', {rounding_mode: 'floor'}) }}

    {# €1,000,000.0000 #}
    {{ '1000000'|format_currency('EUR', {fraction_digit: 4}) }}

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

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# 1.000.000,00 € #}
    {{ '1000000'|format_currency('EUR', locale: 'de') }}

.. note::

    Фільтр ``format_currency`` є частиною ``IntlExtension``, яке не
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

* ``currency``: Валюта
* ``attrs``: Мапа атрибутів
* ``locale``: Локаль
