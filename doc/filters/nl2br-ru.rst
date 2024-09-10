``nl2br``
=========

Фильтр ``nl2br`` вставляет разрывы строк HTML перед всеми новыми строчками в строке:

.. code-block:: html+twig

    {{ "I like Twig.\nYou will like it too."|nl2br }}
    {# выводит

        I like Twig.<br />
        You will like it too.

    #}

.. note::

    Фильтр ``nl2br`` предварительно экранирует вводимые данные перед применением
    преобразования.
