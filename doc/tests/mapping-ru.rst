``mapping``
===========

``mapping`` проверяет, является ли переменная отображением:

.. code-block:: twig

    {% set users = {alice: "Alice Dupond", bob: "Bob Smith"} %}
    {# оценивается как true, если переменная users является отображением #}
    {% if users is mapping %}
        {% for key, user in users %}
            {{ key }}: {{ user }};
        {% endfor %}
    {% endif %}
