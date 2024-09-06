Застарілі функції
=================

У цьому документі перелічено застарілі функції у Twig 3.x. Застарілі функції зберігаються 
для забезпечення зворотної сумісності і будуть вилучені у наступному старшому випуску 
(функція, яка була оголошена застарілою у Twig 3.x, буде видалена у наступному випуску).

Функції
-------

 * Функція ``twig_test_iterable`` застаріла; натомість використовуйте нативну
  функцію PHP ``is_iterable``.

Розширення
----------

* Усі функції, визначені у розширеннях Twig, позначено як внутрішні, починаючи з Twig 3.9.0, і будуть видалені у Twig 4.0. 
  Їх було замінено на внутрішні
  методи у відповідних класах розширень.

  Якщо ви використовували функцію ``twig_escape_filter()`` у вашому коді, використовуйте
  ``$env->getRuntime(EscaperRuntime::class)->escape()`` замість неї.

* Наступні методи з ``Twig\Extension\EscaperExtension`` застаріли:
  ``setEscaper()``, ``getEscapers()``, ``setSafeClasses``,
  ``addSafeClasses()``. Використовуйте такі ж методи у класі
  ``Twig\Runtime\EscaperRuntime`` замість цього:
  
  До:
  ``$twig->getExtension(EscaperExtension::class)->METHOD();``
  
  Після:
  ``$twig->getRuntime(EscaperRuntime::class)->METHOD();``

Вузли
-----

* Параметр конструктора "tag" класу ``Twig\Node\Node`` застарів, починаючи з Twig 3.12,
  оскільки тег тепер автоматично встановлюється парсером, коли це потрібно.

* Передача другого аргументу в "ExpressionParser::parseFilterExpressionRaw()"
  застаріла, починаючи з Twig 3.12.

* Наступні методи ``Twig\Node\Node`` прийматимуть рядок або ціле число (замість просто
  рядка) у Twig 4.0 як аргумент "name": ``getNode()``, ``hasNode()``, ``setNode()``, 
  ``removeNode()`` та ``deprecateNode()``.

* Не передавати екземпляр ``BodyNode`` як тіло ``ModuleNode`` або
  ``MacroNode`` застаріло, починаючи з Twig 3.12.

* Повернення ``null`` з ``TokenParserInterface::parse()`` застаріло, починаючи з
  Twig 3.12 (оскільки це заборонено інтерфейсом).

* Другий аргумент методу ``Twig\Node\Expression\CallExpression::compileArguments()`` є застарілим.

* Методи ``Twig\Node\Expression\NameExpression::isSimple()`` та
  ``Twig\Node\Expression\NameExpression::isSpecial()`` є застарілими, починаючи з Twig 
  3.11 і будуть видалені в Twig 4.0.

* Вузол ``filter`` у ``Twig\Node\Expression\FilterExpression`` є застарілим, починаючи
  з Twig 3.12 і буде видалений у версії 4.0. Замість нього використовуйте атрибут ``filter``
  для отримання фільтра:

  До:
  ``$node->getNode('filter')->getAttribute('value')``

  Після:
  ``$node->getAttribute('twig_callable')->getName()``

* Передача імені ``Twig\Node\Expression\FunctionExpression``,
  ``Twig\Node\Expression\FilterExpression`` та
  ``Twig\Node\Expression\TestExpression`` застаріла, починаючи з Twig 3.12.
  Починаючи з Twig 4.0, вам потрібно натомість передавати ``TwigFunction``, ``TwigFilter``,
  або ``TestFilter``.

  Візьмемо для прикладу ``FunctionExpression``.

  Якщо у вас є вузол, який розширює ``FunctionExpression``, і якщо ви не перевизначаєте
  конструктор, вам не потрібно нічого робити. Але якщо ви перевизначаєте конструктор, то
  вам потрібно змінити підказку типу імені та позначити конструктор атрибутом   ``Twig\Attribute\FirstClassTwigCallableReady``.

  До::

      class NotReadyFunctionExpression extends FunctionExpression
      {
          public function __construct(string $function, Node $arguments, int $lineno)
          {
              parent::__construct($function, $arguments, $lineno);
          }
      }

      class NotReadyFilterExpression extends FilterExpression
      {
          public function __construct(Node $node, ConstantExpression $filter, Node $arguments, int $lineno)
          {
              parent::__construct($node, $filter, $arguments, $lineno);
          }
      }

      class NotReadyTestExpression extends TestExpression
      {
          public function __construct(Node $node, string $test, ?Node $arguments, int $lineno)
          {
              parent::__construct($node, $test, $arguments, $lineno);
          }
      }

  Після::

      class ReadyFunctionExpression extends FunctionExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(TwigFunction|string $function, Node $arguments, int $lineno)
          {
              parent::__construct($function, $arguments, $lineno);
          }
      }

      class ReadyFilterExpression extends FilterExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(Node $node, TwigFilter|ConstantExpression $filter, Node $arguments, int $lineno)
          {
              parent::__construct($node, $filter, $arguments, $lineno);
          }
      }

      class ReadyTestExpression extends TestExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(Node $node, TwigTest|string $test, ?Node $arguments, int $lineno)
          {
              parent::__construct($node, $test, $arguments, $lineno);
          }
      }

* Наступні атрибути ``Twig\Node\Expression\FunctionExpression`` застаріли, починаючи
  з Twig 3.12: ``needs_charset``,  ``needs_environment``,
  ``needs_context``,  ``arguments``,  ``callable``,  ``is_variadic``,
  та ``dynamic_name``.

* Наступні атрибути ``Twig\Node\Expression\FilterExpression`` застаріли, починаючи
  з 3.12: ``needs_charset``,  ``needs_environment``,
  ``needs_context``,  ``arguments``,  ``callable``,  ``is_variadic``,
  та ``dynamic_name``.

* Наступні атрибути ``Twig\Node\Expression\TestExpression`` застаріли, починаючи з
  3.12: ``arguments``,  ``callable``,  ``is_variadic``, та ``dynamic_name``.

Відвідувачі вузлів
------------------

* Клас ``Twig\NodeVisitor\AbstractNodeVisitor`` застарів, натомість реалізуйте інтерфейс
  ``Twig\NodeVisitor\NodeVisitorInterface``.

* Опції ``Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_RAW_FILTER`` та
  ``Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_TEXT_NODES`` застаріли, починаючи
  з Twig 3.12, і будуть видалені в Twig 4.0; вони більше нічого не роблять.

Парсер
------

* Наступні методи з ``Twig\Parser`` застаріли, починаючи з Twig 3.12:
  ``getBlockStack()``, ``hasBlock()``, ``getBlock()``, ``hasMacro()``,
  ``hasTraits()``, ``getParent()``.

* Метод ``Twig\ExpressionParser::parseHashExpression()`` застарів, натомість
  використовуйте ``Twig\ExpressionParser::parseMappingExpression()``.

* Метод ``Twig\ExpressionParser::parseArrayExpression()`` застарів, натомість
  використовуйте ``Twig\ExpressionParser::parseSequenceExpression()``.

* Передача ``null`` до ``Twig\Parser::setParent()`` застаріла, починаючи з Twig
  3.12.

Шаблони
-------

* Передача екземплярів ``Twig\Template`` до публічного API Twig застаріла (як в
  ``Environment::resolveTemplate()``, ``Environment::load()`` та
  ``Template::loadTemplate()``); натомість передайте екземпляри ``Twig\TemplateWrapper``.

Фільтри
-------

* Фільтр ``spaceless`` застарів, починаючи з Twig 3.12 і буде видалений в
  Twig 4.0.

Пісочниця
---------

* Наявність тегів ``extends`` та ``use``, дозволених за замовчуванням в пісочниці, застарілa,
  починаючи з  Twig 3.12. Вам потрібно буде явно дозволити їх за необхідності в версії 4.0.

Тестування утиліт
-----------------

* Реалізація методу постачальника даних ``Twig\Test\NodeTestCase::getTests()``
  є застарілою, починаючи з Twig 3.13. Натомість, реалізуйте статичний постачальник даних
  ``provideTests()``.

* Для того, щоб зробити їх функціональність доступною для статичних постачальників даних, методи-помічники   ``getVariableGetter()`` та ``getAttributeGetter()`` у
  ``Twig\Test\NodeTestCase`` були оголошені застарілими. Викличте нові методи
  ``createVariableGetter()`` та ``createAttributeGetter()`` замість них.

* Метод ``Twig\Test\NodeTestCase::getEnvironment()`` вважається фінальним, починаючи з
  Twig 3.13. Якщо ви хочете перевизначити, як конструюється середовище Twig, натомість
  перевизначіть ``createEnvironment()``.

* Метод ``getFixturesDir()`` у ``Twig\Test\IntegrationTestCase`` застарів, натомість
  реалізуйте новий статичний метод ``getFixturesDirectory()``, який буде абстрактним
  в 4.0.

* Постачальники даних ``getTests()`` та ``getLegacyTests()`` в
  ``Twig\Test\IntegrationTestCase`` вважаються фінальними альтернативами, починаючи
  з Twig 3.13.
