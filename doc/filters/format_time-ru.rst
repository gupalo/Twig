``format_time``
===============

Фильтр ``format_time`` форматирует время. Он ведет себя так же, как и
фильтр :doc:`format_datetime<format_datetime>`, но без даты.

.. note::

    Фильтр ``format_time`` является частью ``IntlExtension``, которое не
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

Аргументі
---------

* ``locale``: Локаль
* ``timeFormat``: Формат времени
* ``pattern``: Паттерн даты и времени
* ``timezone``: Часовой пояс даты
* ``calendar``: Календарь ("gregorian" по умолчанию)
