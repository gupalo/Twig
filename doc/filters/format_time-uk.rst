``format_time``
===============

Фільтр ``format_time`` форматує час. Він поводиться так само, як і
фільтр :doc:`format_datetime<format_datetime>`, але без дати.

.. note::

    Фільтр ``format_time`` є частиною ``IntlExtension``, яке не
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
* ``timeFormat``: Формат часу
* ``pattern``: Патерн дати та часу
* ``timezone``: Часовий пояс дати
* ``calendar``: Календар ("gregorian" за замовчуванням)
