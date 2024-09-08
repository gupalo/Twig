``column``
==========

Фільтр ``column`` повертає значення з одного стовпця масиву введення.

.. code-block:: twig

    {% set items = [{ 'fruit' : 'apple'}, {'fruit' : 'orange' }] %}

    {% set fruits = items|column('fruit') %}

    {# fruits тепер містить ['apple', 'orange'] #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `array_column`_.

Аргументи
---------

* ``name``: Імʼя стовпця для вилучення

.. _`array_column`: https://www.php.net/array_column
