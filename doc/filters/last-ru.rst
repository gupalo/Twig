``last``
========

Фільтр ``last`` повертає останній "елемент" послідовності, відображення або
рядка:

.. code-block:: twig

    {{ [1, 2, 3, 4]|last }}
    {# виводить 4 #}

    {{ {a: 1, b: 2, c: 3, d: 4}|last }}
    {# виводить 4 #}

    {{ '1234'|last }}
    {# виводить 4 #}

.. note::

    Він також працює з об'єктами, що реалізують інтерфейс `Traversable`_.

.. _`Traversable`: https://www.php.net/manual/en/class.traversable.php
