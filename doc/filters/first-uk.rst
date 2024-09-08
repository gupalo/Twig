``first``
=========

Фільтр ``first`` повертає перший "елемент" послідовності, відображення або
рядка:

.. code-block:: twig

    {{ [1, 2, 3, 4]|first }}
    {# виводить 1 #}

    {{ {a: 1, b: 2, c: 3, d: 4}|first }}
    {# виводить 1 #}

    {{ '1234'|first }}
    {# виводить 1 #}

.. note::

    Він також працює з об'єктами, що реалізують інтерфейс `Traversable`_.

.. _`Traversable`: https://www.php.net/manual/en/class.traversable.php
