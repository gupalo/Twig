``upper``
=========

Фильтр ``upper`` преобразует значение в верхний регистр:

.. code-block:: twig

    {{ 'welcome'|upper }}

    {# outputs 'WELCOME' #}

.. note::

    Внутренне Twig использует PHP-функцию `mb_strtoupper`_.

.. _`mb_strtoupper`: https://www.php.net/mb_strtoupper
