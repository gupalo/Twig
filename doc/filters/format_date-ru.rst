``format_date``
===============

Фільтр ``format_date`` форматує дату. Він поводиться так само, як і
фільтр :doc:`format_datetime<format_datetime>`, але без часу.

.. note::

    Фільтр ``format_date`` є частиною ``IntlExtension``, яке не
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
* ``dateFormat``: Формат дати
* ``pattern``: Патерн дати та часу
* ``timezone``: Чаовий пояс дати
* ``calendar``: Календар ("gregorian" за замовчуванням)
