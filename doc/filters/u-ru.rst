``u``
=====

Фильтр ``u`` обертывает текст в объект Unicode (экземпляр `Symfony UnicodeString <https://symfony.com/doc/current/components/string.html>`_), который раскрывает методы
для "манипулирования" строкой.

Рассмотрим некоторые распространенные случаи использования.

Обертывание текста до заданного количества символов:

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5) }}
    Symfony
    String
    +
    Twig
    = <3

Обрезание строки:

.. code-block:: twig

    {{ 'Lorem ipsum'|u.truncate(8) }}
    Lorem ip

    {{ 'Lorem ipsum'|u.truncate(8, '...') }}
    Lorem...

Метод ``truncate`` также принимает третий аргумент для сохранения целых слов:

.. code-block:: twig

    {{ 'Lorem ipsum dolor'|u.truncate(10, '...', false) }}
    Lorem ipsum...

Преобразование строки в регистр *snake* или *camelCase*:

.. code-block:: twig

    {{ 'SymfonyStringWithTwig'|u.snake }}
    symfony_string_with_twig

    {{ 'symfony_string with twig'|u.camel.title }}
    SymfonyStringWithTwig

Вы также можете объединить методы в цепочку:

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5).upper }}
    SYMFONY
    STRING
    +
    TWIG
    = <3

Для манипуляций с большими строками используйте тег ``apply``:

.. code-block:: twig

    {% apply u.wordwrap(5) %}
        Some large amount of text...
    {% endapply %}

.. note::

    Фильтр ``u`` является частью ``StringExtension``, которое не установлено
    по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/string-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());
