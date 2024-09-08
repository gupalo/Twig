``format``
==========

Фільтр ``format`` форматує заданий рядок, замінюючи заповнювачі
(заповнювачі слідують нотації `sprintf`_):

.. code-block:: twig

    {{ "I like %s and %s."|format(foo, "bar") }}

    {# виводить I like foo and bar,
       якщо параметр foo дорівнює рядку foo. #}

.. seealso::

    :doc:`replace<replace>`

.. _`sprintf`: https://www.php.net/sprintf
