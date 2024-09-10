``striptags``
=============

Фильтр ``striptags`` извлекает теги SGML/XML и заменяет смежные символы пробелов
на один пробел:

.. code-block:: twig

    {{ some_html|striptags }}

Вы также можете указать теги, которые не следует удалять:

.. code-block:: html+twig

    {{ some_html|striptags('<br><p>') }}

В этом примере теги ``<br/>``, ``<br>``, ``<p>`` и ``</p>`` не будут
удалены из строки.

.. note::

    Внутренне Twig использует PHP-функцию `strip_tags`_.

Аргументы
---------

* ``allowable_tags``: Теги, которые не нужно удалять

.. _`strip_tags`: https://www.php.net/strip_tags
