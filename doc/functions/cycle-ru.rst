``cycle``
=========

Функція ``cycle`` виконує циклічний перегляд послідовності:

.. code-block:: twig

    {% set start_year = date() | date('Y') %}
    {% set end_year = start_year + 5 %}

    {% for year in start_year..end_year %}
        {{ cycle(['odd', 'even'], loop.index0) }}
    {% endfor %}
    
    {# outputs

        odd
        even
        odd
        even
        odd
        even
        
    #}

Функція ``cycle`` приймає два аргументи: ``sequence`` для перебору та ``position`` у послідовності.

``sequence`` повинна бути непорожньою і може містити будь-яку кількість значень:

.. code-block:: twig

    {% set fruits = ['apple', 'orange', 'citrus'] %}

    {% for i in 0..10 %}
        {{ cycle(fruits, i) }}
    {% endfor %}
    
    {# outputs
    
        apple
        orange
        citrus
        apple
        orange
        citrus
        apple
        orange
        citrus
        apple
        orange
    
    #}

Аргументи
---------

* ``values``: Послідовність для перебору
* ``position``: Позиція в послідовності
