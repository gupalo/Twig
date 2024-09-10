``date``
========

Фильтр ``date`` форматирует дату в заданный формат:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y") }}

Определитель формата тот же, что поддерживается `date`_,
за исключением случаев, когда отфильтрованные данные имеют тип `DateInterval`_, когда формат должен соответствовать `DateInterval::format`_ вместо этого. 

Фильтр ``date`` принимает строки (они должны быть в формате, который поддерживается функцией
`strtotime`_), экземпляры `DateTime`_ или `DateInterval`_. Например, чтобы отобразить
текущую дату, отфильтруйте слово "now":

.. code-block:: twig

    {{ "now"|date("m/d/Y") }}

Для экранирования слов и символов в формате даты используйте ``\\`` перед каждым
знаком:

.. code-block:: twig

    {{ post.published_at|date("F jS \\a\\t g:ia") }}

Если значение, переданное фильтру ``date``, является ``null``, он вернет
текущую дату по умолчанию. Если вместо текущей даты нужно получить пустую
строку, используйте тернарный оператор:

.. code-block:: twig

    {{ post.published_at is empty ? "" : post.published_at|date("m/d/Y") }}

Если формат не предоставлен, Twig будет использовать формат по умолчанию: ``F j, Y H:i``. 
Этот формат по умолчанию можно изменить, вызвав метод ``setDateFormat()`` в 
экземпляре расширения ``core``. Первый аргумент - это формат по умолчанию для дат,
а второй - формат по умолчанию для интервалов между датами::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setDateFormat('d/m/Y', '%d days');

Часовой пояс
------------

По умолчанию дата отображается с применением часового пояса по умолчанию (того, что
указан в php.ini или объявлен в Twig - см. ниже), но вы можете переопределить его, явно
указав поддерживаемый `часовой пояс`_:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", "Europe/Paris") }}

Если дата уже является объектом DateTime, и вы хотите сохранить ее текущий
часовой пояс, передайте ``false`` как значение часового пояса:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", false) }}

Часовой пояс по умолчанию также можно установить глобально с помощью вызова ``setTimezone()``::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

Аргументы
---------

* ``format``:   Формат даты (по умолчанию - ``F j, Y H:i``, что отображается как ``11 января 2024 15:17``)
* ``timezone``: Дата часового пояса

.. _`strtotime`:            https://www.php.net/strtotime
.. _`DateTime`:             https://www.php.net/DateTime
.. _`DateInterval`:         https://www.php.net/DateInterval
.. _`date`:                 https://www.php.net/date
.. _`DateInterval::format`: https://www.php.net/DateInterval.format
.. _`часовий пояс`:            https://www.php.net/manual/en/timezones.php
