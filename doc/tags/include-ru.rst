``include``
===========

Твердження ``include`` включає шаблон і виводить відображений зміст
цього файлу:

.. code-block:: twig

    {% include 'header.html' %}
        Body
    {% include 'footer.html' %}

.. note::

    Рекомендується використовувати функцію :doc:`include<../functions/include>` натомість,
    оскільки вона надає ті самі можливості з трохи більшою гнучкістю:

    * Функція ``include`` є семантично більш "правильною" ( включення шаблону виводить його відображений зміст у        поточній області видимості; тег не повинен
      нічого не виводити);

    * Функція ``include`` є більш "композиційною":

      .. code-block:: twig

          {# Зберегти відображений шаблон у змінній #}
          {% set content %}
              {% include 'template.html' %}
          {% endset %}
          {# vs #}
          {% set content = include('template.html') %}

          {# Застосувати фільтр до відображеного шаблону #}
          {% apply upper %}
              {% include 'template.html' %}
          {% endapply %}
          {# vs #}
          {{ include('template.html')|upper }}

    * Функція ``include`` не накладає ніякого певного порядку для
      аргументів завдяки :ref:`іменованим аргументам <named-arguments>`.

Включені шаблони мають доступ до змінних активного контексту.

Якщо ви використовуєте завантажувач файлової системи, шаблони шукаються у
визначених ним шляхах.

Ви можете додати додаткові змінні, передавши їх після ключового слова ``with``:

.. code-block:: twig

    {# template.html матиме доступ до змінних з поточного контексту та наданих додатково #}
    {% include 'template.html' with {'foo': 'bar'} %}

    {% set vars = {'foo': 'bar'} %}
    {% include 'template.html' with vars %}

Ви можете відключити доступ до контексту, додавши ключове слово ``only``:

.. code-block:: twig

    {# доступною буде тільки змінна foo #}
    {% include 'template.html' with {'foo': 'bar'} only %}

.. code-block:: twig

    {# жодна змінна не буде доступною #}
    {% include 'template.html' only %}

.. tip::

    При включенні шаблону, створеного кінцевим користувачем, слід подумати про те,
    щоб пропустити його через пісочницю. Більше інформації у розділі 
    :doc:`Twig для розробників<../api>` та у документації до тегу :doc:`sandbox<../tags/sandbox>`.

Ім'я шаблону може бути будь-яким валідним виразом Twig:

.. code-block:: twig

    {% include some_var %}
    {% include ajax ? 'ajax.html' : 'not_ajax.html' %}

І якщо вираз призводить до ``\Twig\Template`` або до екземпляра
``\Twig\TemplateWrapper``, Twig використає його напряму::

    // {% include template %}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', ['template' => $template]);

Ви можете позначити включення за допомогою ``ignore missing``, у цьому випадку Twig проігнорує
твердження, якщо шаблон, який потрібно включити, не існує. Це має бути вказано
одразу після назви шаблону. Ось кілька валідних прикладів:

.. code-block:: twig

    {% include 'sidebar.html' ignore missing %}
    {% include 'sidebar.html' ignore missing with {'foo': 'bar'} %}
    {% include 'sidebar.html' ignore missing only %}

Ви також можете надати список шаблонів, які перевіряються на існування перед
включенням. Перший шаблон, який існує, буде включено:

.. code-block:: twig

    {% include ['page_detailed.html', 'page.html'] %}

Якщо надано``ignore missing``, в якості резерву буде повернено до відображення, 
якщо не існує жодного, інакше буде викликано виключення.
