``replace``
===========

Фильтр ``replace`` заменяет заполнители в строке (формат заполнителя
может быть произвольным):

.. code-block:: twig

    {{ "I like %this% and %that%."|replace({'%this%': fruit, '%that%': "oranges"}) }}
    {# если переменная "fruit" установлена как "apples", #}
    {# то выводится "I like apples and oranges" #}

    {# использование % в качестве разделителя абсолютно условное и не есть обязательным #}
    {{ "I like this and --that--."|replace({'this': fruit, '--that--': "oranges"}) }}
    {# выводит "I like apples and oranges" #}

Аргументы
---------

* ``from``: Значение заполнителя как отображение

.. seealso::

    :doc:`format<format>`
