``timezone_names``
==================

.. versionadded:: 3.5

    Функція ``timezone_names`` була представлена в Twig 3.5.

Функція ``timezone_names`` повертає назви часових поясів:

.. code-block:: twig

    {# Acre Time (Eirunepe), Acre Time (Rio Branco), ... #}
    {{ timezone_names()|join(', ') }}
    
За замовчуванням функція використовує поточну локаль. Ви можете передати її явно:

.. code-block:: twig

    {# heure : Antarctique (Casey), heure : Canada (Montreal), ... #}
    {{ timezone_names('fr')|join(', ') }}

.. note::

    Функція ``timezone_names`` є частиною ``IntlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовище Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументи
---------

* ``locale``: Локаль
