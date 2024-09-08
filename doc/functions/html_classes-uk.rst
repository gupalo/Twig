``html_classes``
================

Функція ``html_classes`` повертає рядок, умовно об'єднуючи імена класів разом:

.. code-block:: html+twig

    <p class="{{ html_classes('a-class', 'another-class', {
        'errored': object.errored,
        'finished': object.finished,
        'pending': object.pending,
    }) }}">How are you doing?</p>

.. note::

    Функція ``html_classes`` є частиною ``HtmlExtension``, яке не встановлена за 
    замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/html-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());
