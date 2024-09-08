``country_timezones``
=====================

Функція ``country_timezones`` повертає назви часових поясів, пов'язаних
із заданим кодом країни:

.. code-block:: twig

    {# Europe/Paris #}
    {{ country_timezones('FR')|join(', ') }}

.. note::

    Функція ``country_timezones`` є частиною ``IntlExtension``, яке не
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
