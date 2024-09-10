``reverse``
===========

Фильтр ``reverse`` реверсирует последовательность, отображение или строку:

.. code-block:: twig

    {% for user in users|reverse %}
        ...
    {% endfor %}

    {{ '1234'|reverse }}

    {# виводить 4321 #}

.. tip::

    Для последовательностей и отображений цифровые клавиши не сохраняются. Чтобы реверсировать и их,
    передайте ``true`` как аргумент фильтра ``reverse``:

    .. code-block:: twig

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# вывод: 0: c    1: b    2: a #}

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse(true) %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# вывод: 3: c    2: b    1: a #}

.. note::

    Он также работает с объектами, реализующими интерфейс `Traversable`_.

Аргументы
---------

* ``preserve_keys``: Сохранение ключей при реверсировании отображения или последовательности.

.. _`Traversable`: https://www.php.net/Traversable
