``json_encode``
===============

Фільтр ``json_encode`` повертає JSON-представлення значення:

.. code-block:: twig

    {{ data|json_encode() }}

.. note::

    Внутрішньо Twig використовує PHP-функцію `json_encode`_.

Аргументи
---------

* ``options``: Біт-маска для `json_encode options`_: ``{{
  data|json_encode(constant('JSON_PRETTY_PRINT')) }}``.
  Обʼєднайте константи, використовуючи :ref:`побітові оператори<template_logic>`:
  ``{{ data|json_encode(constant('JSON_PRETTY_PRINT') b-or constant('JSON_HEX_QUOT')) }}``

.. _`json_encode`: https://www.php.net/json_encode
.. _`json_encode options`: https://www.php.net/manual/en/json.constants.php
