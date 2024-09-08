``trim``
========

Фільтр ``trim`` видаляє пробіли (або інші символи) з початку
та кінця рядка:

.. code-block:: twig

    {{ '  I like Twig.  '|trim }}

    {# виводить 'I like Twig.' #}

    {{ '  I like Twig.'|trim('.') }}

    {# виводить '  I like Twig' #}

    {{ '  I like Twig.  '|trim(side: 'left') }}

    {# виводить 'I like Twig.  ' #}

    {{ '  I like Twig.  '|trim(' ', 'right') }}

    {# виводить '  I like Twig.' #}

.. note::

    Внутрішньо Twig використовує PHP-функції `trim`_, `ltrim`_, та `rtrim`_.

Аргументи
---------

* ``character_mask``: Символи для вилучення

* ``side``: За замовчуванням буде вилучено з лівої та правої сторін (``both``),
  але ``left`` і ``right`` буде вилучено або з лівої, або з правої сторони.

.. _`trim`: https://www.php.net/trim
.. _`ltrim`: https://www.php.net/ltrim
.. _`rtrim`: https://www.php.net/rtrim
