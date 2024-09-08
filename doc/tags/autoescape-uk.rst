``autoescape``
==============

Незалежно від того, увімкнено автоматичне екранування чи ні, ви можете позначити частину шаблону, яку слід екранувати чи ні, використовуючи тег ``autoescape``.

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

Якщо увімкнено автоматичне екранування, за замовчуванням екранується все, окрім значень, явно позначених безпечними. Їх можна позначити у шаблоні за допомогою
фільтра :doc:`raw<../filters/raw>`:

.. code-block:: twig

    {% autoescape %}
        {{ safe_value|raw }}
    {% endautoescape %}

Функції, що повертають дані шаблону (наприклад, :doc:`macros<macro>` і 
:doc:`parent<../functions/parent>`) завжди повертають безпечну розмітку.

.. note::

    Twig достатньо розумний, щоб не екранувати вже екрановане значення за допомогою  фільтру     :doc:`escape<../filters/escape>`.

.. note::

    Twig не екранує статичні вирази:

    .. code-block:: html+twig

        {% set hello = "<strong>Hello</strong>" %}
        {{ hello }}
        {{ "<strong>world</strong>" }}

    Буде відображено "<strong>Hello</strong> **world**".

.. note::

    У главі :doc:`Twig для розробників<../api>` надано додаткову інформацію
    про те, коли і як застосовується автоматичне екранування.
