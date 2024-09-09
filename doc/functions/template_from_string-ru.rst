``template_from_string``
========================

Функция ``template_from_string`` загружает шаблон из строки:

.. code-block:: twig

    {{ include(template_from_string("Hello {{ name }}")) }}
    {{ include(template_from_string(page.template)) }}

Для облегчения отладки вы также можете дать шаблону имя, которое будет частью
любого связанного с ним сообщения об ошибке:

.. code-block:: twig

    {{ include(template_from_string(page.template, "template for page " ~ page.name)) }}

.. note::

    Функция ``template_from_string`` недоступна по умолчанию.

    В проектах на Symfony вам нужно загрузить ее в ваш файл ``services.yaml``:

    .. code-block:: yaml

        services:
            Twig\Extension\StringLoaderExtension:

    или файл ``services.php``::

        $services->set(\Twig\Extension\StringLoaderExtension::class);

    В противном случае, добавьте расширение явно в окружение Twig::

        use Twig\Extension\StringLoaderExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringLoaderExtension());

.. note::

    Даже если вы, вероятно, всегда будете использовать функцию ``template_from_string``
    с функцией ``include``, вы можете использовать ее с любым тегом или функцией, которая
    принимает шаблон в качестве аргумента (например, теги ``embed`` или ``extends``).

Аргументы
---------

* ``template``: Шаблон
* ``name``: Имя для шаблона
