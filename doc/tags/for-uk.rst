``for``
=======

Циклічно переглядайте кожен елемент у послідовності або відображенні. Наприклад, щоб
відобразити список користувачів, наданий у змінній з назвою ``users``:

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for user in users %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>

.. note::

    Послідовність або відображення може бути як масивом, так і об'єктом, що реалізує
    інтерфейс ``Traversable``.

Якщо вам потрібно виконати ітерацію над послідовністю чисел, ви можете використати оператор ``..``:

.. code-block:: twig

    {% for i in 0..10 %}
        * {{ i }}
    {% endfor %}

Наведений вище фрагмент коду виведе всі числа від 0 до 10.

Це також може бути корисним для літер:

.. code-block:: twig

    {% for letter in 'a'..'z' %}
        * {{ letter }}
    {% endfor %}

Оператор ``..`` може приймати будь-який вираз з обох сторін:

.. code-block:: twig

    {% for letter in 'a'|upper..'z'|upper %}
        * {{ letter }}
    {% endfor %}

.. tip::

    Якщо вам потрібен крок, відмінний від 1, ви можете використати функцію ``range``
    замість цього.

Змінна ``loop``
---------------

Усередині блоку циклу ``for`` ви можете отримати доступ до деяких спеціальних змінних:

===================== =============================================================
Змінна                Опис
===================== =============================================================
``loop.index``        Поточна ітерація циклу (індексується 1)
``loop.index0``       Поточна ітерація циклу (індексується 0)
``loop.revindex``     Кількість ітерацій від кінця циклу (індексується 1)
``loop.revindex0``    Кількість ітерацій від кінця циклу (індексується 0)
``loop.first``        True, якщо це перша ітерація
``loop.last``         True, якщо це остання ітерація
``loop.length``       Кількість елементів у послідовності
``loop.parent``       Батьківський контекст
===================== =============================================================

.. code-block:: twig

    {% for user in users %}
        {{ loop.index }} - {{ user.username }}
    {% endfor %}

.. note::

    The ``loop.length``, ``loop.revindex``, ``loop.revindex0``, and
    ``loop.last`` variables are only available for PHP arrays, or objects that
    implement the ``Countable`` interface.

The ``else`` Clause
-------------------

If no iteration took place because the sequence was empty, you can render a
replacement block by using ``else``:

.. code-block:: html+twig

    <ul>
        {% for user in users %}
            <li>{{ user.username|e }}</li>
        {% else %}
            <li><em>no user found</em></li>
        {% endfor %}
    </ul>

Iterating over Keys
-------------------

By default, a loop iterates over the values of the sequence. You can iterate
on keys by using the ``keys`` filter:

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for key in users|keys %}
            <li>{{ key }}</li>
        {% endfor %}
    </ul>

Iterating over Keys and Values
------------------------------

You can also access both keys and values:

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for key, user in users %}
            <li>{{ key }}: {{ user.username|e }}</li>
        {% endfor %}
    </ul>

Iterating over a Subset
-----------------------

You might want to iterate over a subset of values. This can be achieved using
the :doc:`slice <../filters/slice>` filter:

.. code-block:: html+twig

    <h1>Top Ten Members</h1>
    <ul>
        {% for user in users|slice(0, 10) %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>

Iterating over a String
-----------------------

To iterate over the characters of a string, use the
:doc:`split <../filters/split>` filter:

.. code-block:: html+twig

    <h1>Characters</h1>
    <ul>
        {% for char in "諺 / ことわざ"|split('') -%}
            <li>{{ char }}</li>
        {%- endfor %}
    </ul>
