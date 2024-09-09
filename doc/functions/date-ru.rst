``date``
========

Преобразует аргумент в дату для возможности сравнения дат:

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days') %}
        {# сделать что-то #}
    {% endif %}

Аргумент должен быть в одном из поддерживаемых PHP `форматов даты и времени`_.

Вы можете передать часовой пояс как второй аргумент:

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days', 'Europe/Paris') %}
        {# сделать что-то #}
    {% endif %}

Если не передано ни одного аргумента, функция возвращает текущую дату:

.. code-block:: html+twig

    {% if date(user.created_at) < date() %}
        {# всегда! #}
    {% endif %}

.. note::

    Вы можете установить часовой пояс по умолчанию глобально, вызвав ``setTimezone()`` в
    экземпляре расширения ``core``::

        $twig = new \Twig\Environment($loader);
        $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

Аргументы
---------

* ``date``:     Дата
* ``timezone``: Часовой пояс

.. _`форматов даты и времени`: https://www.php.net/manual/en/datetime.formats.php
