``include``
===========

Функція ``include`` повертає відображений зміст шаблону:

.. code-block:: twig

    {{ include('template.html') }}
    {{ include(some_var) }}

Включені шаблони мають доступ до змінних активного контексту.

Якщо ви використовуєте завантажувач файлової системи, шаблони шукаються у визначених ним шляхах.

За замовчуванням шаблону передається контекст, але ви також можете передати
додаткові змінні:

.. code-block:: twig

    {# template.html матиме доступ до змінних з поточного контекту та тих, що надані додатково #}
    {{ include('template.html', {foo: 'bar'}) }}

Ви можете відключити доступ до контексту, встановивши ``with_context`` у значення
``false``:

.. code-block:: twig

    {# буде доступною лише змінна foo #}
    {{ include('template.html', {foo: 'bar'}, with_context = false) }}

.. code-block:: twig

    {# жодна змінна не буде доступною #}
    {{ include('template.html', with_context = false) }}

І якщо вираз призводить до ``\Twig\Template`` або до екземпляра
``\Twig\TemplateWrapper``, Twig використає його безпосередньо::

    // {{ include(template) }}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', ['template' => $template]);

Якщо ви встановите прапорець ``ignore_missing``, Twig поверне порожній рядок, якщо
шаблон не існує:

.. code-block:: twig

    {{ include('sidebar.html', ignore_missing = true) }}

Ви також можете надати список шаблонів, які перевіряються на існування перед
включенням. Буде відображено перший знайдений шаблон:

.. code-block:: twig

    {{ include(['page_detailed.html', 'page.html']) }}

Якщо встановлено ``ignore_missing``, резервно буде повернено до відображення нічого, якщо не існує жодного
з шаблонів, інакше буде викликано виключення.

При включенні шаблону, створеного кінцевим користувачем, вам слід подумати про те, щоб
ізолювати його:

.. code-block:: twig

    {{ include('page.html', sandboxed = true) }}

Аргументи
---------

* ``template``:       Шаблон для відображення
* ``variables``:      Змінні для передачі шаблону
* ``with_context``:   Чи передавати поточні змінні контексту
* ``ignore_missing``: Чи ігнорувати відсутні шаблони
* ``sandboxed``:      Чи використовувати пісочницю для шаблону
