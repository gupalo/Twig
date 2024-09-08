``replace``
===========

Фільтр ``replace`` замінює заповнювачі у рядку (формат заповнювача
може бути довільним):

.. code-block:: twig

    {{ "I like %this% and %that%."|replace({'%this%': fruit, '%that%': "oranges"}) }}
    {# якщо змінна "fruit" встановлена як "apples", #}
    {# то виводиться "I like apples and oranges" #}

    {# використання % в якості роздільника суто умовне і не є обовʼязковим #}
    {{ "I like this and --that--."|replace({'this': fruit, '--that--': "oranges"}) }}
    {# виводить "I like apples and oranges" #}

Аргументи
---------

* ``from``: Значення заповнювача як відображення

.. seealso::

    :doc:`format<format>`
