``country_names``
=================

.. versionadded:: 3.5

    Функція ``country_names`` була представлена в Twig 3.5.

Функція ``country_names`` повертає назви країн:

.. code-block:: twig

    {# Afghanistan, Åland Islands, ... #}
    {{ country_names()|join(', ') }}
    
За замовчуванням функція використовує поточну локаль. Ви можете передати її явно:

.. code-block:: twig

    {# Afghanistan, Afrique du Sud, ... #}
    {{ country_names('fr')|join(', ') }}

.. note::

    Функція ``country_names`` є частиною ``IntlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументи
---------

* ``locale``: Локаль
