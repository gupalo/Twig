``map``
=======

Фильтр ``map`` применяет функцию стрелки к элементам последовательности или
отображения. Функция стрелки получает значение последовательности или отображения:

.. code-block:: twig

    {% set people = [
        {first: "Bob", last: "Smith"},
        {first: "Alice", last: "Dupond"},
    ] %}

    {{ people|map(p => "#{p.first} #{p.last}")|join(', ') }}
    {# выводит Bob Smith, Alice Dupond #}

Функция стрелки также получает ключ в качестве второго аргумента:

.. code-block:: twig

    {% set people = {
        "Bob": "Smith",
        "Alice": "Dupond",
    } %}

    {{ people|map((value, key) => "#{key} #{value}")|join(', ') }}
    {# выводит Bob Smith, Alice Dupond #}

Обратите внимание, что функция стрелки имеет доступ к текущему контексту.

Аргументы
---------

* ``arrow``: Функция стрелки
