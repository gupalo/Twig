``include``
===========

Утверждение ``include`` включает шаблон и выводит отображаемое содержание
этого файла:

.. code-block:: twig

    {% include 'header.html' %}
        Body
    {% include 'footer.html' %}

.. note::

    Рекомендуется использовать функцию :doc:`include<../functions/include>`
    вместо этого, поскольку она предоставляет те же возможности, с немного
    большей гибкостью:

    * Функция ``include`` является семантически более "правильной" ( включение шаблона выводит его отображенное         содержание в текущей области видимости; тег не должен
      ничего не выводить);

    * Функция ``include`` является более "композиционной":

      .. code-block:: twig

          {# Сохранить отображенный шаблон в переменной #}
          {% set content %}
              {% include 'template.html' %}
          {% endset %}
          {# vs #}
          {% set content = include('template.html') %}

          {# Применить фильтр к отображенному шаблону #}
          {% apply upper %}
              {% include 'template.html' %}
          {% endapply %}
          {# vs #}
          {{ include('template.html')|upper }}

    * Функция ``include`` не накладывает никакого определенного порядка для аргументов, благодаря                   :ref:`именованным аргументам <named-arguments>`.

Включенные шаблоны имеют доступ к переменным активного контекста.

Если вы используете загрузчик файловой системы, шаблоны ищут в
определенных им путях.

Вы можете добавить дополнительные переменные, передав их после ключевого слова ``with``:

.. code-block:: twig

    {# template.html матиме доступ до змінних з поточного контексту та наданих додатково #}
    {% include 'template.html' with {'foo': 'bar'} %}

    {% set vars = {'foo': 'bar'} %}
    {% include 'template.html' with vars %}

Вы можете отключить доступ к контексту, добавив ключевое слово ``only``:

.. code-block:: twig

    {# доступной будет тольки переменная foo #}
    {% include 'template.html' with {'foo': 'bar'} only %}

.. code-block:: twig

    {# ни одна переменная не будет доступна #}
    {% include 'template.html' only %}

.. tip::

    При включении шаблона, созданного конечным пользователем, следует подумать о том,
    чтобы пропустить его через песочницу. Больше информации в разделе 
    :doc:`Twig для разработчиков<../api>` и в документации к тегу :doc:``sandbox<../tags/sandbox>`.

Имя шаблона может быть любым валидным выражением Twig:

.. code-block:: twig

    {% include some_var %}
    {% include ajax ? 'ajax.html' : 'not_ajax.html' %}

И если выражение приводит к ``\Twig\Template`` или к экземпляру
``\Twig\TemplateWrapper``, Twig использует его напрямую::

    // {% include template %}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', ['template' => $template]);

Вы можете обозначить включение с помощью ``ignore missing``, в этом случае Twig проигнорирует
утверждение, если шаблон, который нужно включить, не существует. Это должно быть указано
сразу после названия шаблона. Вот несколько валидных примеров:

.. code-block:: twig

    {% include 'sidebar.html' ignore missing %}
    {% include 'sidebar.html' ignore missing with {'foo': 'bar'} %}
    {% include 'sidebar.html' ignore missing only %}

Вы также можете предоставить список шаблонов, которые проверяются на существование перед
включением. Первый шаблон, который существует, будет включен:

.. code-block:: twig

    {% include ['page_detailed.html', 'page.html'] %}

Если предоставлено ``ignore missing``, в качестве резерва будет возвращено к отображению, 
если ни одного не существует, иначе будет вызвано исключение.
