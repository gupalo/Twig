``column``
==========

Фильтр ``column`` возвращает значение из одного столбца массива ввода.

.. code-block:: twig

    {% set items = [{ 'fruit' : 'apple'}, {'fruit' : 'orange' }] %}

    {% set fruits = items|column('fruit') %}

    {# fruits теперь содержит ['apple', 'orange'] #}

.. note::

    Внутренне Twig использует PHP-функцию `array_column`_.

Аргументы
---------

* ``name``: Імя столбца для удаления

.. _`array_column`: https://www.php.net/array_column
