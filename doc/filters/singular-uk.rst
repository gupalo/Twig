``singular``
============

.. versionadded:: 3.11

    Фільтр ``singular`` було представлено в Twig 3.11.

Фільтр ``singular`` перетворює заданий іменник у формі множини в його
в однину:

.. code-block:: twig

    {# за замовчуванням використовуються правила англійської мови (en) #}
    {{ 'partitions'|singular() }}
    partition

    {{ 'partitions'|singular('fr') }}
    partition

.. note::

    Фільтр ``singular`` є частиною ``StringExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/string-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

Аргументи
---------

* ``locale``: Локаль початкового рядка (обмежується мовами, які підтримуються `inflector`_ від Symfony, частиною компонента String)
* ``all``: Чи повертати всі можливі множини у вигляді масиву, за замовчуванням ``false``

.. note::

    Внутрішньо Twig використовує метод `singularize`_ з компонента Symfony String.

.. _`inflector`: <https://symfony.com/doc/current/components/string.html#inflector>
.. _`singularize`: <https://symfony.com/doc/current/components/string.html#inflector>
