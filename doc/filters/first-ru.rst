``first``
=========

Фильтр ``first`` возвращает первый "элемент" последовательности, отображения или
строки:

.. code-block:: twig

    {{ [1, 2, 3, 4]|first }}
    {# выводит 1 #}

    {{ {a: 1, b: 2, c: 3, d: 4}|first }}
    {# выводит 1 #}

    {{ '1234'|first }}
    {# выводит 1 #}

.. note::

    Он также работает с объектами, реализующими интерфейс `Traversable`_.

.. _`Traversable`: https://www.php.net/manual/en/class.traversable.php
