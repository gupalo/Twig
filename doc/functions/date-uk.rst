``date``
========

Перетворює аргумент на дату для можливості порівняння дат:

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days') %}
        {# зробити щось #}
    {% endif %}

Аргумент має бути в одному з підтримуваних PHP `форматів дати і часу`_.

Ви можете передати часовий пояс як другий аргумент:

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days', 'Europe/Paris') %}
        {# зробити щось #}
    {% endif %}

Якщо не передано жодного аргументу, функція повертає поточну дату:

.. code-block:: html+twig

    {% if date(user.created_at) < date() %}
        {# завжди! #}
    {% endif %}

.. note::

    Ви можете встановити часовий пояс за замовчуванням глобально, викликавши ``setTimezone()`` в
    екземплярі розширення ``core``::

        $twig = new \Twig\Environment($loader);
        $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

Аргументи
---------

* ``date``:     Дата
* ``timezone``: Часовий пояс

.. _`форматів дати і часу`: https://www.php.net/manual/en/datetime.formats.php
