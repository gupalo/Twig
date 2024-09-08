``abs``
=======

Фільтр ``abs`` повертає абсолютне значення.

.. code-block:: twig

    {# number = -5 #}

    {{ number|abs }}

    {# outputs 5 #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `abs`_.

.. _`abs`: https://www.php.net/abs
