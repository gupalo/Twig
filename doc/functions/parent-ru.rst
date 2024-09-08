``parent``
==========

Коли шаблон використовує успадкування, можна відобразити зміст батьківського блоку
при перевизначенні блоку за допомогою функції ``parent``:

.. code-block:: html+twig

    {% extends "base.html" %}

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

Виклик ``parent()`` поверне зміст блоку ``sidebar`` як визначено у шаблоні ``base.html``.

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`block<../functions/block>`, :doc:`block<../tags/block>`
