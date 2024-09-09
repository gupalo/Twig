``sequence``
============

``sequence`` перевіряє, чи є змінна послідовністю:

.. code-block:: twig

    {% set users = ["Alice", "Bob"] %}
    {# оцінюється як true, якщо змінна users є послідовністю #}
    {% if users is sequence %}
        {% for user in users %}
            Hello {{ user }}!
        {% endfor %}
    {% endif %}
