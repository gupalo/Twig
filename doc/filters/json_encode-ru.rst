``json_encode``
===============

Фильтр ``json_encode`` возвращает JSON-представление значения:

.. code-block:: twig

    {{ data|json_encode() }}

.. note::

    Внутренне Twig использует PHP-функцию `json_encode`_.

Аргументы
---------

* ``options``: Бит-маска для `json_encode options`_: ``{{
  data|json_encode(constant('JSON_PRETTY_PRINT')) }}``.
  Объедините константы, используя :ref:`побитовые операторы<template_logic>`:
  ``{{ data|json_encode(constant('JSON_PRETTY_PRINT') b-or constant('JSON_HEX_QUOT')) }}``

.. _`json_encode`: https://www.php.net/json_encode
.. _`json_encode options`: https://www.php.net/manual/en/json.constants.php
