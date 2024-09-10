``format``
==========

Фильтр ``format`` форматирует заданную строку, заменяя заполнители
(заполнители следуют нотации `sprintf`_):

.. code-block:: twig

    {{ "I like %s and %s."|format(foo, "bar") }}

    {# выводит I like foo and bar,
       если параметр foo равняется строке foo. #}

.. seealso::

    :doc:`replace<replace>`

.. _`sprintf`: https://www.php.net/sprintf
