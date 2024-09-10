``keys``
========

Фильтр ``keys`` возвращает ключи последовательности или отображения. Он может быть полезным,
когда вы хотите итерировать ключи последовательности или отображения:

.. code-block:: twig

    {% for key in [1, 2, 3, 4]|keys %}
        {{ key }}
    {% endfor %}
    {# выводит: 1 2 3 4 #}

    {% for key in {a: 'a_value', b: 'b_value'}|keys %}
        {{ key }}
    {% endfor %}
    {# выводит: a b #}

.. note::

    Внутренне Twig использует PHP-функцию `array_keys`_.

.. _`array_keys`: https://www.php.net/array_keys
