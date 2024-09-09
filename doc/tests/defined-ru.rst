``defined``
===========

``defined`` перевіряє, чи визначено змінну у поточному контексті. Це дуже
корисно, якщо ви використовуєте опцію ``strict_variables``:

.. code-block:: twig

    {# defined працює з іменами змінних #}
    {% if foo is defined %}
        ...
    {% endif %}

    {# та атрибутами в іменах змінних #}
    {% if foo.bar is defined %}
        ...
    {% endif %}

    {% if foo['bar'] is defined %}
        ...
    {% endif %}

При використанні тесту ``defined`` для виразу, який використовує змінні у деяких викликах методів
методах, переконайтеся, що всі вони попередньо визначені:

.. code-block:: twig

    {% if var is defined and foo.method(var) is defined %}
        ...
    {% endif %}
