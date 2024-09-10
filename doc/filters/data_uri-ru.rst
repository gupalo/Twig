``data_uri``
============

Фильтр ``data_uri`` генерирует URL-адрес, используя схему данных, определенную в
`RFC 2397`_:

.. code-block:: html+twig

    {{ image_data|data_uri }}

    {{ source('path_to_image')|data_uri }}

    {# форсировать mime-тип, выключить угадывание mime-типа #}
    {{ image_data|data_uri(mime: "image/svg") }}

    {# также работает с простым текстом #}
    {{ '<b>foobar</b>'|data_uri(mime: "text/html") }}

    {# добавить дополнительные параметры #}
    {{ '<b>foobar</b>'|data_uri(mime: "text/html", parameters: {charset: "ascii"}) }}

.. note::

    Фильтр ``data_uri`` является частью ``HtmlExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/html-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());

.. note::

    Фильтр не выполняет валидацию длины намеренно (ограничение зависит от
    от контекста использования), валидацию следует выполнять перед вызовом этого фильтра.

Аргументы
---------

* ``mime``: Mime-тип
* ``parameters``: Отображение параметров

.. _RFC 2397: https://tools.ietf.org/html/rfc2397
