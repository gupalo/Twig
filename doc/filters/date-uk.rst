``date``
========

Фільтр ``date`` форматує дату у заданий формат:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y") }}

Визначник формату той самий, що підтримується `date`_,
за винятком випадків, коли відфільтровані дані мають тип `DateInterval`_, коли формат
натомітсть повинен відповідати `DateInterval::format`_.

Фільтр ``date`` приймає рядки (вони мають бути у форматі, який підтримується функцією
`strtotime`_), екземпляри `DateTime`_ або `DateInterval`_. Наприклад, щоб відобразити
поточну дату, відфільтруйте слово «now»:

.. code-block:: twig

    {{ "now"|date("m/d/Y") }}

Для екранування слів і символів у форматі дати використовуйте ``\\`` перед кожним
знаком:

.. code-block:: twig

    {{ post.published_at|date("F jS \\a\\t g:ia") }}

Якщо значення, передане фільтру ``date``, є ``null``, він поверне
поточну дату за замовчуванням. Якщо замість поточної дати потрібно отримати порожній
рядок, використовуйте тернарний оператор:

.. code-block:: twig

    {{ post.published_at is empty ? "" : post.published_at|date("m/d/Y") }}

Якщо формат не надано, Twig використовуватиме формат за замовчуванням: ``F j, Y H:i``. 
Цей формат за замовчуванням можна змінити, викликавши метод ``setDateFormat()`` в 
екземплярі розширення``core``. Перший аргумент - це формат за замовчуванням для дат,
а другий - формат за замовчуванням для інтервалів між датами::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setDateFormat('d/m/Y', '%d days');

Часовий пояс
------------

За замовчуванням дата відображається із застосуванням часового поясу за замовчуванням (того, що
вказаний у php.ini або оголошений у Twig - див. нижче), але ви можете перевизначити його, явно
вказавши підтримуваний `часовий пояс`_:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", "Europe/Paris") }}

Якщо дата вже є об'єктом DateTime, і ви хочете зберегти її поточний
часовий пояс, передайте ``false`` як значення часового поясу:

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", false) }}

Часовий пояс за замовчуванням також можна встановити глобально за допомогою виклику ``setTimezone()``::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

Аргументи
---------

* ``format``:   Формат дати (за замовчуванням - ``F j, Y H:i``, що відобразиться як ``11 січня 2024 15:17``)
* ``timezone``: Дата часового поясу

.. _`strtotime`:            https://www.php.net/strtotime
.. _`DateTime`:             https://www.php.net/DateTime
.. _`DateInterval`:         https://www.php.net/DateInterval
.. _`date`:                 https://www.php.net/date
.. _`DateInterval::format`: https://www.php.net/DateInterval.format
.. _`часовий пояс`:            https://www.php.net/manual/en/timezones.php
