``html_to_markdown``
====================

Фільтр ``html_to_markdown`` перетворює блок HTML на Markdown:

.. code-block:: html+twig

    {% apply html_to_markdown %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}

Ви також можете використовувати фільтр для всього шаблону, який ви ``включаєте``:

.. code-block:: twig

    {{ include('some_template.html.twig')|html_to_markdown }}

.. note::

    Фільтр ``html_to_markdown`` є частиною ``MarkdownExtension``, яке
    не встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/markdown-extra

    В проектах Symfony ви можете автоматично увімкнути його, встановивши
    ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    Або додайте розширення явно у середовищі Twig::

        use Twig\Extra\Markdown\MarkdownExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new MarkdownExtension());

    Якщо ви не використовуєте Symfony, ви також повинні зареєструвати час виконання розширення::

        use Twig\Extra\Markdown\DefaultMarkdown;
        use Twig\Extra\Markdown\MarkdownRuntime;
        use Twig\RuntimeLoader\RuntimeLoaderInterface;

        $twig->addRuntimeLoader(new class implements RuntimeLoaderInterface {
            public function load($class) {
                if (MarkdownRuntime::class === $class) {
                    return new MarkdownRuntime(new DefaultMarkdown());
                }
            }
        });

``html_to_markdown`` - це лише фронтенд; фактичне перетворення виконується однією з
наведених нижче сумісних бібліотек, з яких ви можете вибрати:

* `league/html-to-markdown`_
* `michelf/php-markdown`_
* `erusev/parsedown`_

Залежно від бібліотеки, ви також можете додати деякі опції, передавши їх як аргумент
до фільтру. Приклад для ``league/html-to-markdown``:

.. code-block:: html+twig

    {% apply html_to_markdown({hard_break: false}) %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}
    
.. _league/html-to-markdown: https://github.com/thephpleague/html-to-markdown
.. _michelf/php-markdown: https://github.com/michelf/php-markdown
.. _erusev/parsedown: https://github.com/erusev/parsedown
