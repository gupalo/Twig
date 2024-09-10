``format_datetime``
===================

Фильтр ``format_datetime`` форматирует дату и время:

.. code-block:: twig

    {# Aug 7, 2019, 11:39:12 PM #}
    {{ '2019-08-07 23:39:12'|format_datetime() }}

Формат
------

Вы можете настроить вывод части даты и части времени:

.. code-block:: twig

    {# 23:39 #}
    {{ '2019-08-07 23:39:12'|format_datetime('none', 'short', locale: 'fr') }}

    {# 07/08/2019 #}
    {{ '2019-08-07 23:39:12'|format_datetime('short', 'none', locale: 'fr') }}

    {# mercredi 7 août 2019 23:39:12 UTC #}
    {{ '2019-08-07 23:39:12'|format_datetime('full', 'full', locale: 'fr') }}

Поддерживаемыми значениями являются: ``none``, ``short``, ``medium``, ``long`` и ``full``.

.. versionadded:: 3.6

    ``relative_short``, ``relative_medium``, ``relative_long`` и ``relative_full``
    также поддерживаются при работе в PHP 8.0 и новее или при использовании полизаполнения,
    которое определяет константы ``IntlDateFormatter::RELATIVE_*`` и связанное с ними поведение.

Для большей гибкости вы даже можете определить собственный паттерн
(см. `руководство пользователя ICU`_ относительно поддерживаемых паттернов).

.. code-block:: twig

    {# 11 oclock PM, GMT #}
    {{ '2019-08-07 23:39:12'|format_datetime(pattern: "hh 'oclock' a, zzzz") }}

Локаль
------

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# 7 août 2019 23:39:12 #}
    {{ '2019-08-07 23:39:12'|format_datetime(locale: 'fr') }}

Часовой пояс
------------

По умолчанию дата отображается путем применения часового пояса по умолчанию (того,
что указан в php.ini или объявлен в Twig - см. ниже), но вы можете переопределить его,
явно указав часовой пояс:

.. code-block:: twig

    {{ datetime|format_datetime(locale: 'en', timezone: 'Pacific/Midway') }}

Если дата уже является объектом типа DateTime, и вы хотите сохранить ее текущий
часовой пояс, передайте ``false`` как значение часового пояса:

.. code-block:: twig

    {{ datetime|format_datetime(locale: 'en', timezone: false) }}

Часовой пояс по умолчанию также можно установить глобально с помощью вызова ``setTimezone()``::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

.. note::

    Фильтр ``format_datetime`` является частью ``IntlExtension``, которое не
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
* ``timeFormat``: Формат времени
* ``pattern``: Паттерн даты и времени
* ``timezone``: Імя часового пояса даты
* ``calendar``: Календарь ("gregorian" по умолчанию)

.. _посібник користувача ICU: https://unicode-org.github.io/icu/userguide/format_parse/datetime/#datetime-format-syntax
