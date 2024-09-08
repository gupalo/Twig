``data_uri``
============

Фільтр ``data_uri`` генерує URL-адресу, використовуючи схему даних, визначену в
`RFC 2397`_:

.. code-block:: html+twig

    {{ image_data|data_uri }}

    {{ source('path_to_image')|data_uri }}

    {# форсувати mime-тип, вимкнути вгадування mime-типу #}
    {{ image_data|data_uri(mime: "image/svg") }}

    {# також працює з простим текстом #}
    {{ '<b>foobar</b>'|data_uri(mime: "text/html") }}

    {# додати додаткові параметри #}
    {{ '<b>foobar</b>'|data_uri(mime: "text/html", parameters: {charset: "ascii"}) }}

.. note::

    Фільтр ``data_uri`` є частиною ``HtmlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/html-extra

    Потім, в проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно в середовищі Twig::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());

.. note::

    Фільтр не виконує валідацію довжини навмисно (обмеження залежить
    від контексту використання), валідацію слід виконувати перед викликом цього фільтра.

Аргументи
---------

* ``mime``: Mime-тип
* ``parameters``: Відображення параметрів

.. _RFC 2397: https://tools.ietf.org/html/rfc2397
