``block``
=========

Якщо у шаблоні використовується успадкування і ви хочете відобразити блок декілька разів,
використовуйте функцію ``block``.

.. code-block:: html+twig

    <title>{% block title %}{% endblock %}</title>

    <h1>{{ block('title') }}</h1>

    {% block body %}{% endblock %}

Функція ``block`` також може бути використана для відображення одного блоку з іншого шаблону:

.. code-block:: twig

    {{ block("title", "common_blocks.twig") }}

Використовуйте тест ``defined``, щоб перевірити, чи існує блок в контексті поточного шаблону:

.. code-block:: twig

    {% if block("footer") is defined %}
        ...
    {% endif %}

    {% if block("footer", "common_blocks.twig") is defined %}
        ...
    {% endif %}

Аргументи
---------

* ``name``: Імʼя блоку
* ``template``: Шаблон, де шукати блок

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`parent<../functions/parent>`
