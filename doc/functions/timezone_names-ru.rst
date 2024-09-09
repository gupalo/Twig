``timezone_names``
==================

.. versionadded:: 3.5

    Функция ``timezone_names`` была представлена в Twig 3.5.

Функция ``timezone_names`` возвращает названия часовых поясов:

.. code-block:: twig

    {# Acre Time (Eirunepe), Acre Time (Rio Branco), ... #}
    {{ timezone_names()|join(', ') }}
    
По умолчанию функция использует текущую локаль. Вы можете передать ее явно:

.. code-block:: twig

    {# heure : Antarctique (Casey), heure : Canada (Montreal), ... #}
    {{ timezone_names('fr')|join(', ') }}

.. note::

    Функция ``timezone_names`` является частью ``IntlExtension``, которое не
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
