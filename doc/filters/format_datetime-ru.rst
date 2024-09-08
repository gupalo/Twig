``format_datetime``
===================

Фільтр ``format_datetime`` форматує дату і час:

.. code-block:: twig

    {# Aug 7, 2019, 11:39:12 PM #}
    {{ '2019-08-07 23:39:12'|format_datetime() }}

Формат
------

Ви можете налаштувати виведення частини дати і частини часу:

.. code-block:: twig

    {# 23:39 #}
    {{ '2019-08-07 23:39:12'|format_datetime('none', 'short', locale: 'fr') }}

    {# 07/08/2019 #}
    {{ '2019-08-07 23:39:12'|format_datetime('short', 'none', locale: 'fr') }}

    {# mercredi 7 août 2019 23:39:12 UTC #}
    {{ '2019-08-07 23:39:12'|format_datetime('full', 'full', locale: 'fr') }}

Підтримуваними значеннями є: ``none``, ``short``, ``medium``, ``long`` та ``full``.

.. versionadded:: 3.6

   ``relative_short``, ``relative_medium``, ``relative_long`` і ``relative_full`` також підтримуються при роботі в
    PHP 8.0 і новіше або при використанні полізаповнення, яке визначає константи ``IntlDateFormatter::RELATIVE_*`` і
    пов'язану з ними поведінку.

Для більшої гнучкості ви навіть можете визначити власний патерн
(див. `посібник користувача ICU`_ щодо підтримуваних патернів).

.. code-block:: twig

    {# 11 oclock PM, GMT #}
    {{ '2019-08-07 23:39:12'|format_datetime(pattern: "hh 'oclock' a, zzzz") }}

Локаль
------

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# 7 août 2019 23:39:12 #}
    {{ '2019-08-07 23:39:12'|format_datetime(locale: 'fr') }}

Часовий пояс
------------

За замовчуванням дата відображається шляхом застосування часового поясу за замовчуванням (того,
що вказаний у php.ini або оголошений у Twig - див. нижче), але ви можете перевизначити його,
явно вказавши часовий пояс:

.. code-block:: twig

    {{ datetime|format_datetime(locale: 'en', timezone: 'Pacific/Midway') }}

Якщо дата вже є об'єктом типу DateTime, і ви хочете зберегти її поточний
часовий пояс, передайте ``false`` як значення часового поясу:

.. code-block:: twig

    {{ datetime|format_datetime(locale: 'en', timezone: false) }}

Часовий пояс за замовчуванням також можна встановити глобально за допомогою виклику ``setTimezone()``::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

.. note::

    Фільтр ``format_datetime`` є частиною ``IntlExtension``, яке не
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
* ``timeFormat``: Формат часу
* ``pattern``: Патерн дати та часу
* ``timezone``: Імʼя часового поясу дати
* ``calendar``: Календар ("gregorian" за замовчуванням)

.. _посібник користувача ICU: https://unicode-org.github.io/icu/userguide/format_parse/datetime/#datetime-format-syntax
