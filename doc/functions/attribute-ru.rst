``attribute``
=============

Функция ``attribute`` может быть использована для доступа к "динамическому" атрибуту переменной:

.. code-block:: twig

    {{ attribute(object, method) }}
    {{ attribute(object, method, arguments) }}
    {{ attribute(array, item) }}

Кроме того, тест ``defined`` может проверять наличие динамического атрибута:

.. code-block:: twig

    {{ attribute(object, method) is defined ? 'Method exists' : 'Method does not exist' }}

.. note::

    Алгоритм решения такой же, как и для нотации ``.`` за исключением того, что 
    элементом может быть любое валидное выражение.

Аргументы
---------

* ``variable``: Переменная
* ``attribute``: Імя атрибута
* ``arguments``: Массив аргументов для передачи выражению
