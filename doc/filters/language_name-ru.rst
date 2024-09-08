``language_name``
=================

Фільтр ``language_name`` повертає назву мови за її двобуквеним кодом:

.. code-block:: twig

    {# German #}
    {{ 'de'|language_name }}

За замовчуванням фільтр використовує поточну локаль. Ви можете вказати її явно:

.. code-block:: twig

    {# allemand #}
    {{ 'de'|language_name('fr') }}

    {# français canadien #}
    {{ 'fr_CA'|language_name('fr_FR') }}

.. note::

    Фільтр ``language_name`` є частиною ``IntlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/intl-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

Аргументи
---------

* ``locale``: Локаль
