``date_modify``
===============

Фильтр ``date_modify`` модифицирует дату с заданной строкой модификатора:

.. code-block:: twig

    {{ post.published_at|date_modify("+1 day")|date("m/d/Y") }}

Фильтр ``date_modify`` принимает строки (они должны быть в формате, поддерживаемом
функцией `strtotime`_) или экземпляры `DateTime`_. Вы можете комбинировать его с 
фильтром :doc:`date<date>` для форматирования.

Аргументы
---------

* ``modifier``: Модификатор

.. _`strtotime`: https://www.php.net/strtotime
.. _`DateTime`:  https://www.php.net/DateTime
