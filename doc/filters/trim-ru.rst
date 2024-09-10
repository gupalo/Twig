``trim``
========

Фильтр ``trim`` удаляет пробелы (или другие символы) из начала
и конца строки:

.. code-block:: twig

    {{ '  I like Twig.  '|trim }}

    {# выводит 'I like Twig.' #}

    {{ '  I like Twig.'|trim('.') }}

    {# выводит '  I like Twig' #}

    {{ '  I like Twig.  '|trim(side: 'left') }}

    {# выводит 'I like Twig.  ' #}

    {{ '  I like Twig.  '|trim(' ', 'right') }}

    {# выводит '  I like Twig.' #}

.. note::

    Внутренне Twig использует PHP-функции `trim`_, `ltrim`_, и `rtrim`_.

Аргументы
---------

* ``character_mask``: Символы для удаления

* ``side``: По умолчанию будет удалено с левой и правой сторон (``both``),
  но ``left`` и ``right`` будут удалены либо с левой, либо с правой стороны.

.. _`trim`: https://www.php.net/trim
.. _`ltrim`: https://www.php.net/ltrim
.. _`rtrim`: https://www.php.net/rtrim
