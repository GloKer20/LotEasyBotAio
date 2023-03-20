<div id="badges">
  <a href="https://t.me/LotEasyBot">
    <img src="https://img.shields.io/badge/Telegram-LotEasyBot-blue" alt="Telegram Badge"/>
  </a>
</div>

# LotEasyBotAio
Игровой Telegram бот, позволяющий играть как в быстрые (с помощью <a href="https://telegram.org/blog/folders#and-one-more-thing">Telegram Dice</a>), так и в онлайн игры с другими пользователями.

# Технологии и инструменты

<a href = "https://www.jetbrains.com/ru-ru/pycharm/">__PyCharm__</a> - среда разработки.


<a href = "https://docs.aiogram.dev/en/dev-3.x/">__aiogram 3.x__</a> - работа с Telegram Bot API.

<a href = "https://www.postgresql.org/">__PostgreSQL__</a> - управление БД.

<a href = "https://docs.pydantic.dev/">__Pydantic__</a> - передача значений токена бота и настроек подключения к БД.

<a href = "https://termius.com/">__Termius__</a> - SSH клиент для управления хостом.

<a href = "https://timeweb.cloud/">__TimeWeb__</a> - хостинг.


# Навигация по файлам
<ul>

<li><b>__main__</b> - запуск проекта, настройки <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/dispatcher.html">диспетчера</a>, подключение <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/router.html">роутеров</a>, внешних <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/middlewares.html">миддлварей</a>, некоторых шаблонных и кастомных <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/filters/index.html">фильтров</a>.</b></li>

<li><b>callback_factory</b> - фабрика Telegram <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/filters/callback_data.html">коллбеков</a>.</li>

<li><b>checkers</b> - ручные и авточекеры, проверяющие возможность вывода, ставки, а также проверка уведомлений пользователей о результатах операций и игр.</li>

<li><b>db</b> - инициализация базы данных, а также запросы к ней.</li>

<li><b>db_connect_create</b> - создание коннектора БД для последующего использования во всех обращениях к ней.</li>

<li><b>fsm</b> - установка состояний <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/finite_state_machine/index.html">машины состояний</a> (конечного автомата).</li>

<li><b>games</b> - обработчик онлайн и оффлайн игр.</li>

<li><b>handlers</b> - определение <a href = "https://docs.aiogram.dev/en/dev-3.x/dispatcher/class_based_handlers/index.html">хэндлеров</a>.</li>

<li><b>filters</b> - определение кастомных фильтров.</li>

<li><b>middlewares</b> - определение внешних и внутренних миддлварей.</li>

<li><b>templates</b> - определение шаблонных сообщений, клавиатур, кнопок и текстов.</li>

<li><b>configs</b> - настройки бота, логирования, а также токена бота и данных БД.</li>

<li><b>difs</b> - текстовые файлы с логами, номерами реквизитов пополнения.</li>
</ul>

# Функции и возможности бота
# Онлайн игры
<p>Следующие смайлы - &#127922 &#127921 &#127920, используемые в боте, с помощью Telegram превращаются в настоящий генератор (псевдо)случайных чисел, что позволяет использовать эту механику для игр прямо в мессенджере.</p>

<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226392290-dc3ffec2-0655-49dc-8f62-2aa368fdc35b.gif" width="500"/>
</div>



# Онлайн игры
Пользователи бота также могут посоревноваться с другими пользователями, выбрав категорию Онлайн игр. В разных играх участвтует разное количество игроков (по умолчанию в Дуэли - 2, в Русской рулетке - 6, в Королевской битве - 10. В зависимости от игры и ставки пользователя закидывает в одну из соответствующих комнат. 

Выбор псевдослучайного числа происходит при помощи Python модуля random. В процессе игры производится анимация загрузки игры, а также выбора числа при помощи стикера. Ниже предоставлен пример игры в Дуэль с разных аккаунтов.
<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226405496-c617d4f5-d3e6-472d-92ae-e3c2ccaadfb9.gif" width="1000"/>
</div>



# Пополнения и выводы
<b>Данный радел несет исключительно информативный формат как методы возможной реализации ввода и вывода условных единиц. Бот не производит никких финансовых операций!</b>

Для пополнения внутриигрового баланса реализована функция пополнения, а также вывода условных единиц. Если пользователь хочет пополнить или вывести сумму, отличающуюся от стандартных (50, 100, 500, 1000 у.е.), реализована функция ввода другой суммы, которая подвергается различным проверкам (корректность числа, величина суммы, при выводе наличие введенной суммы у пользователя на балансе). 

В случае, если пользователь совершает пополнение, в базе данных отображается это, и при нажатии на кнопку проверки пополнения, происходит зачисление средств и завершение операции с последующим уведомлением пользователя. Также в боте реализованы авточекеры, которые раз в заданное время (по умолчанию 30 секунд) проверяют завершенность операций. Если перевод совершен, но операция не завершена, авточекер автоматически завершает операцию и уведомляет пользователя об этом. Процесс пополнения и работы авточекера показаны ниже. 

Вывод средств организован так же, только с дополнительным вводом пользователей реквизитов для вывода. 
<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226409599-5d31ae0d-c8f8-4fc6-b71d-da26a36ccb38.gif" width="500"/>
</div>



# История операций и игр
Для того, чтобы пользователь мог ознакомиться со всеми операциями ввода/вывода, которые он совершил, а также историей игр, реализованы соответсвующие разделы.

Разделы имеют пагинацию, а также с помощью емоджи визуально информативно отображает завершенность и результат. Приложенная ниже гифка отображает данные разделы.

<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226412504-6994f78c-ffee-4f01-a406-b5a66e481b89.gif" width="500"/>
</div>

Для управления операциями, пользователями и администраторами разработана админ панель BPManage. Доступ к админ панели имеют только занесенные в базу данных администраторы.  

С помощью BPManage можно просматривать, подтверждать и изменять операции, управлять пользователями (блокировать, изменять баланс, просматривать их историю операций и игр), а также управлять администраторами, выдавая и забирая полномочия, повышая или понижая их звание.

Существует 4 уровня администраторов. С каждым новым уровнем к предыдущим возможностям добавляются новые. 

Уровни:
<ul>
<li><b>0 (JUNIOR)</b> - может только просматривать информацию об операциях и пользователях.</li>

<li><b>1 (MIDDLE)</b> - может подтверждать операции, блокировать пользователей.</li>

<li><b>2 (MASTER)</b> - может изменять способы и реквизиты операций вывода.</li>

<li><b>3 (SUPERUSER)</b> - имеет возможность изменять баланс пользователей, а также имеет полный доступ к панели управления администраторами.</li>
</ul>

Приведенный ниже короткий пример отображает возможности управления админ панелью BPManage от лица админа 3 уровня. (гифка отвратитльного качества из-за ограничений в 10 МБ GitHub)
<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226424231-4f4f12d8-6b86-4570-a27a-a537f3e095f2.gif" width="500"/>
</div>



# Мелочь, но приятно
В боте реализованы некоторые мелочи, которые порой даже удобны в использовании.
<ul>

<li>При пересылке любого сообщения в бот выводится сообщение об ID пользователя и его имени (если у пользователя выставлены разрешающие настройки профиля). Если пересылает администратор, а сообщение от пользователя, который уже использовал бот, то к информации добавляется кнопка перехода в админ панель управления пользователем (картинка ниже).</li>

<li>При попытке сделать ставку, превышающую баланс пользователя, выведется сообщение с вариантом пополнить. Если пользователь выберет способ и/или сумму пополнения, а потом решит вернуться назад, переход будет совершен к выбору ставок в игре, из которой он нажал кнопку пополнения.</li>

<li>В боте ведутся логи некоторых событий.</li>

<li>В боте настроены некоторые шаблонные и кастомные фильтры (например работа только в приватных ЛС чатах).</li>

<li>Настроены авточекеры пополнения и уведомления о результатах игры (пример авточекера пополнения указан выше).</li>

<li>Если отправить боту стикер, он выведет ID стикера, эмодзи, прикрепленное к стикеру, в также статус анимации.</li>

<li>Если отправить боту сообщение типа Dice (анимированный стикер с миниигрой), бот отреагирует, предложив сыграть все таки с его помощью.</li>
<div id="header" align="center">
  <img src="https://user-images.githubusercontent.com/64438506/226427248-4aeda498-43ac-480a-baf5-0cf3bd3b83de.png" width="500"/>
</div>
</ul>


