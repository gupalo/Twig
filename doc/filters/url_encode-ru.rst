``url_encode``
==============

Фильтр ``url_encode`` шифрует заданную строку как сегмент URL-адреса или
отображение как строку запроса:

.. code-block:: twig

    {{ "path-seg*ment"|url_encode }}
    {# выводит "path-seg%2Ament" #}

    {{ "string with spaces"|url_encode }}
    {# выводит "string%20with%20spaces" #}

    {{ {'param': 'value', 'foo': 'bar'}|url_encode }}
    {# выводит "param=value&foo=bar" #}

.. note::

    Внутренне Twig использует PHP-функцию `rawurlencode`_ или `http_build_query`_.

.. _`rawurlencode`: https://www.php.net/rawurlencode
.. _`http_build_query`: https://www.php.net/http_build_query
