``url_encode``
==============

Фільтр ``url_encode`` кодує заданий рядок як сегмент URL-адреси або
відображення як рядок запиту:

.. code-block:: twig

    {{ "path-seg*ment"|url_encode }}
    {# виводить "path-seg%2Ament" #}

    {{ "string with spaces"|url_encode }}
    {# виводить "string%20with%20spaces" #}

    {{ {'param': 'value', 'foo': 'bar'}|url_encode }}
    {# виводить "param=value&foo=bar" #}

.. note::

    Внутрішньо Twig використовує PHP-функцію `rawurlencode`_ або `http_build_query`_.

.. _`rawurlencode`: https://www.php.net/rawurlencode
.. _`http_build_query`: https://www.php.net/http_build_query
