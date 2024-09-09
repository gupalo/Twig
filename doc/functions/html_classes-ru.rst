``html_classes``
================

Функция ``html_classes`` возвращает строку, условно объединяя имена классов вместе:

.. code-block:: html+twig

    <p class="{{ html_classes('a-class', 'another-class', {
        'errored': object.errored,
        'finished': object.finished,
        'pending': object.pending,
    }) }}">How are you doing?</p>

.. note::

    Функция ``html_classes`` является частью ``HtmlExtension``, который не установлен по 
    по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/html-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());
