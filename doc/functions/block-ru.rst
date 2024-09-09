``block``
=========

Если в шаблоне используется наследование и вы хотите отобразить блок несколько раз,
используйте функцию ``block``.

.. code-block:: html+twig

    <title>{% block title %}{% endblock %}</title>

    <h1>{{ block('title') }}</h1>

    {% block body %}{% endblock %}

Функция ``block`` также может быть использована для отображения одного блока из другого шаблона:

.. code-block:: twig

    {{ block("title", "common_blocks.twig") }}

Используйте тест ``defined``, чтобы проверить, существует ли блок в контексте текущего шаблона:

.. code-block:: twig

    {% if block("footer") is defined %}
        ...
    {% endif %}

    {% if block("footer", "common_blocks.twig") is defined %}
        ...
    {% endif %}

Аргументы
---------

* ``name``: Имя блока
* ``template``: Шаблон, где искать блок

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`parent<../functions/parent>`
