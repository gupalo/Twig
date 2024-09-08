``striptags``
=============

Фільтр ``striptags`` вилучає теги SGML/XML і замінює суміжні символи пробілів
на один пробіл:

.. code-block:: twig

    {{ some_html|striptags }}

Ви також можете вказати теги, які не слід вилучати:

.. code-block:: html+twig

    {{ some_html|striptags('<br><p>') }}

У цьому прикладі теги ``<br/>``, ``<br>``, ``<p>`` та ``</p>`` не буде
вилучено з рядка.

.. note::

    Внутрішньо Twig використовує PHP-функцію `strip_tags`_.

Аргументи
---------

* ``allowable_tags``: Теги, які не слід вилучати

.. _`strip_tags`: https://www.php.net/strip_tags
