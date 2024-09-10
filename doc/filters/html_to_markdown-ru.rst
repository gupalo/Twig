``html_to_markdown``
====================

Фильтр ``html_to_markdown`` превращает блок HTML в Markdown:

.. code-block:: html+twig

    {% apply html_to_markdown %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}

Вы также можете использовать фильтр для всего шаблона, который вы ``включаете``:

.. code-block:: twig

    {{ include('some_template.html.twig')|html_to_markdown }}

.. note::

    Фильтр ``html_to_markdown`` является частью ``MarkdownExtension``, которое
    не установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/markdown-extra

    В проектах Symfony вы можете автоматически включить его, установив
    ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    Или добавьте расширение явно в окружение Twig::

        use Twig\Extra\Markdown\MarkdownExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new MarkdownExtension());

    Если вы не используете Symfony, вы также должны зарегистрировать время выполнения расширения::

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

``html_to_markdown`` - это только фронтенд; фактическое преобразование выполняется одной из
приведенных ниже совместимых библиотек, из которых вы можете выбирать:

* `league/html-to-markdown`
* `michelf/php-markdown`_
* `erusev/parsedown`_

В зависимости от библиотеки, вы также можете добавить некоторые опции, передав их в качестве аргумента
к фильтру. Пример для ``league/html-to-markdown``:

.. code-block:: html+twig

    {% apply html_to_markdown({hard_break: false}) %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}
    
.. _league/html-to-markdown: https://github.com/thephpleague/html-to-markdown
.. _michelf/php-markdown: https://github.com/michelf/php-markdown
.. _erusev/parsedown: https://github.com/erusev/parsedown
