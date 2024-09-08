``verbatim``
============

Тег ``verbatim`` позначає розділи як необроблений текст, який не слід 
аналізувати. Наприклад, щоб додати синтаксис Twig як приклад у шаблон, ви можете використати
цей фрагмент:

.. code-block:: html+twig

    {% verbatim %}
        <ul>
        {% for item in seq %}
            <li>{{ item }}</li>
        {% endfor %}
        </ul>
    {% endverbatim %}
