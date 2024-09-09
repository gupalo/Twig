``constant``
============

``constant`` возвращает константное значение для заданной строки:

.. code-block:: twig

    {{ some_date|date(constant('DATE_W3C')) }}
    {{ constant('Namespace\\Classname::CONSTANT_NAME') }}

Вы также можете читать константы из экземпляров объектов:

.. code-block:: twig

    {{ constant('RSS', date) }}

Получить полное имя класса объекта:

.. code-block:: twig

    {{ constant('class', date) }}

Используйте тест ``defined`` для проверки того, является ли константа определенной:

.. code-block:: twig

    {% if constant('SOME_CONST') is defined %}
        ...
    {% endif %}
