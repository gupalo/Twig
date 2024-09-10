``markdown_to_html``
====================

Фільтр ``markdown_to_html`` перетворює блок Markdown на HTML:

.. code-block:: twig

    {% apply markdown_to_html %}
    Title
    =====

    Hello!
    {% endapply %}

Зверніть увагу, що ви можете робити відступи у змісті Markdown, оскільки початкові пробіли 
послідовно видалятимуться перед перетворенням:

.. code-block:: twig

    {% apply markdown_to_html %}
        Title
        =====

        Hello!
    {% endapply %}

Ви також можете використовувати фільтр для включеного файлу або змінної:

.. code-block:: twig

    {{ include('some_template.markdown.twig')|markdown_to_html }}

    {{ changelog|markdown_to_html }}

.. note::

    Фільтр ``markdown_to_html`` є частиною ``MarkdownExtension``, яке
    не встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/markdown-extra

    Потім, в проєктах Symfony, установіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Markdown\MarkdownExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new MarkdownExtension());

    Якщо ви не використовуєте Symfony, ви також повинні зареєструвати розширення виконання::

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

    Після цього вам потрібно встановити бібліотеку розмітки за вашим вибором. Деякі з них
    згадано у розділі ``require-dev`` пакету ``twig/markdown-extra``.

.. note::

    Якщо ви використовуєте Symfony (повний стек), з ``twig/extra-bundle`` та ``league/commonmark`` як
    ваша бібліотека Markdown, ви можете сконфігурувати розширення CommonMark. Зареєструйте бажане(і)
    розширення як сервіс, а потім позначте сервіс тегом ``twig.markdown.league_extension``.
