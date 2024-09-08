``deprecated``
==============

Twig генерує повідомлення про застарівання (через виклик PHP-функції ``trigger_error()``), якщо у шаблоні використовується тег ``deprecated``:

.. code-block:: twig

    {# base.twig #}
    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' %}
    {% extends 'layout.twig' %}

Ви також можете оголосити макрос застарілим у такий спосіб:

.. code-block:: twig

    {% macro welcome(name) %}
        {% deprecated 'The "welcome" macro is deprecated, use "hello" instead.' %}

        ...
    {% endmacro %}

Зауважте, що за замовчуванням сповіщення про застарівання вимкнені, вони не відображаються і не записуються до журналу логів. Дивіться :ref:`deprecation-notices`, щоб дізнатися, як з ними поводитися.

.. versionadded:: 3.11

    Опції ``package`` та ``version`` були представлені в Twig 3.11.

За бажанням ви можете додати пакет і версію, у якій було введено застарівання:

.. code-block:: twig

    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' package='twig/twig' %}
    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' package='twig/twig' version='3.11' %}

.. note::

    Не використовуйте тег ``deprecated`` для застарівання ``block``, оскільки це
    не завжди спрацьовує коректно.
