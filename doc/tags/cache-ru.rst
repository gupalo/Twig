``cache``
=========

.. versionadded:: 3.2

    Тег ``cache`` был представлен в Twig 3.2.

Тег ``cache`` указывает Twig кешировать фрагмент шаблона:

.. code-block:: twig

    {% cache "cache key" %}
        Cached forever (depending on the cache implementation)
    {% endcache %}

Если вы хотите, чтобы кеш завершал свою работу через определенный промежуток времени, укажите значение
в секундах с помощью модификатора ``ttl()``:

.. code-block:: twig

    {% cache "cache key" ttl(300) %}
        Cached for 300 seconds
    {% endcache %}

Ключом кеша может быть любая строка, которая не использует следующие зарезервированные
символы ``{}()/\@:``; хорошей практикой является встраивание некоторой полезной информации в ключ, которая позволяет кешу автоматически завершать свою работу, когда он должен быть
обновленным:

* Дайте каждому кешу уникальное имя и разместите его в пространстве имен, как и ваши шаблоны;

* Встройте целое число, которое будет увеличиваться каждый раз, когда код шаблона изменяется (для того, 
  чтобы автоматически инвалидировать все текущие кеши);

* Встройте уникальный ключ, который обновляется каждый раз, когда изменяются переменные, используемые
  в коде шаблона.

Например, я бы использовал ``{% cache "blog_post;v1;" ~ post.id ~ ";" ~
post.updated_at %}``` для кеширования фрагмента шаблона содержания блога, где ``blog_post`` описывает
фрагмент шаблона, ``v1`` представляет первую версию кода шаблона, ``post.id`` - идентификатор
записи в блоге, а ``post.updated_at`` возвращает метку времени, которая представляет время, когда
в последний раз была изменена запись в блоге.

Использование такой стратегии именования ключей кеша позволяет избежать использования ``ttl``.
Это все равно, что использовать стратегию "валидации" вместо стратегии "истечения срока действия", как
мы делаем для HTTP-кешей.

Если ваша реализация кеша поддерживает теги, вы также можете тегировать элементы кеша:

.. code-block:: twig

    {% cache "cache key" tags('blog') %}
        Some code
    {% endcache %}

    {% cache "cache key" tags(['cms', 'blog']) %}
        Some code
    {% endcache %}

Тег ``cache`` создает новую "область действия" для переменных, что означает, что изменения
происходят локально во фрагменте шаблона:

.. code-block:: twig

    {% set count = 1 %}

    {% cache "cache key" tags('blog') %}
        {# Не повлияет на значение счетчика за пределами тега кеша #}
        {% set count = 2 %}
        Some code
    {% endcache %}

    {# Отображает 1 #}
    {{ count }}

.. note::

    Тег ``cache`` является частью ``CacheExtension``, которое не установлено
    по умолчанию. Сначала установите его:

    .. code-block:: bash

        $ composer require twig/cache-extra

    В проектах Symfony вы можете автоматически включить это, установив
    ``twig/extra-bundle``:

    .. code-block:: bash

        $ composer require twig/extra-bundle
    
    Или добавьте расширение явно в окружение Twig::

        use Twig\Extra\Cache\CacheExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CacheExtension());

    Если вы не используете Symfony, вы также должны зарегистрировать расширение выполнения::

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
