Интерналы Twig
==============

Twig очень расширяемый, и вы можете его хакнуть. Имейте в виду, что вам
стоит попробовать создать расширение перед тем, как хакнуть ядро, поскольку большинство
функций и улучшений можно реализовать с помощью расширений. Этот раздел также может быть
полезным для людей, которые хотят понять, как работает Twig "за кулисами".

Как работает Twig?
------------------

Отображение шаблона Twig можно разбить на четыре ключевых этапа:

* **Загрузите** шаблон: Если шаблон уже скомпилирован, загрузите его и перейдите к шагу
  к шагу *оценивания*, в противном случае:

  * Сначала, **лексер** токенизирует исходный код шаблона на небольшие фрагменты
    для облегчения обработки;

  * Затем **парсер** преобразует поток токенов в осмысленное дерево узлов (Дерево 
     Абстрактного Синтаксиса, AST);

  * Наконец, **компилятор** преобразует AST в PHP-код.

* **Оцените** шаблон: Это означает вызов метода ``display()`` скомпилированного
  шаблона и передача ему контекста.

Лексер
------

Лексер токенизирует исходный код шаблона в поток токенов (каждый токен является
экземпляром `\Twig\Token``, а поток - экземпляром ``\Twig\TokenStream``). Стандартный 
лексер распознает 15 различных типов токенов:

* ``\Twig\Token::BLOCK_START_TYPE``, ``\Twig\Token::BLOCK_END_TYPE``: Разделители для блоков (``{% %}``)
* ``\Twig\Token::VAR_START_TYPE``, ``\Twig\Token::VAR_END_TYPE``: Разделители для переменных (``{{ }}``)
* ``\Twig\Token::TEXT_TYPE``: Текст за выражением;
* ``\Twig\Token::NAME_TYPE``: Имя в выражении;
* ``\Twig\Token::NUMBER_TYPE``: Число в выражении;
* ``\Twig\Token::STRING_TYPE``: Строка в выражении;
* ``\Twig\Token::OPERATOR_TYPE``: Оператор;
* ``\Twig\Token::ARROW_TYPE``: Оператор функции стрелки (``=>``);
* ``\Twig\Token::SPREAD_TYPE``: Оператор спереди (``...``);
* ``\Twig\Token::PUNCTUATION_TYPE``: Знак пунктуации;
* ``\Twig\Token::INTERPOLATION_START_TYPE``, ``\Twig\Token::INTERPOLATION_END_TYPE``: Разделители для     интерполяции строк;
* ``\Twig\Token::EOF_TYPE``: Окончание шаблона.

Вы можете вручную преобразовать исходный код в поток токенов, вызвав метод окружения
``tokenize()``::

    $stream = $twig->tokenize(new \Twig\Source($source, $identifier));

Поскольку поток имеет метод ``__toString()``, вы можете иметь текстовое
представление потока путем повторения объекта::

    echo $stream."\n";

Вот вывод для шаблона ``Hello {{ name }}``:

.. code-block:: text

    TEXT_TYPE(Hello )
    VAR_START_TYPE()
    NAME_TYPE(name)
    VAR_END_TYPE()
    EOF_TYPE()

.. note::

    Лексер по умолчанию (``\Twig\Lexer``) может быть изменен путем вызова
    метода ``setLexer()``::

        $twig->setLexer($lexer);

Парсер
------

Парсер преобразует поток токенов в AST (дерево абстрактного синтаксиса), или дерево узлов (экземпляр ``\Twig\Node\ModuleNode\ModuleNode``). Основное расширение определяет
базовые узлы, такие как: ``for``, ``if``, ... и узлы выражений.

Вы можете вручную преобразовать поток токенов в дерево узлов, вызвав функцию метода среды
``parse()``::

    $nodes = $twig->parse($stream);

Повторение объекта узла дает вам хорошее представление дерева::

    echo $nodes."\n";

Вот вывод для шаблона ``Hello {{ name }}``:

.. code-block:: text

    \Twig\Node\ModuleNode(
      \Twig\Node\TextNode(Hello )
      \Twig\Node\PrintNode(
        \Twig\Node\Expression\NameExpression(name)
      )
    )

.. note::

    Парсер по умолчанию (``\Twig\TokenParser\AbstractTokenParser``) может быть изменен, путем
    вызова метода ``setParser()``::

        $twig->setParser($parser);

Компилятор
----------

Последний шаг выполняется компилятором. Он получает дерево узлов в качестве ввода и
генерирует PHP-код, пригодный для использования во время выполнения шаблона.

Вы можете вручную скомпилировать дерево узлов в PHP-код с помощью метода окружения 
``compile()``::

    $php = $twig->compile($nodes);

Сгенерированный шаблон для шаблона ``Hello {{ name }}`` выглядит следующим образом
(фактический вывод может отличаться в зависимости от версии Twig, которую вы используете)::

    /* Hello {{ name }} */
    class __TwigTemplate_1121b6f109fe93ebe8c6e22e3712bceb extends Template
    {
        protected function doDisplay(array $context, array $blocks = []): iterable
        {
            $macros = $this->macros;
            // строка 1
            yield "Hello ";
            // строка 2
            yield $this->env->getRuntime('Twig\Runtime\EscaperRuntime')->escape((isset($context["name"]) || array_key_exists("name", $context) ? $context["name"] : (function () { throw new RuntimeError('Variable "name" does not exist.', 2, $this->source); })()), "html", null, true);
            return; yield '';
        }

        // еше немного кода
    }

.. note::

    Компилятор по умолчанию (``\Twig\Compiler``) может быть изменен, путем вызова
    метода ``setCompiler()``::

        $twig->setCompiler($compiler);
