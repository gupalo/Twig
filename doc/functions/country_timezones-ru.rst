``country_timezones``
=====================

Функция ``country_timezones`` возвращает названия часовых поясов, связанных
с заданным кодом страны:

.. code-block:: twig

    {# Europe/Paris #}
    {{ country_timezones('FR')|join(', ') }}

.. note::

    Функция ``country_timezones`` является частью ``IntlExtension``, которое не
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
