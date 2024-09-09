``country_names``
=================

.. versionadded:: 3.5

    Функция ``country_names`` была представлена в Twig 3.5.

Функция ``country_names`` возвращает названия стран:

.. code-block:: twig

    {# Afghanistan, Åland Islands, ... #}
    {{ country_names()|join(', ') }}
    
По умолчанию функция использует текущую локаль. Вы можете передать ее явно:

.. code-block:: twig

    {# Afghanistan, Afrique du Sud, ... #}
    {{ country_names('fr')|join(', ') }}

.. note::

    Функция ``country_names`` является частью ``IntlExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументы
---------

* ``locale``: Локаль
