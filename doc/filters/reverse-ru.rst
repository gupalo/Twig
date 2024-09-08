``reverse``
===========

Фільтр ``reverse`` реверсує послідовність, відображення або рядок:

.. code-block:: twig

    {% for user in users|reverse %}
        ...
    {% endfor %}

    {{ '1234'|reverse }}

    {# виводить 4321 #}

.. tip::

    Для послідовностей і відображень цифрові клавіші не зберігаються. Щоб реверсувати і їх,
    передайте ``true`` як аргумент фільтру ``reverse``:

    .. code-block:: twig

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# виведення: 0: c    1: b    2: a #}

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse(true) %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# виведення: 3: c    2: b    1: a #}

.. note::

    Він також працює з об'єктами, що реалізують інтерфейс `Traversable`_.

Аргументи
---------

* ``preserve_keys``: Збереження ключів при реверсуванні відображення або послідовності.

.. _`Traversable`: https://www.php.net/Traversable
