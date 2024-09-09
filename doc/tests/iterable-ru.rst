``iterable``
============

``iterable`` перевіряє, чи є змінна масивом або об'єктом, який можна траверсувати:

.. code-block:: twig

    {# оцінюється як true, якщо змінна users є ітерабельною #}
    {% if users is iterable %}
        {% for user in users %}
            Hello {{ user }}!
        {% endfor %}
    {% else %}
        {# users, скоріш за все, є рядком #}
        Hello {{ users }}!
    {% endif %}
