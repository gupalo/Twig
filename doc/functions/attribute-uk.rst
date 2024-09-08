``attribute``
=============

Функція ``attribute`` може бути використана для доступу до "динамічного" атрибуту змінної:

.. code-block:: twig

    {{ attribute(object, method) }}
    {{ attribute(object, method, arguments) }}
    {{ attribute(array, item) }}

Крім того, тест ``defined`` може перевіряти наявність динамічного атрибуту:

.. code-block:: twig

    {{ attribute(object, method) is defined ? 'Method exists' : 'Method does not exist' }}

.. note::

    Алгоритм розвʼязання такий самий, як і для нотації ``.``за винятком того, що 
    елементом може бути будь-який валідний вираз.

Аргументи
---------

* ``variable``: Змінна
* ``attribute``: Імʼя атрибут
* ``arguments``: Масив аргументів для передачі виразу
