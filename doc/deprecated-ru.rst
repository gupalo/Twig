Устаревшие функции
==================

В этом документе перечислены устаревшие функции в Twig 3.x. Устаревшие функции сохраняются для обеспечения обратной совместимости и будут удалены в следующем старшем выпуске 
(функция, которая была объявлена устаревшей в Twig 3.x, будет удалена в следующем выпуске).

Функции
-------

 * Функция ``twig_test_iterable`` устарела; вместо этого используйте нативную
  функцию PHP ``is_iterable``.

Расширения
----------

* Все функции, определенные в расширениях Twig, обозначены как внутренние, начиная с 
  Twig 3.9.0, и будут удалены в Twig 4.0. Они были заменены на внутренние методы в 
  соответствующих классах расширений.

  Если вы использовали функцию ``twig_escape_filter()`` в вашем коде, используйте
  ``$env->getRuntime(EscaperRuntime::class)->escape()`` вместо нее.

* Следующие методы из ``Twig\Extension\EscaperExtension`` устарели:
  ``setEscaper()``, ``getEscapers()``, ``setSafeClasses``,
  ``addSafeClasses()``. Используйте такие же методы в классе
  ``Twig\Runtime\EscaperRuntime`` вместо этого:
  
  До:
  ``$twig->getExtension(EscaperExtension::class)->METHOD();``
  
  После:
  ``$twig->getRuntime(EscaperRuntime::class)->METHOD();``

Узлы
----

* Параметр конструктора "tag" класса ``Twig\Node\Node`` устарел, начиная с Twig 3.12,
  поскольку тег теперь автоматически устанавливается парсером, когда это требуется.

* Передача второго аргумента в "ExpressionParser::parseFilterExpressionRaw()"
  устарела, начиная с Twig 3.12.

* Следующие методы ``Twig\Node\Node`` будут принимать строку или целое число (вместо просто
  строки) в Twig 4.0 в качестве аргумента "name": ``getNode()``, ``hasNode()``, ``setNode()``, 
  ``removeNode()`` и ``deprecateNode()``.

* Не передавать экземпляр ``BodyNode`` как тело ``ModuleNode`` или
  ``MacroNode`` устарело, начиная с Twig 3.12.

* Возвращение ``null`` из ``TokenParserInterface::parse()`` устарело, начиная с
  Twig 3.12 (поскольку это запрещено интерфейсом).

* Второй аргумент метода ``Twig\Node\Expression\CallExpression::compileArguments()`` устарел.

* Методы ``Twig\Node\Expression\NameExpression::isSimple()`` м
  ``Twig\Node\Expression\NameExpression::isSpecial()`` являются устарелыми, начиная с Twig 
  3.11 и будут удалены в Twig 4.0.

* Узел ``filter`` в ``Twig\Node\Expression\FilterExpression`` является устаревшим, начиная
  с Twig 3.12 и будет удален в версии 4.0. Вместо него используйте атрибут ``filter``
  для получения фильтра:

  До:
  ``$node->getNode('filter')->getAttribute('value')``

  После:
  ``$node->getAttribute('twig_callable')->getName()``

* Передача имени ``Twig\Node\Expression\FunctionExpression``,
  ``Twig\Node\Expression\FilterExpression`` и
  ``Twig\Node\Expression\TestExpression`` устарела, начиная з Twig 3.12.
  Начиная с Twig 4.0, вам нужно вместо этого передавать ``TwigFunction``, ``TwigFilter``,
  или ``TestFilter``.

  Возьмем к примеру ``FunctionExpression``.

  Если у вас есть узел, который расширяет ``FunctionExpression``, и если вы не переопределяете
  конструктор, вам не нужно ничего делать. Но если вы переопределяете конструктор, то
  вам нужно изменить подсказку типа имени и пометить конструктор атрибутом   ``Twig\Attribute\FirstClassTwigCallableReady``.

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

  После::

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

* Следующие атрибуты ``Twig\Node\Expression\FunctionExpression`` устарели, начиная
  с Twig 3.12: ``needs_charset``, ``needs_environment``,
  ``needs_context``, ``arguments``, ``callable``, ``is_variadic``,
  и ``dynamic_name``.

* Следующие атрибуты ``Twig\Node\Expression\FilterExpression`` устарели, начиная
  с 3.12: ``needs_charset``,  ``needs_environment``,
  ``needs_context``,  ``arguments``,  ``callable``,  ``is_variadic``,
  и ``dynamic_name``.

* Следующие атрибуты ``Twig\Node\Expression\TestExpression`` устарели, начиная с
  3.12: ``arguments``,  ``callable``,  ``is_variadic``, и ``dynamic_name``.

Посетители узлов
----------------

* Класс ``Twig\NodeVisitor\AbstractNodeVisitor`` устарел, вместо этого реализуйте интерфейс
  ``Twig\NodeVisitor\NodeVisitorInterface``.

* Опции ``Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_RAW_FILTER`` и
  ``Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_TEXT_NODES`` устарели, начиная с
  Twig 3.12, и будут удалены в Twig 4.0; они больше ничего не делают.

Парсер
------

* Следующие методы из ``Twig\Parser`` устарели, начиная с Twig 3.12:
  ``getBlockStack()``, ``hasBlock()``, ``getBlock()``, ``hasMacro()``,
  ``hasTraits()``, ``getParent()``.

* Метод ``Twig\ExpressionParser::parseHashExpression()`` устарел, вместо этого
  используйте ``Twig\ExpressionParser::parseMappingExpression()``.

* Метод ``Twig\ExpressionParser::parseArrayExpression()`` устарел, вместо этого
  используйте ``Twig\ExpressionParser::parseSequenceExpression()``.

* Передача ``null`` к ``Twig\Parser::setParent()`` устарела, начиная с Twig
  3.12.

Шаблоны
-------

* Передача экземпляром ``Twig\Template`` к публичному API Twig устарела (как в
  ``Environment::resolveTemplate()``, ``Environment::load()`` и
  ``Template::loadTemplate()``); вместо этого передайте экземпляры ``Twig\TemplateWrapper``.

Фильтры
-------

* Фильтр ``spaceless`` устарел, начиная с Twig 3.12 и будет удален в
  Twig 4.0.

Песочница
---------

* Наличие тегов ``extends`` и ``use``, разрешенных по умолчанию в песочнице, устарело,
  начиная с Twig 3.12. Вам нужно будет явно разрешить их, при необходимости, в версии 4.0.

Тестирование утилит
-------------------

* Реализация метода поставщика данных ``Twig\Test\NodeTestCase::getTests()``
  является устаревшей, начиная с Twig 3.13. Вместо этого, реализуйте статический поставщик данных
  ``provideTests()``.

* Для того, чтобы сделать их функциональность доступной для статических поставщиков данных,
  методы-помощники ``getVariableGetter()`` и ``getAttributeGetter()`` в
  ``Twig\Test\NodeTestCase`` были объявлены устарелыми. Вызовите новые методы
  ``createVariableGetter()`` и ``createAttributeGetter()`` вместо них.

* Метод ``Twig\Test\NodeTestCase::getEnvironment()`` считается окончательным, начиная с
  Twig 3.13. Если вы хотите переопределить, как конструируется окружение Twig, вместо этого
  переопределите ``createEnvironment()``.

* Метод ``getFixturesDir()`` в ``Twig\Test\IntegrationTestCase`` устарел, вместо этого
  реализуйте новый статический метод ``getFixturesDirectory()``, который будет абстрактным
  в 4.0.

* Поставщики данных ``getTests()`` и ``getLegacyTests()`` в
  ``Twig\Test\IntegrationTestCase`` считаются финальными альтернативами, начиная
  с Twig 3.13.
