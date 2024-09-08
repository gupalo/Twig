``upper``
=========

Фільтр ``upper`` перетворює значення на верхній регістр:

.. code-block:: twig

    {{ 'welcome'|upper }}

    {# outputs 'WELCOME' #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `mb_strtoupper`_.

.. _`mb_strtoupper`: https://www.php.net/mb_strtoupper
