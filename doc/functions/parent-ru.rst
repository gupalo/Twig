``parent``
==========

Когда шаблон использует наследование, можно отобразить содержание родительского блока
при переопределении блока с помощью функции ``parent``:

.. code-block:: html+twig

    {% extends "base.html" %}

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

Вызов ``parent()`` вернет содержание блока ``sidebar`` как определено в шаблоне ``base.html``.

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`block<../functions/block>`, :doc:`block<../tags/block>`
