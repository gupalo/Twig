``sequence``
============

``sequence`` проверяет, является ли переменная последовательностью:

.. code-block:: twig

    {% set users = ["Alice", "Bob"] %}
    {# оценивается как true, если переменная users является последовательностью #}
    {% if users is sequence %}
        {% for user in users %}
            Hello {{ user }}!
        {% endfor %}
    {% endif %}
