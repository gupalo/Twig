``cycle``
=========

Функция ``cycle`` выполняет циклический просмотр последовательности:

.. code-block:: twig

    {% set start_year = date() | date('Y') %}
    {% set end_year = start_year + 5 %}

    {% for year in start_year..end_year %}
        {{ cycle(['odd', 'even'], loop.index0) }}
    {% endfor %}
    
    {# выводит

        odd
        even
        odd
        even
        odd
        even
        
    #}

Функция ``cycle`` принимает два аргумента: ``sequence`` для перебора и ``position`` в последовательности.

``sequence`` должна быть непустой и может содержать любое количество значений:

.. code-block:: twig

    {% set fruits = ['apple', 'orange', 'citrus'] %}

    {% for i in 0..10 %}
        {{ cycle(fruits, i) }}
    {% endfor %}
    
    {# выводит
    
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

Аргументы
---------

* ``values``: Последовательность для перебора
* ``position``: Позиция в последовательности
