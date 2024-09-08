``nl2br``
=========

Фільтр ``nl2br`` вставляє розриви рядків HTML перед усіма новими рядками у рядку:

.. code-block:: html+twig

    {{ "I like Twig.\nYou will like it too."|nl2br }}
    {# outputs

        I like Twig.<br />
        You will like it too.

    #}

.. note::

    Фільтр ``nl2br`` попередньо екранує введення дані перед застосуванням
    перетворення.
