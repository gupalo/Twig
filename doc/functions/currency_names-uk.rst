``currency_names``
==================

.. versionadded:: 3.5

    Функція ``currency_names`` була представлена в Twig 3.5.

Функція ``currency_names`` повертає назви валют:

.. code-block:: twig

    {# Afghan Afghani, Afghan Afghani (1927–2002), ... #}
    {{ currency_names()|join(', ') }}
    
За замовчуванням функція використовує поточну локаль. Ви можете передати її явно:

.. code-block:: twig

    {# afghani (1927–2002), afghani afghan, ... #}
    {{ currency_names('fr')|join(', ') }}

.. note::

    Функція ``currency_names`` є частиною ``IntlExtension``, яке не
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
