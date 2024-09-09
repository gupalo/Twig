``same as``
===========

``same as`` перевіряє, чи є змінна такою самою, як інша змінна.
Це еквівалентно ``===`` у PHP:

.. code-block:: twig

    {% if foo.attribute is same as(false) %}
        the foo attribute really is the 'false' PHP value
    {% endif %}
