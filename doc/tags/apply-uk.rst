``apply``
=========

Тег ``apply`` дозволяє застосувати фільтри Twig до блоку даних шаблону:

.. code-block:: twig

    {% apply upper %}
        This text becomes uppercase
    {% endapply %}

Ви також можете об'єднувати фільтри в ланцюжок і передавати їм аргументи:

.. code-block:: html+twig

    {% apply lower|escape('html') %}
        <strong>SOME TEXT</strong>
    {% endapply %}

    {# виводить "&lt;strong&gt;some text&lt;/strong&gt;" #}
