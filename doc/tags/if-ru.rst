``if``
======

Утверждение ``if`` в Twig можно сравнить с утверждениями if в PHP.

В простейшей форме вы можете использовать его для проверки того, определяется ли выражение как
``true``:

.. code-block:: html+twig

    {% if online == false %}
        <p>Our website is in maintenance mode. Please, come back later.</p>
    {% endif %}

Вы также можете проверить, не является ли последовательность или отображение пустым:

.. code-block:: html+twig

    {% if users %}
        <ul>
            {% for user in users %}
                <li>{{ user.username|e }}</li>
            {% endfor %}
        </ul>
    {% endif %}

.. note::

   Если вы хотите протестировать, определена ли переменная, используйте ``if users is defined`` вместо этого.

Вы также можете использовать ``not`` для проверки значений, которые имеют значение ``false``:

.. code-block:: html+twig

    {% if not user.subscribed %}
        <p>You are not subscribed to our mailing list.</p>
    {% endif %}

Для нескольких условий можно использовать ``and`` и ``or``:

.. code-block:: html+twig

    {% if temperature > 18 and temperature < 27 %}
        <p>It's a nice day for a walk in the park.</p>
    {% endif %}

Для нескольких ветвей можно использовать ``elseif`` и ``else``, как в PHP. Вы также можете
использовать более сложные ``expressions``:

.. code-block:: twig

    {% if product.stock > 10 %}
       Available
    {% elseif product.stock > 0 %}
       Only {{ product.stock }} left!
    {% else %}
       Sold-out!
    {% endif %}

.. note::

    Правила определения того, является ли выражение ``true`` или ``false`` такие
    же, как и в PHP; здесь приведены правила для граничных случаев:

    =========================== ====================
    Значение                    Булевая оценка
    =========================== ====================
    пустая строка               false
    числовой ноль               false
    NAN (не число)              true
    INF (бесконечность)         true
    строка только с пробелами   true
    строка "0" або '0'          false
    пустая последовательность   false
    пустое отображение          false
    null                        false
    непустая последовательность true
    непустое отображение        true
    объект                      true
    ========================== ====================
