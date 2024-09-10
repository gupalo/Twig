``merge``
=========

Фильтр ``merge`` объединяет последовательности и отображения:

Для последовательностей новые значения добавляются в конце существующих:

.. code-block:: twig

    {% set values = [1, 2] %}

    {% set values = values|merge(['apple', 'orange']) %}

    {# значения теперь содержат [1, 2, 'apple', 'orange'] #}

Для отображений процесс слияния происходит по ключам; если ключ еще не существует, он 
добавляется, но если ключ уже существует, его значение переопределяется:

.. code-block:: twig

    {% set items = {'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'unknown'} %}

    {% set items = items|merge({ 'peugeot': 'car', 'renault': 'car' }) %}

    {# элементы теперь содержат {'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'car', 'renault': 'car'} #}

.. tip::

    Если вы хотите гарантировать, что некоторые значения будут определены в отображении 
    (по заданным значениям по умолчанию), поменяйте местами два элемента в вызове:

    .. code-block:: twig

        {% set items = {'apple': 'fruit', 'orange': 'fruit'} %}

        {% set items = {'apple': 'unknown'}|merge(items) %}

        {# элементы теперь содержат {'apple': 'fruit', 'orange': 'fruit'} #}

.. note::

    Внутренне Twig использует PHP-функцию `array_merge`_. Она поддерживает
    объекты Traversable, превращая их в массивы.

.. _`array_merge`: https://www.php.net/array_merge
