``cache``
=========

.. versionadded:: 3.2

    Тег ``cache`` був представлений в Twig 3.2.

Тег ``cache`` вказує Twig кешувати фрагмент шаблону:

.. code-block:: twig

    {% cache "cache key" %}
        Cached forever (depending on the cache implementation)
    {% endcache %}

Якщо ви хочете, щоб кеш завершував свою роботу через певний проміжок часу, вкажіть значення
у секундах за допомогою модифікатора ``ttl()``:

.. code-block:: twig

    {% cache "cache key" ttl(300) %}
        Cached for 300 seconds
    {% endcache %}

Ключем кешу може бути будь-який рядок, який не використовує наступні зарезервовані
символи ``{}()/\@:``; гарною практикою є вбудовування деякої корисної інформації у ключ, яка дозволяє кешу автоматично завершувати свою роботу, коли він повинен бути
оновленим:

* Дайте кожному кешу унікальне ім'я і розмістіть його у просторі імен, як і ваші шаблони;

* Вбудуйте ціле число, яке буде збільшуватися щоразу, коли код шаблону змінюється (для того, 
  щоб автоматично інвалідувати всі поточні кеші);

* Вбудуйте унікальний ключ, який оновлюється щоразу, коли змінюються змінні, що використовуються
  в коді шаблону.

Наприклад, я б використовував ``{% cache «blog_post;v1;» ~ post.id ~ «;» ~
post.updated_at %}`` для кешування фрагмента шаблону змісту блогу, де ``blog_post`` описує
фрагмент шаблону, ``v1`` представляє першу версію коду шаблону, ``post.id`` - ідентифікатор
запису в блозі, а ``post.updated_at`` повертає мітку часу, яка представляє час, коли
востаннє було змінено запис у блозі.

Використання такої стратегії іменування ключів кешу дозволяє уникнути використання ``ttl``.
Це все одно, що використовувати стратегію "валідації" замість стратегії "закінчення терміну дії", як
ми робимо для HTTP-кешів.

Якщо ваша реалізація кешу підтримує теги, ви також можете тегувати елементи кешу:

.. code-block:: twig

    {% cache "cache key" tags('blog') %}
        Some code
    {% endcache %}

    {% cache "cache key" tags(['cms', 'blog']) %}
        Some code
    {% endcache %}

Тег ``cache`` створює новий "діапазон" для змінних, що означає, що зміни
відбуваються локально у фрагменті шаблону:

.. code-block:: twig

    {% set count = 1 %}

    {% cache "cache key" tags('blog') %}
        {# Не вплине на значення на значення лічильника за межами тегу кешу #}
        {% set count = 2 %}
        Some code
    {% endcache %}

    {# Відображає 1 #}
    {{ count }}

.. note::

    Тег ``cache`` є частиною ``CacheExtension``, яке не встановлено
    за замовчуванням. Спочатку встановіть його:

    .. code-block:: bash

        $ composer require twig/cache-extra

    У проєктах Symfony ви можете автоматично включити це, встановивши
    ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle

    Або додайте розширення явно у середовище Twig::

        use Twig\Extra\Cache\CacheExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CacheExtension());

   Якщо ви не використовуєте Symfony, ви також повинні зареєструвати розширення виконання::

        use Symfony\Component\Cache\Adapter\FilesystemAdapter;
        use Symfony\Component\Cache\Adapter\TagAwareAdapter;
        use Twig\Extra\Cache\CacheRuntime;
        use Twig\RuntimeLoader\RuntimeLoaderInterface;

        $twig->addRuntimeLoader(new class implements RuntimeLoaderInterface {
            public function load($class) {
                if (CacheRuntime::class === $class) {
                    return new CacheRuntime(new TagAwareAdapter(new FilesystemAdapter()));
                }
            }
        });
