``date_modify``
===============

Фільтр ``date_modify`` модифікує дату із заданим рядком модифікатора:

.. code-block:: twig

    {{ post.published_at|date_modify("+1 day")|date("m/d/Y") }}

Фільтр ``date_modify`` приймає рядки (вони мають бути у форматі, що підтримується
функцією `strtotime`_) або екземпляри `DateTime`_. Ви можете комбінувати його з фільтром
його з фільтром :doc:`date<date>` для форматування.

Аргументи
---------

* ``modifier``: Модифікатор

.. _`strtotime`: https://www.php.net/strtotime
.. _`DateTime`:  https://www.php.net/DateTime
