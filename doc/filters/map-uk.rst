``map``
=======

Фільтр ``map`` застосовує функцію стрілки до елементів послідовності або
відображення. Функція стрілки отримує значення послідовності або відображення:

.. code-block:: twig

    {% set people = [
        {first: "Bob", last: "Smith"},
        {first: "Alice", last: "Dupond"},
    ] %}

    {{ people|map(p => "#{p.first} #{p.last}")|join(', ') }}
    {# виводить Bob Smith, Alice Dupond #}

Функція стрілки також отримує ключ в якості другого аргументу:

.. code-block:: twig

    {% set people = {
        "Bob": "Smith",
        "Alice": "Dupond",
    } %}

    {{ people|map((value, key) => "#{key} #{value}")|join(', ') }}
    {# виводить Bob Smith, Alice Dupond #}

Зверніть увагу, що функція стрілки має доступ до поточного контексту.

Аргументи
---------

* ``arrow``: Функція стрілки
