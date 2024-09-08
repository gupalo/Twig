``locale_names``
================

.. versionadded:: 3.5

    Функція ``locale_names`` була представлена в Twig 3.5.

Функція ``locale_names`` повертає назви локалей:

.. code-block:: twig

    {# Afrikaans, Afrikaans (Namibia), ... #}
    {{ locale_names()|join(', ') }}
    
За замовчуванням функція використовує поточну локаль. Ви можете передати її явно:

.. code-block:: twig

    {# afrikaans, afrikaans (Afrique du Sud), ... #}
    {{ locale_names('fr')|join(', ') }}

.. note::

    Функція ``locale_names`` є частиною ``IntlExtension``, яке не
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
