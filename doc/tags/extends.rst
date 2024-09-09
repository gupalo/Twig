``extends``
===========

Тег ``extends`` можна використовувати для розширення шаблону з іншого шаблону.

.. note::

    Як і PHP, Twig не підтримує множинну спадковість. Отже, ви можете мати лише
    один тег extends, який викликається для кожного відображення. Однак, Twig
    підтримує горизонтальне :doc:`повторне використання<use>`.

Давайте визначимо базовий шаблон ``base.html``, який визначає простий 
скелет HTML-документа:

.. code-block:: html+twig

    <!DOCTYPE html>
    <html>
        <head>
            {% block head %}
                <link rel="stylesheet" href="style.css"/>
                <title>{% block title %}{% endblock %} - My Webpage</title>
            {% endblock %}
        </head>
        <body>
            <div id="content">{% block content %}{% endblock %}</div>
            <div id="footer">
                {% block footer %}
                    &copy; Copyright 2011 by <a href="https://example.com/">you</a>.
                {% endblock %}
            </div>
        </body>
    </html>

У цьому прикладі теги :doc:`block<block>` визначають чотири блоки, які можуть заповнювати
дочірні шаблони.

Тег ``block`` вказує движку шаблонів, що дочірній шаблон може перевизначити ці частини
шаблону.

Дочірній шаблон
---------------

Дочірній шаблон може виглядати так:

.. code-block:: html+twig

    {% extends "base.html" %}

    {% block title %}Index{% endblock %}
    {% block head %}
        {{ parent() }}
        <style type="text/css">
            .important { color: #336699; }
        </style>
    {% endblock %}
    {% block content %}
        <h1>Index</h1>
        <p class="important">
            Welcome on my awesome homepage.
        </p>
    {% endblock %}

Ключовим тут є тег ``extends``. Він повідомляє движку шаблонів, що цей
шаблон "розширює" інший шаблон. Коли система шаблонів оцінює цей
шаблон, спочатку вона знаходить батьківський. Тег extends повинен бути першим тегом
у шаблоні.

Зауважте, що оскільки дочірній шаблон не визначає блок ``footer``, 
замість нього використовується значення з батьківського шаблону.

Ви не можете визначити декілька тегів ``block`` з однаковими іменами в одному
шаблоні. Це обмеження існує тому, що блок-тег працює в "обох" напрямках. 
Тобто, тег блоку не просто надає прогалину для заповнення - він також
визначає зміст, який заповнює прогалину в *батькові*. Якби у шаблоні існувало два
однойменних теги ``block``, батько шаблону не знатиме, зміст якого з блоків використовувати.

Якщо ви хочете вивести блок декілька разів, ви можете використати функцію ``block``:

.. code-block:: html+twig

    <title>{% block title %}{% endblock %}</title>
    <h1>{{ block('title') }}</h1>
    {% block body %}{% endblock %}

Батьківські блоки
-----------------

Відобразити зміст батьківського блоку можна за допомогою функції
:doc:`parent<../functions/parent>`. Вона повертає результати 
батьківського блоку:

.. code-block:: html+twig

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

Іменовані кінцеві теги блоків
-----------------------------

Twig дозволяє розміщувати ім'я блоку після кінцевого тегу для кращої
читабельності ( ім'я після слова ``endblock`` має збігатися з ім'ям блоку):

.. code-block:: twig

    {% block sidebar %}
        {% block inner_sidebar %}
            ...
        {% endblock inner_sidebar %}
    {% endblock sidebar %}

Вкладеність та область дії блоку
--------------------------------

Блоки можна вкладати для більш складних макетів. За замовчуванням блоки мають доступ
до змінних із зовнішніх областей видимості:

.. code-block:: html+twig

    {% for item in seq %}
        <li>{% block loop_item %}{{ item }}{% endblock %}</li>
    {% endfor %}

Скорочення блоків
-----------------

Для блоків з невеликим змістом можна використовувати синтаксис скороченого коду. 
Наступні конструкції роблять одне і те ж саме:

.. code-block:: twig

    {% block title %}
        {{ page_title|title }}
    {% endblock %}

.. code-block:: twig

    {% block title page_title|title %}

Динамічне успадкування
----------------------

Twig підтримує динамічне успадкування, використовуючи змінну як базовий шаблон:

.. code-block:: twig

    {% extends some_var %}

Якщо змінна призводиться до ``\Twig\Template`` або ``\Twig\TemplateWrapper``,
то Twig буде використовувати її як батьківський шаблон::

    // {% extends layout %}

    $layout = $twig->load('some_layout_template.twig');

    $twig->display('template.twig', ['layout' => $layout]);

Ви також можете вказати список шаблонів, які перевіряються на існування. Перший 
знайдений шаблон буде використано як батьківський:

.. code-block:: twig

    {% extends ['layout.html', 'base_layout.html'] %}

Умовне успадкування
-------------------

Оскільки ім'я шаблону для батька може бути будь-яким валідним виразом Twig, то
можна зробити механізм успадкування умовним:

.. code-block:: twig

    {% extends standalone ? "minimum.html" : "base.html" %}

У цьому прикладі шаблон розширить шаблон макета "minimum.html",
якщо змінна ``standalone`` має значення ``true``, і «base.html» - в іншому випадку.

Як працюють блоки?
------------------

Блок надає можливість змінити спосіб відображення певної частини шаблону, 
але він ніяк не втручається в логіку роботи шаблону.

Розглянемо наступний приклад, щоб проілюструвати, як працює блок і, що важливіше,
як він не працює:

.. code-block:: html+twig

    {# base.twig #}
    {% for post in posts %}
        {% block post %}
            <h1>{{ post.title }}</h1>
            <p>{{ post.body }}</p>
        {% endblock %}
    {% endfor %}

Якщо ви відобразите цей шаблон, результат буде абсолютно однаковим як з тегом ``lock``,
так і без нього. Тег ``block`` всередині циклу ``for`` - це лише спосіб зробити його
перевизначуваним для дочірнього шаблону:

.. code-block:: html+twig

    {# child.twig #}
    {% extends "base.twig" %}

    {% block post %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endblock %}

Тепер при відображенні дочірнього шаблону цикл буде використовувати блок, визначений
у дочірньому шаблоні, замість визначеного у базовому; виконаний шаблон буде еквівалентний
наступному:

.. code-block:: html+twig

    {% for post in posts %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endfor %}

Розглянемо інший приклад: блок, включений в твердження ``if``:

.. code-block:: html+twig

    {% if posts is empty %}
        {% block head %}
            {{ parent() }}

            <meta name="robots" content="noindex, follow">
        {% endblock head %}
    {% endif %}

На відміну від того, що ви можете подумати, цей шаблон не визначає блок
умовно; він просто робить так, щоб дочірній шаблон міг перевизначити виведення,
коли умова буде ``true``.

Якщо ви хочете, щоб виведення відображалося умовно, використовуйте наступне
замість цього:

.. code-block:: html+twig

    {% block head %}
        {{ parent() }}

        {% if posts is empty %}
            <meta name="robots" content="noindex, follow">
        {% endif %}
    {% endblock head %}

.. seealso::

    :doc:`block<../functions/block>`, :doc:`block<../tags/block>`, :doc:`parent<../functions/parent>`, :doc:`use<../tags/use>`
