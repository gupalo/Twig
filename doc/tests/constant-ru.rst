``constant``
============

``constant`` перевіряє, чи змінна має точно таке ж значення, як і константа. Ви
можете використовувати як глобальні константи, так і константи класу:

.. code-block:: twig

    {% if post.status is constant('Post::PUBLISHED') %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}

Ви також можете тестувати константи з екземплярів об'єктів:

.. code-block:: twig

    {% if post.status is constant('PUBLISHED', post) %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}
