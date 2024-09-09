``deprecated``
==============

Twig генерирует сообщение об устаревании (через вызов PHP-функции ``trigger_error()``), если в шаблоне используется тег ``deprecated``:

.. code-block:: twig

    {# base.twig #}
    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' %}
    {% extends 'layout.twig' %}

Вы также можете объявить макрос устаревшим следующим образом:

.. code-block:: twig

    {% macro welcome(name) %}
        {% deprecated 'The "welcome" macro is deprecated, use "hello" instead.' %}

        ...
    {% endmacro %}

Заметьте, что по умолчанию уведомления об устаревании отключены, они не отображаются и не записываются в журнал логов. Смотрите :ref:`deprecation-notices`, чтобы узнать, как с ними обращаться.

.. versionadded:: 3.11

    Опции ``package`` и ``version`` были представлены в Twig 3.11.

При желании вы можете добавить пакет и версию, в которой было введено устаревание:

.. code-block:: twig

    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' package='twig/twig' %}
    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' package='twig/twig' version='3.11' %}

.. note::

    Не используйте тег ``deprecated`` для устаревания ``block``, поскольку это
    не всегда срабатывает корректно.
