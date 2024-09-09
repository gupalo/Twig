``html_cva``
============

.. versionadded:: 3.12

    Функция ``html_cva`` была представлена в Twig 3.12.

`CVA (Class Variant Authority)`_ - это концепция из мира JavaScript, которая используется 
известной библиотекой `shadcn/ui`_. Концепция CVA используется для отображения нескольких
вариантов компонентов, применяя набор условий и рецептов для динамического составления строк
классов CSS (цвет, размер и т.д.) для создания шаблонов, которые можно многократно использовать
и настраивать.

Концепция CVA базируется на функции ``html_cva()`` Twig, где вы определяете ``базовые``
классы, которые всегда должны присутствовать, а затем различные ``варианты`` и соответствующие классы:

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

Затем используйте варианты ``color`` и ``size`` для выбора нужных классов:

.. code-block:: twig

    {# index.html.twig #}
    {{ include('alert.html.twig', {'color': 'blue', 'size': 'md'}) }}
    // class="alert bg-red text-lg"

    {{ include('alert.html.twig', {'color': 'green', 'size': 'sm'}) }}
    // class="alert bg-green text-sm"

    {{ include('alert.html.twig', {'color': 'red', 'class': 'flex items-center justify-center'}) }}
    // class="alert bg-red text-md flex items-center justify-center"

CVA и Tailwind CSS
------------------

CVA отлично работает с Tailwind CSS. Единственным недостатком является то, что могут возникать
конфликты классов. Чтобы "объединить" конфликтующие классы вместе и оставить только те, которые 
вам нужны, воспользуйтесь фильтром ``tailwind_merge()`` из ``tales-from-a-dev/twig-tailwind-extra``
с функцией ``html_cva()``:

.. code-block:: terminal

    $ composer require tales-from-a-dev/twig-tailwind-extra

.. code-block:: html+twig

    {% set alert = html_cva(
       // ...
    ) %}

    <div class="{{ alert.apply({color, size}, class)|tailwind_merge }}">
         ...
    </div>

Комбинированные варианты
------------------------

Вы можете определить комбинированные варианты. Комбинированный вариант - это вариант, который применяется,
когда выполняются условия нескольких других вариантов:

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
            // если color = red И size = (md or lg), добавить класс `font-bold`
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

Варианты по умолчанию
---------------------

Если ни один из вариантов не подходит, вы можете определить набор классов по умолчанию,
который будет применен:

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

    Функция ``html_cva`` является частью ``HtmlExtension``, которое не
    установлено по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/html-extra

    Затем, в проектах Symfony, установите ``twig/extra-bundle``:

    .. code-block:: bash

            $ composer require twig/extra-bundle

    В других случаях, добавьте расширение явно в окружение Twig::

            use Twig\Extra\Html\HtmlExtension;

            $twig = new \Twig\Environment(...);
            $twig->addExtension(new HtmlExtension());

Эта функция лучше всего работает при использовании с `TwigComponent`_.

.. _`CVA (Class Variant Authority)`: https://cva.style/docs/getting-started/variants
.. _`shadcn/ui`: https://ui.shadcn.com
.. _`tales-from-a-dev/twig-tailwind-extra`: https://github.com/tales-from-a-dev/twig-tailwind-extra
.. _`TwigComponent`: https://symfony.com/bundles/ux-twig-component/current/index.html
