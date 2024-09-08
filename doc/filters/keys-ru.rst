``keys``
========

Фільтр ``keys`` повертає ключі послідовності або відображення. Він може бути корисним
коли ви хочете ітерувати ключі послідовності або відображення:

.. code-block:: twig

    {% for key in [1, 2, 3, 4]|keys %}
        {{ key }}
    {% endfor %}
    {# виводить: 1 2 3 4 #}

    {% for key in {a: 'a_value', b: 'b_value'}|keys %}
        {{ key }}
    {% endfor %}
    {# виводить: a b #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `array_keys`_.

.. _`array_keys`: https://www.php.net/array_keys
