.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

===========
Запуск ноды
===========

.. contents::
   :local:
   :backlinks: none

В этом руководстве вы узнаете, как запустить на своем компьютере ноду
для участия в сети Concordium. Это означает, что вы будете получать
блоки и транзакции от других нод (узлов), а также делиться
информацией о блоках и транзакциях с нодами в Concordium
сети. Следуя этому руководству, вы сможете

-  запустить ноду Concordium
-  найти её на сетевой панели
-  выполнять запросы состояния блокчейна Concordium непосредственно с вашего компьютера

Вам не нужен счёт чтобы запустить ноду.

Прежде чем начать
=================

Прежде чем запустить ноду Concordium вам необходимо

1. Установить и запустить Docker.

   -  на *Linux*, дать доступ Docker'у запускаться как non-root пользователь.

2. Загрузить и извлечь программное обеспечение :ref:`concordium-node-and-client-download`.

Обновление с более ранней версии Open Testnet
=============================================

Для обновления к текущей версии ПО Concordium для Open Testnet 4:

-  Следуйте инструкции по :ref:`загрузке<downloads>` последней версии
   программного обеспечения Concordium.

-  Запустите исполняемый файл``concordium-node-reset-data`` из скачанного
   архива.

   -  Для пользователей *Mac*: когда в первый раз открываете программу,
      кликните правой кнопкой мыши по файлу ``concordium-node-reset-data``
      и выберете **Открыть**. Появится сообщение, что это программное
      обеспечение от неизвестного разработчика. Выберите **Открыть** снова.
   -  Для пользователей *Windows*: когда в первый раз открываете программу,
      дважды кликните на ``concordium-node-reset-data``. Появится сообщение,
      что это программное беспечение от неизвестного разработчика.
      Выберите **Детали** → **Запустить всё равно**.

-  Программа спросит:

      *Do you also want to remove saved keys?*

   Счета которые были созданы для предыдущих версий тестнета более не
   действительны. Следовательно, если у вас есть сохранённые счета,
   мы рекомендуем выбрать опцию **Y**, которая удалит все старые записи.

.. _running-a-node:

Запуск ноды
===========

Чтобы запустить клиент, который присоединится к открытой тестовой сети,
выполните следующие действия:

1. Запустите ``concordium-node`` из скачанного архива.

-  Для пользователей *Mac*: когда в первый раз открываете программу,
   кликните правой кнопкой мыши по файлу ``concordium-node-reset-data``
   и выберете **Открыть**. Появится сообщение, что это программное
   обеспечение от неизвестного разработчика. Выберите **Открыть** снова.
-  Для пользователей *Windows*: когда в первый раз открываете программу,
   дважды кликните на ``concordium-node-reset-data``. Появится сообщение,
   что это программное беспечение от неизвестного разработчика.
   Выберите **Детали** → **Запустить всё равно**.
-  Во время *перезагрузки* ноды рекомендуется использовать опцию
   ``--no-block-state-import``. Это позволит загрузить только те обновления
   блокчейна Concordium, которые повяились пока нода была неактивна и
   ускорит процесс загрузки ноды.

2. Введите имя вашей ноды. Это имя будет отображаться на общей панели.

3. Если инструмент был запущен раньше, вас спросят, хотите ли вы перед
   запуском удалить локальную базу данных ноды. Если вы нажмёте **Y**,
   будут удалены, а затем скачаны заново и восстановлены все данные о
   состоянии блокчейна Concordium, после чего сохранены вам на компьютер.
   **Имейте в виду, что удаление локальной базы данных приведёт к тому,
   что ваша нода будет дольше синхронизироваться с сетью Concordium**

После этого программа загрузит образ Concordium Client и загрузит его в
Docker. Клиент запустится и начнет вывод информации о работе ноды.

Находим свою ноду на общей панели
=================================

После того как запустится ``concordium-node``, вы можете

-  увидеть вашу ноду на панели `Network Dashboard`_
-  :ref:`выполнять запросы<testnet-query-node>` дял получение информации о блоках, транзакциях и счетах

Сетевая панель
--------------

Клиенту понадобится время, чтобы синхронизироваться с текущим состоянием
блокчейна Concordium. Синхронизация включает, например, загрузку
информации обо всех блоках в цепочке.

Помимо другой информации, на панели `Network Dashboard`_ вы можете
получить представление о том, как долго вашей ноде необходимо синхронизироваться.
Для этого вам необходимо сравнить значение ноды **Length** (количество
блоков, которые получила ваша нода) со значением **Total Length** (количество
блоков в самой длинной цепочке в сети), которое вы можете найти сверху
страницы панели.


Включение входящих подключений
==============================

Если вы используете свой узел за брандмауэром или за домашним роутером,
то вы, вероятно, сможете подключиться к другим нодам,
но другие ноды не смогут инициировать подключения к вашей.
Это нормально, и ваша нода будет полностью участвовать в сети
Concordium. Будет возможность отправлять транзакции и
:ref:`если будет настроено<become-a-baker>`, готовить блоки и финализировать.

Однако, вы также можете сделать свою ноду более полезным участником сети,
если разрешите входящие подключения. По умолчанию ``concordium-node``
слушает ``8888`` порт для входящих запросов. В зависимости от вашей сети и
конфигурации платформы, вам необходимо пробросить внешний порт ``8888``
на вашем роутере и\или открыть его в брандамуэре. Детали настройки
сильно зависят от конфигурации вашей сети.

Настройка портов
----------------

Нода слушает 4 порта, которые могут быть настроены путём передачи
соответствующих параметров команд при запуске ноды. Порты, используемые
нодой:

-  8888, порт для peer-to-peer сетевого взаимодействия, может быть изменён параметром
   ``--listen-node-port``
-  8082, порт, используеммый для middleware, может быть изменён параметром ``--listen-middleware-port``
-  10000, порт gRPC, может быть изменён параметром ``--listen-grpc-port``

При изменении портов, docker контейнер должен быть остановлен (:ref:`stop-a-node`),
сброшен, и запущен заново. Для сброса контейнера можно использовать
``concordium-node-reset-data`` или команду ``docker rm concordium-client`` в терминале.

Мы *настойчиво рекомендуем* чтобы ваш брандамуэр был открыт только на
подключение по порту 8888 (peer-to-peer подключения). Открытый доступ
к другим портам может привести к тому, что злоумышленник получит доступ
к вашей ноде или счетам, которые в ней хранятся.

.. _stop-a-node:

Остановка ноды
==============

Чтобы остановить ноду, нажмите в терминале **CTRL+c**, и дождитесь пока нода
выполнит очистку и прекратит работу.

Если вы случайно закроете окно без явного завершения работы
ноды, она будет продолжать работать в фоновом режиме в Docker.
В этом случае используйте файл ``concordium-node-stop`` таким же
образом, как и ``concordium-node``.

Поддержка и обратная связь
==========================

Информацию о логах вашей ноды можно получить с помощью инструмента
``concordium-node-retrieve-logs``. Эта утилита сохранит логи из запущенного образа
в файл. Кроме того, при наличии соответствующего разрешения, она
может записать информацию о программах, запущенных в настоящее время в системе.

Вы можете отправить логи, системную информацию, вопросы, отзывы и пожелания
на электронную почту testnet@concordium.com. Также вы можете найти нас
в `Discord`_, или обратиться к странице :ref:`<troubleshooting-and-known-issues>`
