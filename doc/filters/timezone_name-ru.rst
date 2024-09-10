``timezone_name``
=================

Фильтр ``timezone_name`` возвращает название часового пояса по идентификатору часового пояса:

.. code-block:: twig

    {# Центральноевропейское время (Париж) #}
    {{ 'Europe/Paris'|timezone_name }}

    {# Тихоокеанское время (Лос-Анджелес) #}
    {{ 'America/Los_Angeles'|timezone_name }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# heure du Pacifique nord-américain (Los Angeles) #}
    {{ 'America/Los_Angeles'|timezone_name('fr') }}

.. note::

    Фильтр ``timezone_name`` является частью ``IntlExtension``, которое не
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
