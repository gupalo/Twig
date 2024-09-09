``defined``
===========

``defined`` проверяет, определена ли переменная в текущем контексте. Это очень
полезно, если вы используете опцию ``strict_variables``:

.. code-block:: twig

    {# defined работает с именами переменных #}
    {% if foo is defined %}
        ...
    {% endif %}

    {# и атрибутами в именах переменных #}
    {% if foo.bar is defined %}
        ...
    {% endif %}

    {% if foo['bar'] is defined %}
        ...
    {% endif %}

При использовании теста ``defined`` для выражения, которое использует переменные в некоторых
вызовах методов, убедитесь, что все они предварительно определены:

.. code-block:: twig

    {% if var is defined and foo.method(var) is defined %}
        ...
    {% endif %}
