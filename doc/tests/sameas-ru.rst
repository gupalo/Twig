``same as``
===========

``same as`` проверяет, является ли переменная такой же, как и другая переменная.
Это эквивалентно ``===`` в PHP:

.. code-block:: twig

    {% if foo.attribute is same as(false) %}
        the foo attribute really is the 'false' PHP value
    {% endif %}
