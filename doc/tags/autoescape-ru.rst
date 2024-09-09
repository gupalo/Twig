``autoescape``
==============

Независимо от того, включено автоматическое экранирование или нет, вы можете обозначить часть шаблона, которую следует экранировать или нет, используя тег ``autoescape``.

.. code-block:: twig

    {% autoescape %}
        Everything will be automatically escaped in this block
        using the HTML strategy
    {% endautoescape %}

    {% autoescape 'html' %}
        Everything will be automatically escaped in this block
        using the HTML strategy
    {% endautoescape %}

    {% autoescape 'js' %}
        Everything will be automatically escaped in this block
        using the js escaping strategy
    {% endautoescape %}

    {% autoescape false %}
        Everything will be outputted as is in this block
    {% endautoescape %}

Если включено автоматическое экранирование, по умолчанию экранируется все, кроме значений, явно помеченных безопасными. Их можно обозначить в шаблоне с помощью
фильтра :doc:`raw<../filters/raw>`:

.. code-block:: twig

    {% autoescape %}
        {{ safe_value|raw }}
    {% endautoescape %}

Функции, возвращающие данные шаблона (например, :doc:`macros<macro>` и 
:doc:`parent<../functions/parent>`) всегда возвращают безопасную разметку.

.. note::

    Twig достаточно умен, чтобы не экранировать уже экранированное значение с помощью фильтра                     :doc:`escape<../filters/escape>`.

.. note::

    Twig не экранирует статические выражения:

    .. code-block:: html+twig

        {% set hello = "<strong>Hello</strong>" %}
        {{ hello }}
        {{ "<strong>world</strong>" }}

    Будет отображено "<strong>Hello</strong> **world**".

.. note::

    В главе :doc:`Twig для разработчиков<./api>` предоставлена дополнительная информация
    о том, когда и как применяется автоматическое экранирование.
