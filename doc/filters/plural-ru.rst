``plural``
==========

.. versionadded:: 3.11

    Фильтр ``plural`` был представлен в Twig 3.11.

Фильтр ``plural`` преобразует заданное существительное в единственном числе в его
множественное число:

.. code-block:: twig

    {# Правила английского языка (en) используются по умолчанию #}
    {{ 'partition'|pluralize() }}
    partitions

    {{ 'partition'|pluralize('fr') }}
    partitions

.. note::

    Фильтр ``plural`` является частью ``StringExtension``, которое не установлено
    по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/string-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

Аргументы
---------

* ``locale``: Локаль начальной строки (ограничивается языками, которые поддерживаются `inflector`_ от Symfony, частью компонента String)
* ``all``: Возвращать ли все возможные множества в виде массива, по умолчанию ``false``

.. note::

    Внутренне Twig использует метод `pluralize`_ из компонента Symfony String.

.. _`inflector`: <https://symfony.com/doc/current/components/string.html#inflector>
.. _`pluralize`: <https://symfony.com/doc/current/components/string.html#inflector>
