``batch``
=========

Фильтр ``batch`` "группирует" элементы, возвращая список списков с заданным количеством элементов.
Второй параметр может быть предоставлен и использован для заполнения недостающих элементов:

.. code-block:: html+twig

    {% set items = ['a', 'b', 'c', 'd'] %}

    <table>
        {% for row in items|batch(3, 'No item') %}
            <tr>
                {% for index, column in row %}
                    <td>{{ index }} - {{ column }}</td>
                {% endfor %}
            </tr>
        {% endfor %}
    </table>

Пример выше будет отображен как:

.. code-block:: html+twig

    <table>
        <tr>
            <td>0 - a</td>
            <td>1 - b</td>
            <td>2 - c</td>
        </tr>
        <tr>
            <td>3 - d</td>
            <td>4 - No item</td>
            <td>5 - No item</td>
        </tr>
    </table>

Если вы установите третий параметр ``preserve_keys`` в значение ``false``, ключи будут обнуляться в каждом цикле.

.. code-block:: html+twig

    {% set items = ['a', 'b', 'c', 'd'] %}

    <table>
        {% for row in items|batch(3, 'No item', false) %}
            <tr>
                {% for index, column in row %}
                    <td>{{ index }} - {{ column }}</td>
                {% endfor %}
            </tr>
        {% endfor %}
    </table>

Пример выше будет отображен как:

.. code-block:: html+twig

    <table>
        <tr>
            <td>0 - a</td>
            <td>1 - b</td>
            <td>2 - c</td>
        </tr>
        <tr>
            <td>0 - d</td>
            <td>1 - No item</td>
            <td>2 - No item</td>
        </tr>
    </table>

Аргументы
---------

* ``size``: Размер партии; дробные числа будут округлены в большую сторону
* ``fill``: Используется для заполнения недостающих элементов
* ``preserve_keys``: Сохранять ли ключи (по умолчанию ``true``)
