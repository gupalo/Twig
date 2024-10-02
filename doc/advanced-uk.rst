Розширення Twig
===============

Twig можна розширювати багатьма способами: ви можете додавати додаткові теги, фільтри, тести,
оператори, глобальні змінні та функції. Ви навіть можете розширити сам парсер
за допомогою відвідувачів вузлів.

.. note::

    У першому розділі цієї глави описано, як розширити Twig. Якщо ви хочете
    повторно використовувати ваші зміни у різних проєктах або якщо ви хочете поділитися ними
    з іншими, вам слід створити розширення, як описано у
    наступному розділі.

.. caution::

    При розширенні Twig без створення розширення, Twig не зможе
    повторно скомпілювати ваші шаблони при оновленні PHP-коду. Щоб бачити ваші зміни
    в реальному часі, вимкніть кешування шаблонів або упакуйте ваш код в
    розширення (див. наступний розділ цієї глави).

Перш ніж розширювати Twig, ви повинні розуміти відмінності між усіма
різними можливими точками розширення і коли їх використовувати.

По-перше, пам'ятайте, що Twig має дві основні мовні конструкції:

* ``{{ }}``: використовується для виведення результату обчислення виразу;

* ``{% %}``: використовується для виконання стверджень.

Щоб зрозуміти, чому Twig виставляє так багато точок розширення, давайте подивимося, як
реалізувати генератор *Lorem ipsum* (йому потрібно знати кількість слів для
для генерування).

Ви можете використати *тег* ``lipsum``:

.. code-block:: twig

    {% lipsum 40 %}

Це працює, але використання тегу для ``lipsum`` не є гарною ідеєю щонайменше з
з трьох основних причин:

* ``lipsum`` не є мовною конвтрукцією;
* Тег щось виводить;
* Тег не є гнучким, так як ви не можете використовувати його у виразі:

  .. code-block:: twig

      {{ 'some text' ~ {% lipsum 40 %} ~ 'some more text' }}

Насправді, вам рідко доведеться створювати теги; і це хороша новина, тому що теги - це
найскладніша точка розширення.

Тепер давайте скористаємося *фільтром* ``lipsum``:

.. code-block:: twig

    {{ 40|lipsum }}

Знову ж таки, це працює. Але фільтр повинен перетворити передане значення на щось
інше. Тут ми використовуємо значення для вказання кількості слів, які потрібно згенерувати (тобто,
``40`` - це аргумент фільтра, а не значення, яке ми хочемо перетворити).

Далі давайте використаємо *функцію* ``lipsum``:

.. code-block:: twig

    {{ lipsum(40) }}

Отже, почнемо. У цьому конкретному прикладі створення функції є точкою розширення,
яку слід використовувати. І ви можете використовувати її скрізь, де приймається вираз:

.. code-block:: twig

    {{ 'some text' ~ lipsum(40) ~ 'some more text' }}

    {% set lipsum = lipsum(40) %}

Нарешті, ви також можете використовувати *глобальний* об'єкт з методом, здатним генерувати текст
lorem ipsum:

.. code-block:: twig

    {{ text.lipsum(40) }}

Як загальне правило, використовуйте функції для часто використовуваних можливостей, а глобальні
об'єкти для всього іншого.

Коли ви хочете розширити Twig, майте на увазі наступне:

========== ========================== ========== =========================
Що?        Складніть реалізації?      Як часто?  Коли?
========== ========================== ========== =========================
*macro*    просто                     часто      Генерування змісту
*global*   просто                     часто      Обʼєкт-помічник
*function* просто                     часто      Генерування змісту
*filter*   просто                     часто      Перетворення значення
*tag*      складно                    рідко      Мовна конструкція DSL
*test*     просто                     рідко      Булеве рішення
*operator* просто                     рідко      Перетворення значення
========== ========================== ========== =========================

Глобали
-------

Глобальна змінна схожа на будь-яку іншу змінну шаблону, за винятком того, що вона
доступна у всіх шаблонах і макросах::

    $twig = new \Twig\Environment($loader);
    $twig->addGlobal('text', new Text());

Потім ви можете використовувати змінну ``text`` будь-де у шаблоні:

.. code-block:: twig

    {{ text.lipsum(40) }}

Фільтри
-------

Створення фільтра полягає у зв'язуванні імені з PHP-викличним::

    // анонімна функція
    $filter = new \Twig\TwigFilter('rot13', function ($string) {
        return str_rot13($string);
    });

    // або проста PHP-функція
    $filter = new \Twig\TwigFilter('rot13', 'str_rot13');

    // або статичний метод класу
    $filter = new \Twig\TwigFilter('rot13', ['SomeClass', 'rot13Filter']);
    $filter = new \Twig\TwigFilter('rot13', 'SomeClass::rot13Filter');

    // або метод класу
    $filter = new \Twig\TwigFilter('rot13', [$this, 'rot13Filter']);
    // наведений нижче потребує реалізації під час виконання (див. нижче для отримання додаткової інформації)
    $filter = new \Twig\TwigFilter('rot13', ['SomeClass', 'rot13Filter']);

Перший аргумент, що передається конструктору ``\Twig\TwigFilter`` - це назва фільтра, який ви
будете використовувати у шаблонах, а другий - це PHP-викличне для зв'язку з ним.

Потім додайте фільтр до середовища Twig::

    $twig = new \Twig\Environment($loader);
    $twig->addFilter($filter);

А ось приклад, як використовувати його у шаблоні:

.. code-block:: twig

    {{ 'Twig'|rot13 }}

    {# will output Gjvt #}

При виклику Twig, PHP-викличне отримує ліву частину фільтра (перед символом ``|``) в якості
першого аргумента і додаткові аргументи, передані фільтру (в дужках ``()``) як додаткові аргументи.

Наприклад, наступний код:

.. code-block:: twig

    {{ 'TWIG'|lower }}
    {{ now|date('d/m/Y') }}

компілюється приблизно так::

    <?php echo strtolower('TWIG') ?>
    <?php echo twig_date_format_filter($now, 'd/m/Y') ?>

Клас ``\Twig\TwigFilter`` бере масив опцій в якості свого останнього аргумента::

    $filter = new \Twig\TwigFilter('rot13', 'str_rot13', $options);

Фільтри, що враховують набір символів
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Якщо ви хочете отримати доступ до набору кодувань за замовчуванням у вашому фільтрі, встановіть опцію
``needs_charset`` у значення ``true``; Twig передасть набір символів за замовчуванням як
як перший аргумент виклику фільтра::

    $filter = new \Twig\TwigFilter('rot13', function (string $charset, $string) {
        return str_rot13($string);
    }, ['needs_charset' => true]);

Фільтри, що враховують середовище
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Якщо ви хочете отримати доступ до поточного екземпляру середовища у вашому фільтрі, встановіть опцію
``needs_environment`` у значення ``true``; Twig передасть поточне
середовище як перший аргумент виклику фільтра::

    $filter = new \Twig\TwigFilter('rot13', function (\Twig\Environment $env, $string) {
        // get the current charset for instance
        $charset = $env->getCharset();

        return str_rot13($string);
    }, ['needs_environment' => true]);

Фільтри, що враховують контекст
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Якщо ви хочете отримати доступ до поточного контексту у вашому фільтрі, встановіть опцію
``needs_context`` у значення ``true``; Twig передасть поточний контекст як
перший аргумент виклику фільтра (або другий, якщо ``needs_environment`` також встановлено
у значення ``true``)::

    $filter = new \Twig\TwigFilter('rot13', function ($context, $string) {
        // ...
    }, ['needs_context' => true]);

    $filter = new \Twig\TwigFilter('rot13', function (\Twig\Environment $env, $context, $string) {
        // ...
    }, ['needs_context' => true, 'needs_environment' => true]);

Автоматичне екранування
~~~~~~~~~~~~~~~~~~~~~~~

Якщо увімкнено автоматичне екранування, виведення фільтра може бути екрановано
перед відображенням. Якщо ваш фільтр діє як екранувальник (або явно виводить HTML
чи JavaScript код), ви хочете, щоб було відображено чисте виведення. У такому випадку
встановіть опцію ``is_safe``::

    $filter = new \Twig\TwigFilter('nl2br', 'nl2br', ['is_safe' => ['html']]);

Деяким фільтрам може знадобитися працювати з введенням, яке вже було екрановане або є безпечним,
наприклад, при додаванні (безпечних) HTML-тегів до початково небезпечного виведення. У такому
випадку встановіть опцію ``pre_escape``, щоб екранувати дані введення перед тим, як вони будуть
пропущені через ваш фільтр ::

    $filter = new \Twig\TwigFilter('somefilter', 'somefilter', ['pre_escape' => 'html', 'is_safe' => ['html']]);

Варіативні фільтри
~~~~~~~~~~~~~~~~~~

Якщо фільтр має приймати довільну кількість аргументів, встановіть опцію
``is_variadic`` у значення ``true``; Twig передасть додаткові аргументи
як останній аргумент виклику фільтра у вигляді масиву::

    $filter = new \Twig\TwigFilter('thumbnail', function ($file, array $options = []) {
        // ...
    }, ['is_variadic' => true]);

Зверніть увагу, що :ref:`іменовані аргументи <named-arguments>`, передані у варіативний
фільтр, не можуть бути перевірені на валідність, оскільки вони автоматично потрапляють до масиву
опцій.

Динамічні фільтри
~~~~~~~~~~~~~~~~~

Ім'я фільтра, що містить спеціальний символ ``*``, є динамічним фільтром, а
частина ``*`` буде співпадати з будь-яким рядком::

    $filter = new \Twig\TwigFilter('*_path', function ($name, $arguments) {
        // ...
    });

Наступні фільтри співпадають з визначеним вище динамічним фільтром:

* ``product_path``
* ``category_path``

Динамічний фільтр може визначати більше однієї динамічної частини::

    $filter = new \Twig\TwigFilter('*_path_*', function ($name, $suffix, $arguments) {
        // ...
    });

Фільтр отримує всі значення динамічних частин перед звичайними аргументами фільтра,
але після середовища та контексту. Наприклад, виклик ``'foo'|a_path_b()`` призведе до
того, що фільтру буде передано наступні аргументи: ``('a', 'b', 'foo')``.

Застарілі фільтри
~~~~~~~~~~~~~~~~~

Ви можете позначити фільтр як застарілий, встановивши опцію ``deprecated``
у значення ``true``. Ви також можете вказати альтернативний фільтр, який замінить
застарілий, якщо це має сенс::

    $filter = new \Twig\TwigFilter('obsolete', function () {
        // ...
    }, ['deprecated' => true, 'alternative' => 'new_one']);

.. versionadded:: 3.11

    Опція ``deprecating_package`` була представлена в Twig 3.11.

Ви також можете встановити опцію ``deprecating_package``, щоб вказати пакет, який
оголошує фільтр застарілим, а ``deprecated`` можна встановити на версію пакета, в якій
фільтр було оголошено застарілим::

    $filter = new \Twig\TwigFilter('obsolete', function () {
        // ...
    }, ['deprecated' => '1.1', 'deprecating_package' => 'foo/bar']);

Коли фільтр застаріває, Twig видає повідомлення про застарівання під час компіляції
шаблону, який його використовує. Докладнішу інформацію наведено у :ref:`deprecation-notices`.

Функції
-------

Функції визначаються так само, як і фільтри, але вам потрібно створити
екземпляр ``\Twig\TwigFunction``::

    $twig = new \Twig\Environment($loader);
    $function = new \Twig\TwigFunction('function_name', function () {
        // ...
    });
    $twig->addFunction($function);

Функції підтримують ті самі можливості, що й фільтри, за винятком опцій ``pre_escape`` та
``preserves_safety``.

Тести
-----

Тести визначаються так само, як фільтри та функції, але вам потрібно створити екземпляр
``\Twig\TwigTest``::

    $twig = new \Twig\Environment($loader);
    $test = new \Twig\TwigTest('test_name', function () {
        // ...
    });
    $twig->addTest($test);

Тести дозволяють вам створювати власну логіку, специфічну для додатків, для оцінки булевих 
умов. Як простий приклад, давайте створимо Twig-тест, який перевіряє, чи є об'єкти 'red'::

    $twig = new \Twig\Environment($loader);
    $test = new \Twig\TwigTest('red', function ($value) {
        if (isset($value->color) && $value->color == 'red') {
            return true;
        }
        if (isset($value->paint) && $value->paint == 'red') {
            return true;
        }
        return false;
    });
    $twig->addTest($test);

Функції тестів мають завжди повертати ``true``/``false``.

При створенні тестів ви можете використовувати опцію ``node_class``, щоб надати 
користувацьку компіляцію тесту. Це корисно, якщо ваш тест може бути скомпільовано 
у вигляді примітивів PHP. Це використовується у багатьох тестах, вбудованих у Twig::

    namespace App;

    use Twig\Environment;
    use Twig\Node\Expression\TestExpression;
    use Twig\TwigTest;

    $twig = new Environment($loader);
    $test = new TwigTest(
        'odd',
        null,
        ['node_class' => OddTestExpression::class]);
    $twig->addTest($test);

    class OddTestExpression extends TestExpression
    {
        public function compile(\Twig\Compiler $compiler)
        {
            $compiler
                ->raw('(')
                ->subcompile($this->getNode('node'))
                ->raw(' % 2 != 0')
                ->raw(')')
            ;
        }
    }

У наведеному вище прикладі показано, як створювати тести, що використовують клас вузла.
Клас вузла має доступ до одного підвузла з назвою ``node``. Цей підвузол містить значення,
яке тестується. Коли фільтр ``odd`` використовується в коді типу:

.. code-block:: twig

    {% if my_value is odd %}

Підвузол ``node`` міститиме вираз ``my_value``. Тести на основі вузлів також мають доступ
до вузла ``arguments``. Цей вузол буде містити різні інші аргументи, які були надані 
вашому тесту.

Якщо ви хочете передати змінну кількість позиційних або іменованих аргументів до
тесту, встановіть опцію ``is_variadic`` у значення ``true``. Тести підтримують динамічні
імена (щоб дізнатися про синтаксис, дивіться розділ  про динамічні фільтри).

Теги
----

Однією з найцікавіших особливостей шаблонізаторів, таких як Twig, є
можливість визначення нових **мовних конструкцій**. Це також і найскладніша
функція, оскільки вам потрібно розуміти, як працюють внутрішні механізми Twig.

Втім, здебільшого тег не є необхідним:

* Якщо ваш тег генерує деяке виведення, використовуйте **функцію** замість цього.

* Якщо ваш тег змінює деякий зміст і повертає його, використовуйте **фільтр** замість цього.

  Наприклад, якщо ви хочете створити тег, який перетворить текст у форматі Markdown
  на HTML, створіть замість цього фільтр ``markdown``:

  .. code-block:: twig

      {{ '**markdown** text'|markdown }}

  Якщо ви хочете використовувати цей фільтр для великих обсягів тексту, обгорніть його тегом
  :doc:`apply <tags/apply>`:

  .. code-block:: twig

      {% apply markdown %}
      Title
      =====

      Набагато краще, ніж створення тегу, так як ви можете **складати** фільтри.
      {% endapply %}

* Якщо ваш тег нічого не виводить, а існує лише задля побічного
  ефекту, створіть **функцію**, яка нічого не повертає, і викличте її за допомогою тегу
  :doc:`do <tags/do>`.

  Наприклад, якщо ви хочете створити тег, який веде логи тексту, створіть замість цього функцію ``log``
  і викличте її за допомогою тега :doc:`do <tags/do>`:

  .. code-block:: twig

      {% do log('Log some things') %}

Якщо ви все ще хочете створити тег для нової мовної конструкції, чудово!

Давайте створимо тег ``set``, який дозволяє визначати прості змінні з шаблону. Цей 
тег можна використовувати наступним чином:

.. code-block:: twig

    {% set name = "value" %}

    {{ name }}

    {# should output value #}

.. note::

    Тег ``set`` є частиною розширення Core і тому завжди
    доступний. Вбудована версія є дещо потужнішою і підтримує
    декілька призначень за замовчуванням.

Для визначення нового тегу необхідно зробити три кроки:

* Визначення класу Token Parser (відповідає за аналіз коду шаблону);

* Визначення класу Node (відповідає за перетворення проаналізованого коду в PHP);

* Реєстрація тегу.

Реєстрація нового тегу
~~~~~~~~~~~~~~~~~~~~~~

Додайте тег, викликавши метод ``addTokenParser`` в екземплярі ``\Twig\Environment``::

    $twig = new \Twig\Environment($loader);
    $twig->addTokenParser(new CustomSetTokenParser());

Визначення парсера токена
~~~~~~~~~~~~~~~~~~~~~~~~~

Тепер давайте подивимось реальний код цього класу::

    class CustomSetTokenParser extends \Twig\TokenParser\AbstractTokenParser
    {
        public function parse(\Twig\Token $token)
        {
            $parser = $this->parser;
            $stream = $parser->getStream();

            $name = $stream->expect(\Twig\Token::NAME_TYPE)->getValue();
            $stream->expect(\Twig\Token::OPERATOR_TYPE, '=');
            $value = $parser->getExpressionParser()->parseExpression();
            $stream->expect(\Twig\Token::BLOCK_END_TYPE);

            return new CustomSetNode($name, $value, $token->getLine());
        }

        public function getTag()
        {
            return 'set';
        }
    }

Метод ``getTag()`` повинен повернути тег, який ми хочемо проаналізувати, тут - ``set``.

Метод ``parse()`` викликається кожного разу, коли парсер зустрічає тег ``set``. 
Він має повернути екземпляр ``\Twig\Node\Node``, який представляє вузол (створення 
викликів ``CustomSetNode`` описано у наступному розділі).

Процес аналізу спрощено завдяки низці методів, які ви можете викликати
з потоку токенів (``$this->parser->getStream()``):

* ``getCurrent()``: Отримує поточний токен у потоці.

* ``next()``: Переходить до наступного токену в потоці, *але повертає старий*.

* ``test($type)``, ``test($value)`` або ``test($type, $value)``: Визначає, чи має
  поточний токен певний тип або значення (або обидва). Значення може бути
  масивом з декількох можливих значень.

* ``expect($type[, $value[, $message]])``: Якщо поточний токен не має заданого
  типу/значення, буде викликано помилку синтаксису. В іншому випадку, якщо тип і 
  значення правильні, токен повертається і потік переходить до наступного токену.

* ``look()``: Переглядає наступний токен, не споживаючи його.

Аналіз виразів виконується за допомогою виклику методу ``parseExpression()`` так само,
як ми це робили для тегу ``set``.

.. tip::

    Читання існуючих класів ``TokenParser`` - найкращий спосіб вивчити всі
    дрібні деталі процесу аналізу.

Визначення вузла
~~~~~~~~~~~~~~~~

Сам клас ``CustomSetNode`` достатньо короткий::

    class CustomSetNode extends \Twig\Node\Node
    {
        public function __construct($name, \Twig\Node\Expression\AbstractExpression $value, $line)
        {
            parent::__construct(['value' => $value], ['name' => $name], $line);
        }

        public function compile(\Twig\Compiler $compiler)
        {
            $compiler
                ->addDebugInfo($this)
                ->write('$context[\''.$this->getAttribute('name').'\'] = ')
                ->subcompile($this->getNode('value'))
                ->raw(";\n")
            ;
        }
    }

Компілятор реалізує гнучкий інтерфейс і надає методи, які допомагають
розробнику створювати красивий та читабельний PHP-код:

* ``subcompile()``: Компілює вузол.

* ``raw()``: Записує заданий рядок як є.

* ``write()``: Записує заданий рядок з додаванням відступу на початку
кожного рядка.

* ``string()``: Записує рядок у лапках.

* ``repr()``: Записує PHP-представлення заданого значення (див.
  ``\Twig\Node\ForNode`` для прикладу використання).

* ``addDebugInfo()``: Додає рядок оригінального файлу шаблону, пов'язаний з
  з поточним вузлом як коментар.

* ``indent()``: Робить відступи у згенерованому коді (див. ``\Twig\Node\BlockNode`` для
  прикладу використання).

* ``outdent()``: Робить відступи у згенерованому коді (див. ``\Twig\Node\BlockNode`` для 
  прикладу використання).

.. _creating_extensions-uk:

Створення розширення
--------------------

Основною мотивацією для написання розширення є перенесення часто використовуваного коду у клас
багаторазового використання, наприклад, для додавання підтримки інтернаціоналізації. Розширення може
визначати теги, фільтри, тести, оператори, функції та відвідувачів вузлів.

Найчастіше корисно створити одне розширення для вашого проекту,
щоб розмістити в ньому всі специфічні теги і фільтри, які ви хочете додати до Twig.

.. tip::

    Коли ви упаковуєте свій код у розширення, Twig достатньо розумний, щоб
    перекомпілювати ваші шаблони щоразу, коли ви вносите до них зміни (якщо 
    увімкнено ``auto_reload``).

Розширення - це клас, який реалізує наступний інтерфейс::

    interface \Twig\Extension\ExtensionInterface
    {
        /**
         * Повертає екземпляри парсера токенів для додавання до існуючого списку.
         *
         * @return \Twig\TokenParser\TokenParserInterface[]
         */
        public function getTokenParsers();

        /**
         * Повертає екземпляри відвідувачів вузла для додавання до існуючого списку.
         *
         * @return \Twig\NodeVisitor\NodeVisitorInterface[]
         */
        public function getNodeVisitors();

        /**
         * Повертає список фільтрів для додавання до існуючого списку.
         *
         * @return \Twig\TwigFilter[]
         */
        public function getFilters();

        /**
         * Повертає список тестів для додавання до існуючого списку.
         *
         * @return \Twig\TwigTest[]
         */
        public function getTests();

        /**
         * Повертає список фукнцій для додавання до існуючого списку.
         *
         * @return \Twig\TwigFunction[]
         */
        public function getFunctions();

        /**
         * Повертає список операторів для додавання до існуючого списку.
         *
         * @return array<array> First array of unary operators, second array of binary operators
         */
        public function getOperators();
    }

Щоб зберегти клас розширення чистим та компактним, успадковуйте від вбудованого класу ``\Twig\Extension\AbstractExtension`` замість того, щоб реалізовувати інтерфейс так як 
він надає порожні реалізації для всіх методів::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
    }

Наразі це розширення нічого не робить. Ми налаштуємо його у наступних розділах.

Ви можете зберегти своє розширення будь-де у файловій системі, оскільки всі розширення мають бути
явно зареєстровані, щоб бути доступними у ваших шаблонах.

Ви можете зареєструвати розширення за допомогою методу ``addExtension()`` у вашому
головному об'єкті ``Environment`` ::

    $twig = new \Twig\Environment($loader);
    $twig->addExtension(new CustomTwigExtension());

.. tip::

    Основні розширення Twig є чудовим прикладом того, як працюють розширення.

Глобали
~~~~~~~

Глобальні змінні можуть бути зареєстровані в розширенні за допомогою методу ``getGlobals()``::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension implements \Twig\Extension\GlobalsInterface
    {
        public function getGlobals(): array
        {
            return [
                'text' => new Text(),
            ];
        }

        // ...
    }

Фукнції
~~~~~~~

Функції можуть бути зареєстровані у розширенні за допомогою методу ``getFunctions()``::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getFunctions()
        {
            return [
                new \Twig\TwigFunction('lipsum', 'generate_lipsum'),
            ];
        }

        // ...
    }

Фільтри
~~~~~~~

Щоб додати фільтр до розширення, вам потрібно перевизначити метод ``getFilters()``. 
Цей метод повинен повертати масив фільтрів для додавання у середовище  Twig::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getFilters()
        {
            return [
                new \Twig\TwigFilter('rot13', 'str_rot13'),
            ];
        }

        // ...
    }

Теги
~~~~

Додавання тегу у розширення може бути виконано шляхом перевизначення методу
``getTokenParsers()``. Цей метод повинен повертати масив тегів для додавання
до середовища Twig::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getTokenParsers()
        {
            return [new CustomSetTokenParser()];
        }

        // ...
    }

У вищенаведеному коді ми додали один новий тег, визначений класом ``CustomSetTokenParser``.
Клас ``CustomSetTokenParser`` відповідає за аналіз тегу та його компіляцію в PHP.

Оператори
~~~~~~~~~

Метод ``getOperators()`` дозволяє вам додавати нові оператори. Ось як додати
оператори ``!``, ``||`` та ``&&``::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getOperators()
        {
            return [
                [
                    '!' => ['precedence' => 50, 'class' => \Twig\Node\Expression\Unary\NotUnary::class],
                ],
                [
                    '||' => ['precedence' => 10, 'class' => \Twig\Node\Expression\Binary\OrBinary::class, 'associativity' => \Twig\ExpressionParser::OPERATOR_LEFT],
                    '&&' => ['precedence' => 15, 'class' => \Twig\Node\Expression\Binary\AndBinary::class, 'associativity' => \Twig\ExpressionParser::OPERATOR_LEFT],
                ],
            ];
        }

        // ...
    }

Тести
~~~~~

Метод ``getTests()`` дозволяє вам додавати нові функції тестів::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getTests()
        {
            return [
                new \Twig\TwigTest('even', 'twig_test_even'),
            ];
        }

        // ...
    }

Визначення vs Виконання
~~~~~~~~~~~~~~~~~~~~~~~

Реалізації фільтрів, функцій і тестів Twig під час виконання можуть бути визначені як
будь-яке валідне викличне PHP:

* **функції/статичні методи**: Прості у реалізації та швидкі (використовуються всіма
  розширеннями ядра Twig); але для часу виконання важко залежати від зовнішніх
  об'єктів;

* **замикання**: Прості у реалізації;

* **методи об'єктів**: Більш гнучкі та необхідні, якщо ваш код виконання залежить від зовнішніх об'єктів.

Найпростіший спосіб використовувати методи - визначити їх у самому розширенні::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        private $rot13Provider;

        public function __construct($rot13Provider)
        {
            $this->rot13Provider = $rot13Provider;
        }

        public function getFunctions()
        {
            return [
                new \Twig\TwigFunction('rot13', [$this, 'rot13']),
            ];
        }

        public function rot13($value)
        {
            return $this->rot13Provider->rot13($value);
        }
    }

Це дуже зручно, але не рекомендується, оскільки це змушує компіляцію шаблонів
залежати від залежностей часу виконання, навіть якщо вони не потрібні (подумайте,
наприклад, як залежність, що з'єднує шаблон з движком бази даних).

Ви можете відокремити визначення розширень від їхніх реалізацій під час виконання
зареєструвавши екземпляр ``\Twig\RuntimeLoader\RuntimeLoaderInterface`` у середовищі,
яке знає, як створювати екземпляри таких класів часу виконання (класи часу виконання
мають бути автозавантажуваними)::

    class RuntimeLoader implements \Twig\RuntimeLoader\RuntimeLoaderInterface
    {
        public function load($class)
        {
            // реалізувати логіку для створення екземпляру $class
            // та впровадити його залежності
            // в більшості випадків це означає використання вашого контейнера впровадження залежностей
            if ('CustomRuntimeExtension' === $class) {
                return new $class(new Rot13Provider());
            } else {
                // ...
            }
        }
    }

    $twig->addRuntimeLoader(new RuntimeLoader());

.. note::

    Twig постачається з PSR-11-сумісним завантажувачем часу виконання
    (``\Twig\RuntimeLoader\ContainerRuntimeLoader``).

Тепер є можливість перенести логіку виконання у новий клас
``CustomRuntimeExtension`` і використовувати його безпосередньо у розширенні::

    class CustomRuntimeExtension
    {
        private $rot13Provider;

        public function __construct($rot13Provider)
        {
            $this->rot13Provider = $rot13Provider;
        }

        public function rot13($value)
        {
            return $this->rot13Provider->rot13($value);
        }
    }

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getFunctions()
        {
            return [
                new \Twig\TwigFunction('rot13', ['CustomRuntimeExtension', 'rot13']),
                // або
                new \Twig\TwigFunction('rot13', 'CustomRuntimeExtension::rot13'),
            ];
        }
    }

Тестування розширення
---------------------

Функціональні тести
~~~~~~~~~~~~~~~~~~~

Ви можете створювати функціональні тести для розширень, створивши таку структуру файлів
у вашому каталозі тестів::

    Fixtures/
        filters/
            foo.test
            bar.test
        functions/
            foo.test
            bar.test
        tags/
            foo.test
            bar.test
    IntegrationTest.php

Файл ``IntegrationTest.php`` має виглядати так::

    namespace Project\Tests;

    use Twig\Test\IntegrationTestCase;

    class IntegrationTest extends IntegrationTestCase
    {
        public function getExtensions()
        {
            return [
                new CustomTwigExtension1(),
                new CustomTwigExtension2(),
            ];
        }

        public function getFixturesDir()
        {
            return __DIR__.'/Fixtures/';
        }
    }

Приклади фікстур можна знайти у сховищі Twig `tests/Twig/Fixtures`_.

Вузлові тести
~~~~~~~~~~~~~

Тестування відвідувачів вузла може бути складним, тому розширюйте ваші тестові кейси з
``\Twig\Test\NodeTestCase``. Приклади можна знайти у каталогу сховища Twig `tests/Twig/Node`_.

.. _`tests/Twig/Fixtures`: https://github.com/twigphp/Twig/tree/3.x/tests/Fixtures
.. _`tests/Twig/Node`:     https://github.com/twigphp/Twig/tree/3.x/tests/Node
