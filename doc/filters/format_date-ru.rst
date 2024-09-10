``format_date``
===============

Фильтр ``format_date`` форматирует дату. Он ведет себя так же, как и
фильтр :doc:`format_datetime<format_datetime>`, но без времени.

.. note::

    Фильтр ``format_date`` является частью ``IntlExtension``, которое не
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
* ``dateFormat``: Формат даты
* ``pattern``: Паттерн даты и времени
* ``timezone``: Чаовой пояс даты
* ``calendar``: Календарь ("gregorian" по умолчанию)
