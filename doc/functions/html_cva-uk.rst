``html_cva``
============

.. versionadded:: 3.12

    Функція ``html_cva`` була представлена в Twig 3.12.

`CVA (Class Variant Authority)`_ - це концепція зі світу JavaScript, яка використовується 
відомою бібліотекою `shadcn/ui`_. Концепція CVA використовується для відображення декількох
варіантів компонентів, застосовуючи набір умов і рецептів для динамічного складання рядків
класів CSS (колір, розмір і т.д.) для створення шаблонів, які можна багаторазово використовувати
та налаштовувати.

Концепція CVA базується на функції ``html_cva()`` Twig, де ви визначаєте ``базові``
класи, які завжди повинні бути присутніми, а потім різні ``варіанти`` та відповідні класи:

.. code-block:: html+twig

    {# templates/alert.html.twig #}
    {% set alert = html_cva(
        base='alert ',
        variants={
            color: {
                blue: 'bg-blue',
                red: 'bg-red',
                green: 'bg-green',
            },
            size: {
                sm: 'text-sm',
                md: 'text-md',
                lg: 'text-lg',
            }
        }
    ) %}

    <div class="{{ alert.apply({color, size}, class) }}">
        ...
    </div>

Потім використовуйте варіанти ``color`` і ``size`` для вибору потрібних класів:

.. code-block:: twig

    {# index.html.twig #}
    {{ include('alert.html.twig', {'color': 'blue', 'size': 'md'}) }}
    // class="alert bg-red text-lg"

    {{ include('alert.html.twig', {'color': 'green', 'size': 'sm'}) }}
    // class="alert bg-green text-sm"

    {{ include('alert.html.twig', {'color': 'red', 'class': 'flex items-center justify-center'}) }}
    // class="alert bg-red text-md flex items-center justify-center"

CVA та Tailwind CSS
-------------------

CVA чудово працює з Tailwind CSS. Єдиним недоліком є те, що можуть виникати конфлікти класів.
Щоб " об'єднати" конфліктуючі класи разом і залишити тільки ті, які вам потрібні, скористайтеся фільтром
``tailwind_merge()`` з ``tales-from-a-dev/twig-tailwind-extra`_
з функцією ``html_cva()``:

.. code-block:: terminal

    $ composer require tales-from-a-dev/twig-tailwind-extra

.. code-block:: html+twig

    {% set alert = html_cva(
       // ...
    ) %}

    <div class="{{ alert.apply({color, size}, class)|tailwind_merge }}">
         ...
    </div>

Комбіновані варіанти
--------------------

Ви можете визначати комбіновані варіанти. Комбінований варіант - це варіант, який застосовується,
коли виконуються умови декількох інших варіантів:

.. code-block:: html+twig

    {% set alert = html_cva(
        base='alert',
        variants={
            color: {
                blue: 'bg-blue',
                red: 'bg-red',
                green: 'bg-green',
            },
            size: {
                sm: 'text-sm',
                md: 'text-md',
                lg: 'text-lg',
            }
        },
        compoundVariants=[{
            // if color = red AND size = (md or lg), add the `font-bold` class
            color: ['red'],
            size: ['md', 'lg'],
            class: 'font-bold'
        }]
    ) %}

    <div class="{{ alert.apply({color, size}) }}">
         ...
    </div>

    {# index.html.twig #}

    {{ include('alert.html.twig', {color: 'red', size: 'lg'}) }}
    // class="alert bg-red text-lg font-bold"

    {{ include('alert.html.twig', {color: 'green', size: 'sm'}) }}
    // class="alert bg-green text-sm"

    {{ include('alert.html.twig', {color: 'red', size: 'md'}) }}
    // class="alert bg-green text-lg font-bold"

Варіанти за замовчуванням
-------------------------

Якщо жоден з варіантів не співпадає, ви можете визначити набір класів за замовчуванням,
який буде застосовано:

.. code-block:: html+twig

    {% set alert = html_cva(
        base='alert ',
        variants={
            color: {
                blue: 'bg-blue',
                red: 'bg-red',
                green: 'bg-green',
            },
            size: {
                sm: 'text-sm',
                md: 'text-md',
                lg: 'text-lg',
            },
            rounded: {
                sm: 'rounded-sm',
                md: 'rounded-md',
                lg: 'rounded-lg',
            }
        },
        defaultVariants={
            rounded: 'md',
        }
    ) %}

    <div class="{{ alert.apply({color, size}) }}">
         ...
    </div>

    {# index.html.twig #}

    {{ include('alert.html.twig', {color: 'red', size: 'lg'}) }}
    // class="alert bg-red text-lg font-bold rounded-md"

.. note::

    Функція ``html_cva`` є частиною ``HtmlExtension``, яке не
    встановлено за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/html-extra

    Потім, у проєктах Symfony, встановіть ``twig/extra-bundle``:

    .. code-block:: bash

            $ composer require twig/extra-bundle

    В інших випадках, додайте розширення явно у середовищі Twig::

            use Twig\Extra\Html\HtmlExtension;

            $twig = new \Twig\Environment(...);
            $twig->addExtension(new HtmlExtension());

Ця функція найкраще працює при використанні з `TwigComponent`_.

.. _`CVA (Class Variant Authority)`: https://cva.style/docs/getting-started/variants
.. _`shadcn/ui`: https://ui.shadcn.com
.. _`tales-from-a-dev/twig-tailwind-extra`: https://github.com/tales-from-a-dev/twig-tailwind-extra
.. _`TwigComponent`: https://symfony.com/bundles/ux-twig-component/current/index.html
