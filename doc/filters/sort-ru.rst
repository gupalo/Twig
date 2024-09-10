``sort``
========

Фильтр ``sort`` сортирует последовательности и отображения:

.. code-block:: twig

    {% for user in users|sort %}
        ...
    {% endfor %}

.. note::

    Внутренне Twig использует PHP-функцию `asort`_ для обслуживания ассоциации 
    индексов. Она поддерживает объекты Traversable, преобразуя их в массивы.

Вы можете передать функцию стрелки, чтобы настроить сортировку:

.. code-block:: html+twig

    {% set fruits = [
        {name: 'Apples', quantity: 5},
        {name: 'Oranges', quantity: 2},
        {name: 'Grapes', quantity: 4},
    ] %}

    {% for fruit in fruits|sort((a, b) => a.quantity <=> b.quantity)|column('name') %}
        {{ fruit }}
    {% endfor %}

    {# вывод в таком порядке: Oranges, Grapes, Apples #}

Обратите внимание на использование оператора `spaceship`_ для упрощения сравнения.

Аргументы
---------

* ``arrow``: Функция стрелки

.. _`asort`: https://www.php.net/asort
.. _`spaceship`: https://www.php.net/manual/en/language.operators.comparison.php
