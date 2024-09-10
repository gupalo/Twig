``lower``
=========

Фільтр ``lower`` перетворює значення на нижній регістр:

.. code-block:: twig

    {{ 'WELCOME'|lower }}

    {# виводить 'welcome' #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `mb_strtolower`_.

.. _`mb_strtolower`: https://www.php.net/manual/fr/function.mb-strtolower.php
