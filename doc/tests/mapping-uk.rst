``mapping``
===========

``відображення`` перевіряє, чи є змінна відображенням:

.. code-block:: twig

    {% set users = {alice: "Alice Dupond", bob: "Bob Smith"} %}
    {# оцінюється як true, якщо змінна users є відображенням #}
    {% if users is mapping %}
        {% for key, user in users %}
            {{ key }}: {{ user }};
        {% endfor %}
    {% endif %}
