``lower``
=========

Фильтр ``lower`` преобразует значение в нижний регистр:

.. code-block:: twig

    {{ 'WELCOME'|lower }}

    {# выводит 'welcome' #}

.. note::

    Внутренне Twig использует PHP-функцию `mb_strtolower`_.

.. _`mb_strtolower`: https://www.php.net/manual/fr/function.mb-strtolower.php
