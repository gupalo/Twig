``abs``
=======

Фильтр ``abs`` возвращает абсолютное значение.

.. code-block:: twig

    {# number = -5 #}

    {{ number|abs }}

    {# выводит 5 #}

.. note::

    Внутренне Twig использует PHP-функцию `abs`_.

.. _`abs`: https://www.php.net/abs
