``constant``
============

``constant`` проверяет, имеет ли переменная точно такое же значение, как и константа. Вы
можете использовать как глобальные константы, так и константы класса:

.. code-block:: twig

    {% if post.status is constant('Post::PUBLISHED') %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}

Вы также можете тестировать константы из экземпляров объектов:

.. code-block:: twig

    {% if post.status is constant('PUBLISHED', post) %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}
