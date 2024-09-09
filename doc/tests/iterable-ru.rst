``iterable``
============

``iterable`` проверяет, является ли переменная массивом или объектом, который 
можно траверсировать:

.. code-block:: twig

    {# оценивается как true, если переменная users является итерабельной #}
    {% if users is iterable %}
        {% for user in users %}
            Hello {{ user }}!
        {% endfor %}
    {% else %}
        {# users, скорее всего, является строкой #}
        Hello {{ users }}!
    {% endif %}
