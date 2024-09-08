``shuffle``
===========

.. versionadded:: 3.11

    Фільтр ``shuffle`` було представлено в Twig 3.11.

Фільтр ``shuffle`` перемішує послідовність, відображення або рядок:

.. code-block:: twig

    {% for user in users|shuffle %}
        ...
    {% endfor %}

.. caution::

    Перемішаний масив не зберігає ключі. Тому, якщо введення мало не послідовні
    ключі, а індексовані (наприклад, за ідентифікатором користувача), то після 
    перемішування це вже не так.

Приклад 1:

.. code-block:: html+twig

    {% set items = [
        'a',
        'b',
        'c',
    ] %}

    <ul>
        {% for item in items|shuffle %}
            <li>{{ item }}</li>
        {% endfor %}
    </ul>

Приклад вище буде відображено так:

.. code-block:: html

    <ul>
        <li>a</li>
        <li>c</li>
        <li>b</li>
    </ul>

Результатом також може бути: "a, b, c" або "b, a, c" або "b, c, a" або "c, a, b" або
"c, b, a".

Приклад 2:

.. code-block:: html+twig

    {% set items = {
        'a': 'd',
        'b': 'e',
        'c': 'f',
    } %}

    <ul>
        {% for index, item in items|shuffle %}
            <li>{{ index }} - {{ item }}</li>
        {% endfor %}
    </ul>

Приклад вище буде відображено так:

.. code-block:: html

    <ul>
        <li>0 - d</li>
        <li>1 - f</li>
        <li>2 - e</li>
    </ul>

Результатом також може бути: "d, e, f" або "e, d, f" або "e, f, d" або "f, d, e" або
"f, e, d".

.. code-block:: html+twig

    {% set string = 'ghi' %}

    <p>{{ string|shuffle }}</p>

Приклад вище буде відображено так:

.. code-block:: html

    <p>gih</p>

Результатом також може бути: "ghi" або "hgi" або "hig" або "igh" або "ihg".
