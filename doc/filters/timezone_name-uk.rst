``timezone_name``
=================

Фільтр ``timezone_name`` повертає назву часового поясу за ідентифікатором часового поясу:

.. code-block:: twig

    {# Центральноєвропейський час (Париж) #}
    {{ 'Europe/Paris'|timezone_name }}

    {# Тихоокеанський час (Лос-Анджелес) #}
    {{ 'America/Los_Angeles'|timezone_name }}

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# heure du Pacifique nord-américain (Los Angeles) #}
    {{ 'America/Los_Angeles'|timezone_name('fr') }}

.. note::

    Фільтр ``timezone_name`` є частиною ``IntlExtension``, яке не
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
