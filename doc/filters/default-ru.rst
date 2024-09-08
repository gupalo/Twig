``default``
===========

Фільтр ``default`` повертає передане значення за замовчуванням, якщо значення
не визначено або порожнє, в іншому випадку - значення змінної:

.. code-block:: twig

    {{ var|default('var is not defined') }}

    {{ var.foo|default('foo item on var is not defined') }}

    {{ var['foo']|default('foo item on var is not defined') }}

    {{ ''|default('passed var is empty')  }}

При використанні фільтру ``default`` у виразі, який використовує змінні у деяких викликах
методів, обов'язково використовуйте фільтр ``default`` у всіх випадках, коли змінна може бути
невизначеною:

.. code-block:: twig

    {{ var.method(foo|default('foo'))|default('foo') }}
    
Використання фільтра ``default`` для булевої змінної може спричинити неочікувану поведінку,
оскільки ``false`` розглядається як порожнє значення. Розгляньте можливість використання 
``??`` замість нього:

.. code-block:: twig

    {% set foo = false %}
    {{ foo|default(true) }} {# true #}
    {{ foo ?? true }} {# false #}

.. note::

    Прочитайте документацію для :doc:`визначених<../tests/defined>` та 
    :doc:`порожніх<../tests/empty>` тестів, щоб дізнатися більше про їх семантику.

Аргументи
---------

* ``default``: Значення за замовчуванням
