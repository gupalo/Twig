Расширение Twig
===============

Twig можно расширять многими способами: вы можете добавлять дополнительные теги, фильтры, тесты,
операторы, глобальные переменные и функции. Вы даже можете расширить сам парсер
с помощью посетителей узлов.

.. note::

    В первом разделе этой главы описано, как расширить Twig. Если вы хотите
    повторно использовать ваши изменения в разных проектах или если вы хотите
    поделиться ими с другими, вам следует создать расширение, как описано в
    следующем разделе.

.. caution::

    При расширении Twig без создания расширения, Twig не сможет повторно
    скомпилировать ваши шаблоны при обновлении PHP-кода. Чтобы видеть ваши
    изменения в реальном времени, отключите кеширование шаблонов или упакуйте
    ваш код в расширение (см. следующий раздел этой главы).

Прежде чем расширять Twig, вы должны понимать отличия между всеми
различными возможными точками расширения и когда их использовать.

Во-первых, помните, что Twig имеет две основные языковые конструкции:

* ``{{ }}``: используется для вывода результата вычисления выражения;

* ``{% %}``: используется для выполнения утверждений.

Чтобы понять, почему Twig выставляет так много точек расширения, давайте посмотрим, как
реализовать генератор *Lorem ipsum* (ему нужно знать количество слов 
для генерации).

Вы можете использовать *тег* ``lipsum``:

.. code-block:: twig

    {% lipsum 40 %}

Это работает, но использование тега для ``lipsum`` не является хорошей идеей по меньшей мере по
по трем основным причинам:

* ``lipsum`` не является языковой конвтрукцией;
* Тег что-то выводит;
* Тег не является гибким, так как вы не можете использовать его в выражении:

  .. code-block:: twig

      {{ 'some text' ~ {% lipsum 40 %} ~ 'some more text' }}

На самом деле, вам редко придется создавать теги; и это хорошая новость, потому что теги - это
самая сложная точка расширения.

Теперь давайте воспользуемся *фильтром* ``lipsum``:

.. code-block:: twig

    {{ 40|lipsum }}

Опять же, это работает. Но фильтр должен преобразовать переданное значение во что-то
другое. Здесь мы используем значение для указания количества слов, которые нужно сгенерировать (то есть,
``40`` - это аргумент фильтра, а не значение, которое мы хотим преобразовать).

Далее давайте используем *функцию* ``lipsum``:

.. code-block:: twig

    {{ lipsum(40) }}

Итак, давайте начнем. В этом конкретном примере создание функции является точкой расширения,
которую следует использовать. И вы можете использовать ее везде, где принимается выражение:

.. code-block:: twig

    {{ 'some text' ~ lipsum(40) ~ 'some more text' }}

    {% set lipsum = lipsum(40) %}

Наконец, вы также можете использовать *глобальный* объект с методом, способным генерировать текст
lorem ipsum:

.. code-block:: twig

    {{ text.lipsum(40) }}

Как общее правило, используйте функции для часто используемых возможностей, а глобальные
объекты для всего остального.

Когда вы хотите расширить Twig, имейте в виду следующее:

========== ========================== ========== =========================
Что?       Сложность реализации?      Как часто? Когда?
========== ========================== ========== =========================
*macro*    просто                     часто      Генерирование содержания
*global*   просто                     часто      Объект-помощник
*function* просто                     часто      Генерирование содержания
*filter*   просто                     часто      Преобразование значения
*tag*      сложно                     редко      Языковая конструкция DSL
*test*     просто                     редко      Булевое решение
*operator* просто                     редко      Преобразование значения
========== ========================== ========== =========================

Глобалы
-------

Глобальная переменная похожа на любую другую переменную шаблона, за исключением того, что она
доступна во всех шаблонах и макросах::

    $twig = new \Twig\Environment($loader);
    $twig->addGlobal('text', new Text());

Затем вы можете использовать переменную ``text`` в любом месте шаблона:

.. code-block:: twig

    {{ text.lipsum(40) }}

Фильтры
-------

Создание фильтра заключается в связывании имени с PHP-вызываемым::

    // анонимная функция
    $filter = new \Twig\TwigFilter('rot13', function ($string) {
        return str_rot13($string);
    });

    // или простая PHP-функция
    $filter = new \Twig\TwigFilter('rot13', 'str_rot13');

    // или статический метод класса
    $filter = new \Twig\TwigFilter('rot13', ['SomeClass', 'rot13Filter']);
    $filter = new \Twig\TwigFilter('rot13', 'SomeClass::rot13Filter');

    // или метод класса
    $filter = new \Twig\TwigFilter('rot13', [$this, 'rot13Filter']);
    // приведенный ниже требует реализации во время выполнения (см. ниже для получения дополнительной информации)
    $filter = new \Twig\TwigFilter('rot13', ['SomeClass', 'rot13Filter']);

Первый аргумент, передаваемый конструктору ``\Twig\TwigFilter`` - это название фильтра, который вы
будете использовать в шаблонах, а второй - это PHP-вызываемое для связи с ним.

Затем добавьте фильтр в окружение Twig::

    $twig = new \Twig\Environment($loader);
    $twig->addFilter($filter);

А вот пример, как использовать его в шаблоне:

.. code-block:: twig

    {{ 'Twig'|rot13 }}

    {# выведет Gjvt #}

При вызове Twig, PHP-вызываемое получает левую часть фильтра (перед символом ``|``) в качестве
первого аргумента и дополнительные аргументы, переданные фильтру (в скобках ``()``) как дополнительные аргументы.

Например, следующий код:

.. code-block:: twig

    {{ 'TWIG'|lower }}
    {{ now|date('d/m/Y') }}

компилируется примерно так::

    <?php echo strtolower('TWIG') ?>
    <?php echo twig_date_format_filter($now, 'd/m/Y') ?>

Класс ``\Twig\TwigFilter`` берет массив опций в качестве своего последнего аргумента::

    $filter = new \Twig\TwigFilter('rot13', 'str_rot13', $options);

Фильтры, учитывающие набор символов
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Если вы хотите получить доступ к набору кодировок по умолчанию в вашем фильтре, установите опцию
``needs_charset`` в значение ``true``; Twig передаст набор символов по умолчанию
в качестве первого аргумента вызова фильтра::

    $filter = new \Twig\TwigFilter('rot13', function (string $charset, $string) {
        return str_rot13($string);
    }, ['needs_charset' => true]);

Фильтры, учитывающие окружение
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Если вы хотите получить доступ к текущему экземпляру окружения в вашем фильтре, установите
опцию ``needs_environment`` в значение ``true``; Twig передаст текуще окружение в качестве
первого аргумента вызова фильтра::

    $filter = new \Twig\TwigFilter('rot13', function (\Twig\Environment $env, $string) {
        // получить текущий набор символов для экземпляра
        $charset = $env->getCharset();

        return str_rot13($string);
    }, ['needs_environment' => true]);

Фильтры, учитывающие контекст
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Если вы хотите получить доступ к текущему контексту в вашем фильтре, установите опцию
``needs_context`` в значение ``true``; Twig передаст текущий контекст как
первый аргумент вызова фильтра (или второй, если ``needs_environment`` также установлено
в значение ``true``)::

    $filter = new \Twig\TwigFilter('rot13', function ($context, $string) {
        // ...
    }, ['needs_context' => true]);

    $filter = new \Twig\TwigFilter('rot13', function (\Twig\Environment $env, $context, $string) {
        // ...
    }, ['needs_context' => true, 'needs_environment' => true]);

Автоматическое экранирование
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Если включено автоматическое экранирование, вывод фильтра может быть экранирован
перед отображением. Если ваш фильтр действует как экранировщик (или явно выводит HTML
или JavaScript код), вы хотите, чтобы отображался чистый вывод. В таком случае
установите опцию ``is_safe``::

    $filter = new \Twig\TwigFilter('nl2br', 'nl2br', ['is_safe' => ['html']]);

Некоторым фильтрам может потребоваться работать с вводом, который уже был экранирован или является безопасным,
например, при добавлении (безопасных) HTML-тегов к изначально небезопасному выводу. В таком случае
случае установите опцию ``pre_escape``, чтобы экранировать данные ввода перед тем, как они будут
пропущены через ваш фильтр ::

    $filter = new \Twig\TwigFilter('somefilter', 'somefilter', ['pre_escape' => 'html', 'is_safe' => ['html']]);

Вариативные фильтры
~~~~~~~~~~~~~~~~~~~

Если фильтр должен принимать произвольное количество аргументов, установите опцию
``is_variadic`` в значение ``true``; Twig передаст дополнительные аргументы как
как последний аргумент вызова фильтра в виде массива::

    $filter = new \Twig\TwigFilter('thumbnail', function ($file, array $options = []) {
        // ...
    }, ['is_variadic' => true]);

Обратите внимание, что :ref:`именованные аргументы <named-arguments>`, переданные в вариативный
фильтр, не могут быть проверены на валидность, поскольку они автоматически попадают в массив
опций.

Динамические фильтры
~~~~~~~~~~~~~~~~~~~~

Имя фильтра, содержащее специальный символ ``*``, является динамическим фильтром, а
часть ``*`` будет совпадать с любой строкой::

    $filter = new \Twig\TwigFilter('*_path', function ($name, $arguments) {
        // ...
    });

Следующие фильтры совпадают с определенным выше динамическим фильтром:

* ``product_path``
* ``category_path``

Динамический фильтр может определять более одной динамической части::

    $filter = new \Twig\TwigFilter('*_path_*', function ($name, $suffix, $arguments) {
        // ...
    });

Фильтр получает все значения динамических частей перед обычными аргументами фильтра,
но после среды и контекста. Например, вызов ``'foo'|a_path_b()`` приведет к
тому, что фильтру будут переданы следующие аргументы: ``('a', 'b', 'foo')``.

Устаревшие фильтры
~~~~~~~~~~~~~~~~~~

Вы можете пометить фильтр как устаревший, установив опцию ``deprecated``
в значение ``true``. Вы также можете указать альтернативный фильтр, который заменит
устаревший, если это имеет смысл::

    $filter = new \Twig\TwigFilter('obsolete', function () {
        // ...
    }, ['deprecated' => true, 'alternative' => 'new_one']);

.. versionadded:: 3.11

    Опция ``deprecating_package`` была представлена в Twig 3.11.

Вы также можете установить опцию ``deprecating_package``, чтобы указать пакет, который
объявляет фильтр устаревшим, а ``deprecated`` можно установить на версию пакета, в которой
фильтр был объявлен устаревшим::

    $filter = new \Twig\TwigFilter('obsolete', function () {
        // ...
    }, ['deprecated' => '1.1', 'deprecating_package' => 'foo/bar']);

Когда фильтр устаревает, Twig выдает сообщение об устаревании во время компиляции
шаблона, который его использует. Более подробная информация приведена в :ref:`deprecation-notices`.

Функции
-------

Функции определяются точно так же, как и фильтры, но вам нужно создать
экземпляр ``\Twig\TwigFunction``::

    $twig = new \Twig\Environment($loader);
    $function = new \Twig\TwigFunction('function_name', function () {
        // ...
    });
    $twig->addFunction($function);

Функции поддерживают те же возможности, что и фильтры, за исключением опций ``pre_escape`` и
``preserves_safety``.

Тесты
-----

Тесты определяются так же, как фильтры и функции, но вам нужно создать экземпляр
``\Twig\TwigTest``::

    $twig = new \Twig\Environment($loader);
    $test = new \Twig\TwigTest('test_name', function () {
        // ...
    });
    $twig->addTest($test);

Тесты позволяют вам создавать собственную логику, специфичную для приложений, для оценки булевых 
условий. В качестве простого примера давайте создадим Twig-тест, который проверяет, есть ли объекты 'red'::

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

Функции тестов должны всегда возвращать ``true``/`false``.

При создании тестов вы можете использовать опцию ``node_class``, чтобы предоставить 
пользовательскую компиляцию теста. Это полезно, если ваш тест может быть скомпилирован 
в виде примитивов PHP. Это используется во многих тестах, встроенных в Twig::

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

В приведенном выше примере показано, как создавать тесты, использующие класс узла.
Класс узла имеет доступ к одному подузлу с названием ``node``. Этот подузел содержит значение,
которое тестируется. Когда фильтр ``odd`` используется в коде типа:

.. code-block:: twig

    {% if my_value is odd %}

Подузел ``node`` будет содержать выражение ``my_value``. Тесты на основе узлов также имеют доступ к
к узлу ``arguments``. Этот узел будет содержать различные другие аргументы, которые были предоставлены 
вашему тесту.

Если вы хотите передать переменное количество позиционных или именованных аргументов в
тесту, установите опцию ``is_variadic`` в значение ``true``. Тесты поддерживают динамические
имена (чтобы узнать о синтаксисе, смотрите раздел о динамических фильтрах).

Теги
----

Одной из самых интересных особенностей шаблонизаторов, таких как Twig, является
возможность определения новых **языковых конструкций**. Это также и самая сложная функция
функция, поскольку вам нужно понимать, как работают внутренние механизмы Twig.

Впрочем, в большинстве случаев тег не является необходимым:

* Если ваш тег генерирует некоторый вывод, используйте **функцию** вместо этого.

* Если ваш тег изменяет некоторое содержание и возвращает его, используйте **фильтр** вместо этого.

   Например, если вы хотите создать тег, который преобразует текст в формате Markdown
   в HTML, создайте вместо этого фильтр ``markdown``:

  .. code-block:: twig

      {{ '**markdown** text'|markdown }}

   Если вы хотите использовать этот фильтр для больших объемов текста, оберните его тегом
   :doc:`apply <tags/apply>`:

  .. code-block:: twig

      {% apply markdown %}
      Title
      =====

      Намного лучше, чем создание тега, так как вы можете **объединять** фильтры.
      {% endapply %}

* Если ваш тег ничего не выводит, а существует только благодаря побочному эффекту,
  создайте **функцию**, которая   ничего не возвращает, и вызовите ее с помощью тега
  :doc:`do <tags/do>`.

Например, если вы хотите создать тег, который ведет логи текста, создайте вместо этого функцию ``log``
и вызовите ее с помощью тега :doc:`do <tags/do>`:

  .. code-block:: twig

      {% do log('Log some things') %}

Если вы все еще хотите создать тег для новой языковой конструкции, отлично!

Давайте создадим тег ``set``, который позволяет определять простые переменные из шаблона. Этот 
тег можно использовать следующим образом:

.. code-block:: twig

    {% set name = "value" %}

    {{ name }}

    {# должно вывести значение #}

.. note::

    Тег ``set`` является частью расширения Core и поэтому всегда
    доступен. Встроенная версия является несколько более мощной и поддерживает
    несколько назначений по умолчанию.

Для определения нового тега необходимо сделать три шага:

* Определение класса Token Parser (отвечает за анализ кода шаблона);

* Определение класса Node (отвечает за преобразование проанализированного кода в PHP);

* Регистрация тега.

Регистрация нового тега
~~~~~~~~~~~~~~~~~~~~~~~

Добавьте тег, вызвав метод ``addTokenParser`` в экземпляре ``\Twig\Environment``::

    $twig = new \Twig\Environment($loader);
    $twig->addTokenParser(new CustomSetTokenParser());

Определение парсера токена
~~~~~~~~~~~~~~~~~~~~~~~~~~

Теперь давайте посмотрим на реальный код этого класса::

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

Метод ``getTag()`` должен вернуть тег, который мы хотим проанализировать, здесь - ``set``.

Метод ``parse()`` вызывается каждый раз, когда парсер встречает тег ``set``. 
Он должен вернуть экземпляр ``\Twig\Node\Node\Node``, который представляет узел (создание 
вызовов ``CustomSetNode`` описано в следующем разделе).

Процесс анализа упрощен благодаря ряду методов, которые вы можете вызывать
из потока токенов (``$this->parser->getStream()``):

* ``getCurrent()``: Получает текущий токен в потоке.

* ``next()``: Переходит к следующему токену в потоке, *но возвращает старый*.

* ``test($type)``, ``test($value)`` или ``test($type, $value)``: Определяет, имеет ли
  текущий токен определенный тип или значение (или оба). Значение может быть
  массивом из нескольких возможных значений.

* ``expect($type[, $value[, $message]])``: Если текущий токен не имеет заданного
  типа/значения, будет вызвана ошибка синтаксиса. В противном случае, если тип и 
  значение правильные, токен возвращается и поток переходит к следующему токену.

* ``look()``: Просматривает следующий токен, не потребляя его.

Анализ выражений выполняется с помощью вызова метода ``parseExpression()`` так же,
как мы это делали для тега ``set``.

.. tip::

    Чтение существующих классов ``TokenParser`` - лучший способ изучить все
    мелкие детали процесса анализа.

Определение узла
~~~~~~~~~~~~~~~~

Сам класс ``CustomSetNode`` достаточно короткий::

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

Компилятор реализует гибкий интерфейс и предоставляет методы, которые помогают
разработчику создавать красивый и читабельный PHP-код:

* ``subcompile()`: Компилирует узел.

* ``raw()``: Записывает заданную строку как есть.

* ``write()``: Записывает заданную строку с добавлением отступа в начале
  каждой строки.

* ``string()``: Записывает строку в кавычках.

* ``repr()``: Записывает PHP-представление заданного значения (см.
  ``\Twig\Node\ForNode`` для примера использования).

* ``addDebugInfo()``: Добавляет строку оригинального файла шаблона, связанную с
  с текущим узлом в качестве комментария.

* ``indent()``: Делает отступы в сгенерированном коде (см. ``\Twig\Node\BlockNode\BlockNode`` для
  примера использования).

* ``outdent()``: Делает отступы в сгенерированном коде (см. ``\Twig\Node\BlockNode\BlockNode`` для 
  примера использования).

.. _creating_extensions-ru:

Создание расширения
-------------------

Основной мотивацией для написания расширения является перенос часто используемого кода в класс
многократного использования, например, для добавления поддержки интернационализации. Расширение может
определять теги, фильтры, тесты, операторы, функции и посетителей узлов.

Чаще всего полезно создать одно расширение для вашего проекта,
чтобы разместить в нем все специфические теги и фильтры, которые вы хотите добавить в Twig.

.. tip::

    Когда вы упаковываете свой код в расширения, Twig достаточно умен, чтобы
    перекомпилировать ваши шаблоны каждый раз, когда вы вносите в них изменения (если 
    включена ``auto_reload``).

Расширение - это класс, который реализует следующий интерфейс::

    interface \Twig\Extension\ExtensionInterface
    {
        /**
         * Возвращает экземпляры парсера токенов для добавления к существующему списку.
         *
         * @return \Twig\TokenParser\TokenParserInterface[]
         */
        public function getTokenParsers();

        /**
         * Возвращает экземпляры посетителей узла для добавления к существующему списку.
         *
         * @return \Twig\NodeVisitor\NodeVisitorInterface[]
         */
        public function getNodeVisitors();

        /**
         * Возвращает список фильтров для добавления к существующему списку.
         *
         * @return \Twig\TwigFilter[]
         */
        public function getFilters();

        /**
         * Возвращает список тестов для добавления к существующему списку.
         *
         * @return \Twig\TwigTest[]
         */
        public function getTests();

        /**
         * Возвращает список функций для добавления к существующему списку.
         *
         * @return \Twig\TwigFunction[]
         */
        public function getFunctions();

        /**
         * Возвращает список операторов для добавления к существующему списку.
         *
         * @return array<array> First array of unary operators, second array of binary operators
         */
        public function getOperators();
    }

Чтобы сохранить класс расширения чистым и компактным, наследуйте от встроенного класса ``\Twig\Extension\AbstractExtension`` вместо того, чтобы реализовывать интерфейс, так как 
он предоставляет пустые реализации для всех методов::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
    }

На данный момент это расширение ничего не делает. Мы настроим его в следующих разделах.

Вы можете сохранить свое расширение где угодно в файловой системе, поскольку все расширения должны быть
явно зарегистрированы, чтобы быть доступными в ваших шаблонах.

Вы можете зарегистрировать расширение с помощью метода ``addExtension()`` в вашем
главном объекте ``Environment`` ::

    $twig = new \Twig\Environment($loader);
    $twig->addExtension(new CustomTwigExtension());

.. tip::

    Основные расширения Twig являются отличным примером того, как работают расширения.

Глобалы
~~~~~~~

Глобальные переменные могут быть зарегистрированы в расширении с помощью метода ``getGlobals()``::

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

Фукнции
~~~~~~~

Функции могут быть зарегистрированы в расширении с помощью метода ``getFunctions()``::

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

Фильтры
~~~~~~~

Чтобы добавить фильтр в расширение, вам нужно переопределить метод ``getFilters()``. 
Этот метод должен возвращать массив фильтров для добавления в окружение Twig::

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

Добавление тега в расширение может быть выполнено путем переопределения метода
``getTokenParsers()``. Этот метод должен возвращать массив тегов для добавления
в окружение Twig::

    class CustomTwigExtension extends \Twig\Extension\AbstractExtension
    {
        public function getTokenParsers()
        {
            return [new CustomSetTokenParser()];
        }

        // ...
    }

В вышеприведенном коде мы добавили один новый тег, определенный классом ``CustomSetTokenParser``.
Класс ``CustomSetTokenParser`` отвечает за анализ тега и его компиляцию в PHP.

Операторы
~~~~~~~~~

Метод ``getOperators()`` позволяет вам добавлять новые операторы. Вот как добавить
операторы ``!``, ``||`` и ``&&``::

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

Тесты
~~~~~

Метод ``getTests()`` позволяет вам добавлять новые функции тестов::

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

Определение vs Выполнение
~~~~~~~~~~~~~~~~~~~~~~~~~

Реализации фильтров, функций и тестов Twig во время выполнения могут быть определены как
любое валидное вызываемое PHP:

* **функции/статические методы**: Простые в реализации и быстрые (используются всеми
  расширениями ядра Twig); но для времени выполнения трудно зависеть от внешних
  объектов;

* **замыкания**: Простые в реализации;

* **методы объектов**: Более гибкие и необходимые, если ваш код выполнения зависит от внешних объектов.

Самый простой способ использовать методы - определить их в самом расширении::

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

Это очень удобно, но не рекомендуется, поскольку это заставляет компиляцию шаблонов
зависеть от зависимостей времени выполнения, даже если они не нужны (подумайте,
например, как про зависимость, соединяющая шаблон с движком базы данных).

Вы можете отделить определения расширений от их реализаций во время выполнения,
зарегистрировав экземпляр ``\Twig\RuntimeLoader\RuntimeLoaderInterface`` в окружении,
которое знает, как создавать экземпляры таких классов времени выполнения (классы времени выполнения
должны быть автозагружаемыми)::

    class RuntimeLoader implements \Twig\RuntimeLoader\RuntimeLoaderInterface
    {
        public function load($class)
        {
            // релизовать логику для создания экземпляра $class
            // и внедрить его зависимости
            // в большинстве случаев это означает использование вашего контейнера внедрения зависимостей
            if ('CustomRuntimeExtension' === $class) {
                return new $class(new Rot13Provider());
            } else {
                // ...
            }
        }
    }

    $twig->addRuntimeLoader(new RuntimeLoader());

.. note::

    Twig поставляется с PSR-11-совместимым загрузчиком времени выполнения
    (``\Twig\RuntimeLoader\ContainerRuntimeLoader``).

Теперь есть возможность перенести логику выполнения в новый класс
``CustomRuntimeExtension`` и использовать его непосредственно в расширении::

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

Тестирование расширения
-----------------------

Функциональные тесты
~~~~~~~~~~~~~~~~~~~~

Вы можете создавать функциональные тесты для расширений, создав следующую структуру файлов
в вашем каталоге тестов::

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

Файл ``IntegrationTest.php`` должен выглядеть так::

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

Примеры фикстур можно найти в хранилище Twig `tests/Twig/Fixtures`_.

Узловые тесты
~~~~~~~~~~~~~

Тестирование посетителей узла может быть сложным, поэтому расширяйте ваши тестовые кейсы с
``\Twig\Test\NodeTestCase``. Примеры можно найти в каталоге хранилища Twig `tests/Twig/Node`_.

.. _`tests/Twig/Fixtures`: https://github.com/twigphp/Twig/tree/3.x/tests/Fixtures
.. _`tests/Twig/Node`:     https://github.com/twigphp/Twig/tree/3.x/tests/Node
