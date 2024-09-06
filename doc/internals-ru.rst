Інтернали Twig
==============

Twig дуже розширюваний, і ви можете його хакнути. Майте на увазі, що вам
варто спробувати створити розширення перед тим, як хакнути ядро, оскільки більшість
функцій та покращень можна реалізувати за допомогою розширень. Цей розділ також може бути
корисним для людей, які хочуть зрозуміти, як працює Twig «за лаштунками».

Як працює Twig?
---------------

Відображення шаблону Twig можна розбити на чотири ключові етапи:

* **Завантажте** шаблон: Якщо шаблон вже скомпільовано, завантажте його та перейдіть
до кроку *оцінювання*, в іншому випадку:

  * Спочатку, **лексер** токенізує вихідний код шаблону на невеликі фрагменти
  для полегшення обробки;

  * Потім **парсер** перетворює потік токенів в осмислене дерево вузлів (Дерево Абстрактного Синтаксису, AST);

  * Нарешті, **компілятор** перетворює AST на PHP-код.

* **Оцініть** шаблон: Це означає виклик методу ``display()`` скомпільованого
  шаблону та передача йому контексту.

Лексер
------

Лексер токенізує вихідний код шаблону у потік токенів (кожен токен є
екземпляром ``\Twig\Token``, а потік - екземпляром ``\Twig\TokenStream``). Стандартний 
лексер розпізнає 15 різних типів токенів:

* ``\Twig\Token::BLOCK_START_TYPE``, ``\Twig\Token::BLOCK_END_TYPE``: Роздільники для блоків (``{% %}``)
* ``\Twig\Token::VAR_START_TYPE``, ``\Twig\Token::VAR_END_TYPE``: Роздільники для змінних (``{{ }}``)
* ``\Twig\Token::TEXT_TYPE``: Текст поза виразом;
* ``\Twig\Token::NAME_TYPE``: Імʼя у виразі;
* ``\Twig\Token::NUMBER_TYPE``: Число у виразі;
* ``\Twig\Token::STRING_TYPE``: Рядок у виразі;
* ``\Twig\Token::OPERATOR_TYPE``: Оператор;
* ``\Twig\Token::ARROW_TYPE``: Оператор стрілочної функції (``=>``);
* ``\Twig\Token::SPREAD_TYPE``: Оператор спреду (``...``);
* ``\Twig\Token::PUNCTUATION_TYPE``: Знак пунктуації;
* ``\Twig\Token::INTERPOLATION_START_TYPE``, ``\Twig\Token::INTERPOLATION_END_TYPE``: Роздільники для інтерполяції рядків;
* ``\Twig\Token::EOF_TYPE``: Закінчення шаблону.

Ви можете вручну перетворити вихідний код на потік токенів, викликавши метод середовища
``tokenize()``::

    $stream = $twig->tokenize(new \Twig\Source($source, $identifier));

Оскільки потік має метод ``__toString()``, ви можете мати текстове
представлення потоку шляхом повторення об'єкту::

    echo $stream."\n";

Ось виведення для шаблону ``Hello {{ name }}``:

.. code-block:: text

    TEXT_TYPE(Hello )
    VAR_START_TYPE()
    NAME_TYPE(name)
    VAR_END_TYPE()
    EOF_TYPE()

.. note::

    Лексер за замовчуванням (``\Twig\Lexer``) може бути змінений шляхом виклику
    методу ``setLexer()``::

        $twig->setLexer($lexer);

Парсер
------

Парсер перетворює потік токенів на AST (дерево абстрактного синтаксису), або дерево вузлів
дерево вузлів (екземпляр ``\Twig\Node\ModuleNode``). Основне розширення визначає
базові вузли, такі як: ``for``, ``if``, ... та вузли виразів.

Ви можете вручну перетворити потік токенів на дерево вузлів, викликавши функцію методу середовища
``parse()``::

    $nodes = $twig->parse($stream);

Повторення об'єкта вузла дає вам гарне представлення дерева::

    echo $nodes."\n";

Ось виведення для шаблону ``Hello {{ name }}``:

.. code-block:: text

    \Twig\Node\ModuleNode(
      \Twig\Node\TextNode(Hello )
      \Twig\Node\PrintNode(
        \Twig\Node\Expression\NameExpression(name)
      )
    )

.. note::

    Парсер за замовчуванням (``\Twig\TokenParser\AbstractTokenParser``) може бути змінено, шляхом
    виклику методу ``setParser()``::

        $twig->setParser($parser);

Компілятор
----------

Останній крок виконується компілятором. Він отримує дерево вузлів в якості введеня та
генерує PHP-код, придатний для використання під час виконання шаблону.

Ви можете вручну скомпілювати дерево вузлів у PHP-код за допомогою методу середовища 
``compile()``::

    $php = $twig->compile($nodes);

Згенерований шаблон для шаблону ``Hello {{ name }}`` виглядає наступним чином
(фактичне виведення може відрізнятися в залежності від версії Twig, яку ви використовуєте)::

    /* Hello {{ name }} */
    class __TwigTemplate_1121b6f109fe93ebe8c6e22e3712bceb extends Template
    {
        protected function doDisplay(array $context, array $blocks = []): iterable
        {
            $macros = $this->macros;
            // рядок 1
            yield "Hello ";
            // рядок 2
            yield $this->env->getRuntime('Twig\Runtime\EscaperRuntime')->escape((isset($context["name"]) || array_key_exists("name", $context) ? $context["name"] : (function () { throw new RuntimeError('Variable "name" does not exist.', 2, $this->source); })()), "html", null, true);
            return; yield '';
        }

        // ще трохи коду
    }

.. note::

    Компілятор за замовчуванням (``\Twig\Compiler``) може бути змінено, шляхом виклику
    методу ``setCompiler()``::

        $twig->setCompiler($compiler);
