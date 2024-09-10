``markdown_to_html``
====================

Фильтр ``markdown_to_html`` преобразует блок Markdown в HTML:

.. code-block:: twig

    {% apply markdown_to_html %}
    Title
    =====

    Hello!
    {% endapply %}

Обратите внимание, что вы можете делать отступы в содержании Markdown, поскольку начальные пробелы будут последовательно удаляться перед преобразованием:

.. code-block:: twig

    {% apply markdown_to_html %}
        Title
        =====

        Hello!
    {% endapply %}

Вы также можете использовать фильтр для включенного файла или переменной:

.. code-block:: twig

    {{ include('some_template.markdown.twig')|markdown_to_html }}

    {{ changelog|markdown_to_html }}

.. note::

    Фильтр ``markdown_to_html`` является частью ``MarkdownExtension``, которое
    не установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/markdown-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в среде Twig::

        use Twig\Extra\Markdown\MarkdownExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new MarkdownExtension());

    Если вы не используете Symfony, вы также должны зарегистрировать расширение выполнения::

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

    После этого вам нужно установить библиотеку разметки по вашему выбору. Некоторые из них
    упомянуты в разделе ``require-dev`` пакета ``twig/markdown-extra``.

.. note::

    Если вы используете Symfony (полный стек), с ``twig/extra-bundle`` и ``league/commonmark`` в качестве
    вашей библиотеки Markdown, вы можете сконфигурировать расширение CommonMark. Зарегистрируйте желаемое(е)           расширение(и) как сервис, а затем пометьте сервис тегом ``twig.markdown.league_extension``.
