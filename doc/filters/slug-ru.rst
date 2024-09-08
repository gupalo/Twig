``slug``
========

Фільтр ``slug`` перетворює заданий рядок на інший рядок, який
містить лише безпечні ASCII-символи. 

Нижче наведено приклад:

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug }}
    Workspace-settings

За замовчуванням роздільником між словами є тире (``-``), але ви можете 
визначити селектор на власний розсуд, передавши його як аргумент:

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug('/') }}
    Workspace/settings

Слагер автоматично визначає мову оригінального рядка, але ви також можете вказати її явно
за допомогою другого аргументу:

.. code-block:: twig

    {{ '...'|slug('-', 'ko') }}

Фільтр ``slug`` використовує однойменний метод у модулі Symfony 
`AsciiSlugger <https://symfony.com/doc/current/components/string.html#slugger>`_.

.. note::

    Фільтр ``slug`` є частиною ``StringExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/string-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

Аргументи
---------

* ``separator``: Роздільник, який використовується для з'єднання слів (за замовчуванням ``-``)
* ``locale``: Локаль початкового рядка (якщо її не вказано, вона буде визначена автоматично)
