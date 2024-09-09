``if``
======

Твердження ``if`` у Twig можна порівняти з твердженнями if у PHP.

У найпростішій формі ви можете використовувати його для перевірки того, чи вираз обчислюється як
``true``:

.. code-block:: html+twig

    {% if online == false %}
        <p>Our website is in maintenance mode. Please, come back later.</p>
    {% endif %}

Ви також можете протестувати, чи послідовність або відображення не є порожніми:

.. code-block:: html+twig

    {% if users %}
        <ul>
            {% for user in users %}
                <li>{{ user.username|e }}</li>
            {% endfor %}
        </ul>
    {% endif %}

.. note::

   Якщо ви хочете протестувати, чи визначено змінну, використовуйте ``if users is defined`` замість цього.

Ви також можете використовувати ``not`` для перевірки значень, які мають значення ``false``:

.. code-block:: html+twig

    {% if not user.subscribed %}
        <p>You are not subscribed to our mailing list.</p>
    {% endif %}

Для декількох умов можна використовувати ``and`` та ``or``:

.. code-block:: html+twig

    {% if temperature > 18 and temperature < 27 %}
        <p>It's a nice day for a walk in the park.</p>
    {% endif %}

Для декількох гілок можна використовувати ``elseif`` та ``else``, як у PHP. Ви також можете
використовувати більш складні ``expressions``:

.. code-block:: twig

    {% if product.stock > 10 %}
       Available
    {% elseif product.stock > 0 %}
       Only {{ product.stock }} left!
    {% else %}
       Sold-out!
    {% endif %}

.. note::

    Правила визначення того, чи є вираз ``true`` або ``false`` такі ж, як і в
    PHP; тут наведено правила для межевих випадків:

    ======================== ====================
    Значення                 Булева оцінка
    ======================== ====================
    порожній рядок           false
    числовий нуль            false
    NAN (не число)           true
    INF (безкінечність)      true
    рядок тільки з пробілами true
    рядок "0" або '0'        false
    порожня послідовність    false
    порожнє відображення     false
    null                     false
    непорожня послідовність  true
    непорожнє відображення   true
    обʼєкт                   true
    ======================== ====================
