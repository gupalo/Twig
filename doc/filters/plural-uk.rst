``plural``
==========

.. versionadded:: 3.11

    Фільтр ``plural`` було представлено в Twig 3.11.

Фільтр ``plural`` перетворює заданий іменник в однині на його
у множину:

.. code-block:: twig

    {# Правила англійської мови (en) використовуються за замовчуванням #}
    {{ 'partition'|pluralize() }}
    partitions

    {{ 'partition'|pluralize('fr') }}
    partitions

.. note::

    Фільтр ``plural`` є частиною ``StringExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/string-extra

    Потім, в проєктах Symfony, установіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

Аргументи
---------

* ``locale``: Локаль початкового рядка (обмежується мовами, які підтримуються `inflector`_ від Symfony, частиною компонента String)
* ``all``: Чи повертати всі можливі множини у вигляді масиву, за замовчуванням ``false``

.. note::

    Внутрішньо Twig використовує метод `pluralize`_ з компонента Symfony String.

.. _`inflector`: <https://symfony.com/doc/current/components/string.html#inflector>
.. _`pluralize`: <https://symfony.com/doc/current/components/string.html#inflector>
