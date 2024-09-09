``apply``
=========

Тег ``apply`` позволяет применить фильтры Twig к блоку данных шаблона:

.. code-block:: twig

    {% apply upper %}
        This text becomes uppercase
    {% endapply %}

Вы также можете объединять фильтры в цепочку и передавать им аргументы:

.. code-block:: html+twig

    {% apply lower|escape('html') %}
        <strong>SOME TEXT</strong>
    {% endapply %}

    {# выводит "&lt;strong&gt;some text&lt;/strong&gt;" #}
