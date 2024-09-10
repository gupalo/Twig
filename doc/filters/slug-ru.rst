``slug``
========

Фильтр ``slug`` преобразует заданную строку в другую строку, которая
содержит только безопасные ASCII-символы. 

Ниже приведен пример:

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug }}
    Workspace-settings

По умолчанию разделителем между словами является тире (``-``), но вы можете 
определить селектор по своему усмотрению, передав его в качестве аргумента:

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug('/') }}
    Workspace/settings

Слаггер автоматически определяет язык оригинальной строки, но вы также можете указать его явно
с помощью второго аргумента:

.. code-block:: twig

    {{ '...'|slug('-', 'ko') }}

Фильтр ``slug`` использует одноименный метод в модуле Symfony 
`AsciiSlugger <https://symfony.com/doc/current/components/string.html#slugger>`_.

.. note::

   Фильтр ``slug`` является частью ``StringExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/string-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

Аргументы
---------

* ``separator``: Разделитель, который используется для объединения слов (по умолчанию ``-``)
* ``locale``: Локаль изначальной строки (если она не указана, она будет определена автоматически)
