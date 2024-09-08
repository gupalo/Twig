``sort``
========

Фільтр ``sort`` сортує послідовності та відображення:

.. code-block:: twig

    {% for user in users|sort %}
        ...
    {% endfor %}

.. note::

    Внутрішньо Twig використовує PHP-функцію `asort`_ для обслуговування асоціації 
    індексів. Вона підтримує об'єкти Traversable, перетворюючи їх на масиви.

Ви можете передати функцію стрілки, щоб сконфігурувати сортування:

.. code-block:: html+twig

    {% set fruits = [
        {name: 'Apples', quantity: 5},
        {name: 'Oranges', quantity: 2},
        {name: 'Grapes', quantity: 4},
    ] %}

    {% for fruit in fruits|sort((a, b) => a.quantity <=> b.quantity)|column('name') %}
        {{ fruit }}
    {% endfor %}

    {# виведення в такому порядку: Oranges, Grapes, Apples #}

Зверніть увагу на використання оператора `spaceship`_ для спрощення порівняння.

Аргументи
---------

* ``arrow``: Функція стрілки

.. _`asort`: https://www.php.net/asort
.. _`spaceship`: https://www.php.net/manual/en/language.operators.comparison.php
