``singular``
============

.. versionadded:: 3.11

    Фильтр ``singular`` был представлен в Twig 3.11.

Фильтр ``singular`` превращает заданное существительное в форме множественного числа в его
в единственное число:

.. code-block:: twig

    {# Правила английского языка (en) используются по умолчанию #}
    {{ 'partitions'|singular() }}
    partition

    {{ 'partitions'|singular('fr') }}
    partition

.. note::

    Фильтр ``singular`` является частью ``StringExtension``, которое не
    установлено по умолчанию. Сначала установите его:

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

    Внутренне Twig использует метод `singularize`_ из компонента Symfony String.

.. _`inflector`: <https://symfony.com/doc/current/components/string.html#inflector>
.. _`singularize`: <https://symfony.com/doc/current/components/string.html#inflector>
