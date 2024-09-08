``u``
=====

Фільтр ``u`` обгортає текст в об'єкт Unicode (екземпляр `Symfony UnicodeString <https://symfony.com/doc/current/components/string.html>`_), який розкриває методи
для «маніпулювання» рядком.

Розглянемо деякі поширені випадки використання.

Обгортання тексту до заданої кількості символів:

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5) }}
    Symfony
    String
    +
    Twig
    = <3

Обрізання рядка:

.. code-block:: twig

    {{ 'Lorem ipsum'|u.truncate(8) }}
    Lorem ip

    {{ 'Lorem ipsum'|u.truncate(8, '...') }}
    Lorem...

Метод ``truncate`` також приймає третій аргумент для збереження цілих слів:

.. code-block:: twig

    {{ 'Lorem ipsum dolor'|u.truncate(10, '...', false) }}
    Lorem ipsum...

Перетворення рядка на регістр *snake* або *camelCase*:

.. code-block:: twig

    {{ 'SymfonyStringWithTwig'|u.snake }}
    symfony_string_with_twig

    {{ 'symfony_string with twig'|u.camel.title }}
    SymfonyStringWithTwig

Ви також можете об'єднати методи в ланцюжок:

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5).upper }}
    SYMFONY
    STRING
    +
    TWIG
    = <3

Для маніпуляцій з великими рядками використовуйте тег ``apply``:

.. code-block:: twig

    {% apply u.wordwrap(5) %}
        Some large amount of text...
    {% endapply %}

.. note::

    Фільтр ``u`` є частиною ``StringExtension``, яке не встановлено
    за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/string-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовище Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());
