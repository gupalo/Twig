``constant``
============

``constant`` повертає константне значення для заданого рядка:

.. code-block:: twig

    {{ some_date|date(constant('DATE_W3C')) }}
    {{ constant('Namespace\\Classname::CONSTANT_NAME') }}

Ви також можете читати константи з екземплярів об'єктів:

.. code-block:: twig

    {{ constant('RSS', date) }}

Отримати повне ім'я класу об'єкта:

.. code-block:: twig

    {{ constant('class', date) }}

Використовуйте тест ``defined`` для перевірки того, чи є константа визначеною:

.. code-block:: twig

    {% if constant('SOME_CONST') is defined %}
        ...
    {% endif %}
