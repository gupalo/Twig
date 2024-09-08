``template_from_string``
========================

Функція ``template_from_string`` завантажує шаблон з рядка:

.. code-block:: twig

    {{ include(template_from_string("Hello {{ name }}")) }}
    {{ include(template_from_string(page.template)) }}

Для полегшення налагодження ви також можете дати шаблону ім'я, яке буде частиною
будь-якого пов'язаного з ним повідомлення про помилку:

.. code-block:: twig

    {{ include(template_from_string(page.template, "template for page " ~ page.name)) }}

.. note::

    Функція ``template_from_string`` недоступна за замовчуванням.

    У проєктах на Symfony вам потрібно завантажити її у ваш файл ``services.yaml``:

    .. code-block:: yaml

        services:
            Twig\Extension\StringLoaderExtension:

    або файл ``services.php``::

        $services->set(\Twig\Extension\StringLoaderExtension::class);

    В іншому випадку, додайте розширення явно у середовище Twig::

        use Twig\Extension\StringLoaderExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringLoaderExtension());

.. note::

    Навіть якщо ви, ймовірно, завжди будете використовувати функцію ``template_from_string``
    з функцією ``include``, ви можете використовувати її з будь-яким тегом або функцією, яка
    приймає шаблон як аргумент (наприклад, теги ``embed`` або ``extends``).

Аргументи
---------

* ``template``: Шаблон
* ``name``: Імʼя для шаблону
