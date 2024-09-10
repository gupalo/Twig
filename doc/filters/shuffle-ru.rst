``shuffle``
===========

.. versionadded:: 3.11

    Фильтр ``shuffle`` был представлен в Twig 3.11.

Фильтр ``shuffle`` перемешивает последовательность, отображение или строку:

.. code-block:: twig

    {% for user in users|shuffle %}
        ...
    {% endfor %}

.. caution::

    Перемешанный массив не сохраняет ключи. Поэтому, если ввод имел не последовательные
    ключи, а индексированные (например, по идентификатору пользователя), то после 
    перемешивания это уже не так.

Пример 1:

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

Пример выше будет отображен так:

.. code-block:: html

    <ul>
        <li>a</li>
        <li>c</li>
        <li>b</li>
    </ul>

Результатом также может быть: "a, b, c" или "b, a, c" или "b, c, a" или "c, a, b" или
"c, b, a".

Пример 2:

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

Пример выше будет отображен так:

.. code-block:: html

    <ul>
        <li>0 - d</li>
        <li>1 - f</li>
        <li>2 - e</li>
    </ul>

Результатом также может быть: "d, e, f" або "e, d, f" або "e, f, d" або "f, d, e" або
"f, e, d".

.. code-block:: html+twig

    {% set string = 'ghi' %}

    <p>{{ string|shuffle }}</p>

Пример выше будет отображен так:

.. code-block:: html

    <p>gih</p>

Результатом также может быть: "ghi" або "hgi" або "hig" або "igh" або "ihg".
