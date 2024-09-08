``merge``
=========

Фільтр ``merge`` об'єднує послідовності та відображення:

Для послідовностей нові значення додаються в кінці існуючих:

.. code-block:: twig

    {% set values = [1, 2] %}

    {% set values = values|merge(['apple', 'orange']) %}

    {# значення тепер містять [1, 2, 'apple', 'orange'] #}

Для відображень процес злиття відбувається по ключах; якщо ключ ще не існує, він 
додається, але якщо ключ вже існує, його значення перевизначається:

.. code-block:: twig

    {% set items = {'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'unknown'} %}

    {% set items = items|merge({ 'peugeot': 'car', 'renault': 'car' }) %}

    {# елементи тепер містять {'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'car', 'renault': 'car'} #}

.. tip::

    Якщо ви хочете гарантувати, що деякі значення будуть визначені у відображенні 
    (за заданими значеннями за замовчуванням), поміняйте місцями два елементи у виклику:

    .. code-block:: twig

        {% set items = {'apple': 'fruit', 'orange': 'fruit'} %}

        {% set items = {'apple': 'unknown'}|merge(items) %}

        {# елементи тепер містять {'apple': 'fruit', 'orange': 'fruit'} #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `array_merge`_. Вона підтримує
    об'єкти Traversable, перетворюючи їх на масиви.

.. _`array_merge`: https://www.php.net/array_merge
