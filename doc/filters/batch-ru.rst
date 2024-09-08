``batch``
=========

Фільтр ``batch`` "групує" елементи, повертаючи список списків із заданою кількістю елементів.
Другий параметр може бути наданий і використаний для заповнення відсутніх елементів:

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

Приклад вище буде відображено як:

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

Якщо ви встановите третій параметр ``preserve_keys`` у значення ``false``, ключі обнулятимуться у кожному циклі.

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

Приклад вище буде відображено як:

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

Аргументи
---------

* ``size``: Розмір партії; дробові числа будуть округлені в більшу сторону
* ``fill``: Використовується для заповнення відсутніх елементів
* ``preserve_keys``: Зберігати ключі чи ні (за замовчуванням ``true``)
