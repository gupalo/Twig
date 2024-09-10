``capitalize``
==============

Фильтр ``capitalize`` переводит значения в заглавные буквы. Первый символ будет в
в верхнем регистре, все остальные - в нижнем:

.. code-block:: twig

    {{ 'my first car'|capitalize }}

    {# выводит 'My first car' #}
