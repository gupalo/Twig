``locale_name``
===============

Фильтр ``locale_name`` возвращает название локали, заданное двухбуквенным кодом:

.. code-block:: twig

    {# German #}
    {{ 'de'|locale_name }}

По умолчанию фильтр использует текущую локаль. Вы можете указать ее явно:

.. code-block:: twig

    {# allemand #}
    {{ 'de'|locale_name('fr') }}

    {# français (Canada) #}
    {{ 'fr_CA'|locale_name('fr_FR') }}

.. note::

    Фильтр ``locale_name`` является частью ``IntlExtension``, которое не
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
