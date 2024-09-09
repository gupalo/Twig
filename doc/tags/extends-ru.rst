``extends``
===========

Тег ``extends`` может быть использован для расширения шаблона из другого шаблона.

.. note::

    Как и PHP, Twig не поддерживает множественную наследственность. Следовательно,
    у вас может быть только один тег extends, который вызывается для каждого
    отображения. Однако Twig поддерживает горизонтальное :doc:`повторное использование<use>`.

Давайте определим базовый шаблон ``base.html``, который определяет простой 
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

В этом примере теги :doc:`block<block>` определяют четыре блока, которые могут заполнять
дочерние шаблоны.

Тег ``block`` указывает движку шаблонов, что дочерний шаблон может переопределить эти части
шаблона.

Дочерний шаблон
---------------

Дочерний шаблон может выглядеть так:

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

Ключевым здесь является тег ``extends``. Он сообщает движку шаблонов, что этот
шаблон «расширяет» другой шаблон. Когда система шаблонов оценивает этот
шаблон, сначала она находит родительский. Тег extends должен быть первым тегом
в шаблоне.

Заметьте, что поскольку дочерний шаблон не определяет блок ``footer``, 
вместо него используется значение из родительского шаблона.

Вы не можете определить несколько тегов ``block`` с одинаковыми именами в одном
шаблоне. Это ограничение существует потому, что тег блока работает в "обоих" направлениях. 
То есть, тег блока не просто предоставляет пробел для заполнения - он также
определяет содержание, которое заполняет пробел в *родителе*. Если бы в шаблоне существовало два
одноименных тега ``block``, родитель шаблона не будет знать, содержание какого из блоков использовать.

Если вы хотите вывести блок несколько раз, вы можете использовать функцию ``block``:

.. code-block:: html+twig

    <title>{% block title %}{% endblock %}</title>
    <h1>{{ block('title') }}</h1>
    {% block body %}{% endblock %}

Родительские блоки
------------------

Отобразить содержание родительского блока можно с помощью функции
:doc:`parent<../functions/parent>`. Она возвращает результаты 
родительского блока:

.. code-block:: html+twig

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

Именованные конечные теги блоков
--------------------------------

Twig позволяет размещать имя блока после конечного тега для лучшей
читабельности (имя после слова ``endblock`` должно совпадать с именем блока):

.. code-block:: twig

    {% block sidebar %}
        {% block inner_sidebar %}
            ...
        {% endblock inner_sidebar %}
    {% endblock sidebar %}

Вложенность и область действия блока
------------------------------------

Блоки можно вкладывать для более сложных макетов. По умолчанию блоки имеют доступ к
к переменным из внешних областей действия:

.. code-block:: html+twig

    {% for item in seq %}
        <li>{% block loop_item %}{{ item }}{% endblock %}</li>
    {% endfor %}

Сокращения блоков
-----------------

Для блоков с небольшим содержанием можно использовать синтаксис сокращенного кода. 
Следующие конструкции делают одно и то же самое:

.. code-block:: twig

    {% block title %}
        {{ page_title|title }}
    {% endblock %}

.. code-block:: twig

    {% block title page_title|title %}

Динамическое наследование
-------------------------

Twig поддерживает динамическое наследование, используя переменную в качестве базового шаблона:

.. code-block:: twig

    {% extends some_var %}

Если переменная приводится к ``\Twig\Template`` или ``\Twig\TemplateWrapper``,
то Twig будет использовать ее как родительский шаблон::

    // {% extends layout %}

    $layout = $twig->load('some_layout_template.twig');

    $twig->display('template.twig', ['layout' => $layout]);

Вы также можете указать список шаблонов, которые проверяются на существование. Первый 
найденный шаблон будет использован как родительский:

.. code-block:: twig

    {% extends ['layout.html', 'base_layout.html'] %}

Условное наследование
---------------------

Поскольку имя шаблона для родителя может быть любым валидным выражением Twig, то
можно сделать механизм наследования условным:

.. code-block:: twig

    {% extends standalone ? "minimum.html" : "base.html" %}

В этом примере шаблон расширит шаблон макета "minimum.html", если переменная
``standalone`` имеет значение ``true``, и "base.html" - в противном случае.

Как работают блоки?
-------------------

Блок предоставляет возможность изменить способ отображения определенной части шаблона, 
но он никак не вмешивается в логику работы шаблона.

Рассмотрим следующий пример, чтобы проиллюстрировать, как работает блок и, что важнее,
как он не работает:

.. code-block:: html+twig

    {# base.twig #}
    {% for post in posts %}
        {% block post %}
            <h1>{{ post.title }}</h1>
            <p>{{ post.body }}</p>
        {% endblock %}
    {% endfor %}

Если вы отобразите этот шаблон, результат будет абсолютно одинаковым как с тегом ``lock``,
так и без него. Тег ``block`` внутри цикла ``for`` - это лишь способ сделать его
переопределяемым для дочернего шаблона:

.. code-block:: html+twig

    {# child.twig #}
    {% extends "base.twig" %}

    {% block post %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endblock %}

Теперь при отображении дочернего шаблона цикл будет использовать блок, определенный
в дочернем шаблоне, вместо определенного в базовом; выполненный шаблон будет эквивалентен
следующему:

.. code-block:: html+twig

    {% for post in posts %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endfor %}

Рассмотрим другой пример: блок, включенный в утверждение ``if``:

.. code-block:: html+twig

    {% if posts is empty %}
        {% block head %}
            {{ parent() }}

            <meta name="robots" content="noindex, follow">
        {% endblock head %}
    {% endif %}

В отличие от того, что вы можете подумать, этот шаблон не определяет блок
условно; он просто делает так, чтобы дочерний шаблон мог переопределить вывод,
если условие будет ``true``.

Если вы хотите, чтобы вывод отображался условно, используйте следующее
вместо этого:

.. code-block:: html+twig

    {% block head %}
        {{ parent() }}

        {% if posts is empty %}
            <meta name="robots" content="noindex, follow">
        {% endif %}
    {% endblock head %}

.. seealso::

    :doc:`block<../functions/block>`, :doc:`block<../tags/block>`, :doc:`parent<../functions/parent>`, :doc:`use<../tags/use>`
