Мой БлогIT БлогМои ПроектыQIWI API Документация (Руководство разработчика)

QIWI API Документация (Руководство разработчика)

Автор Константин в 02.02.2016. Опубликовано Мои Проекты, Парсеры, Программный Код
QIWI API Документация

QIWI API PHP

Данная статья написана в помощью разработчику, который использует мой авторский скрипт QIWI API PHP (QIWIControl). Здесь Вы найдете все API вызовы скрипта а также можете более подробно ознакомиться со всеми возможностями QIWI API PHP. Описание действует только для публичных функций класса. Каждая функция описана по назначению, по принимаемым параметрам, а также описаны возвращаемые значения и создан небольшой пример работы с этой функцией.

По всем вопросам работы функций можете оставлять комментарии к этой статье или писать мне на почту или в skype (мои контакты здесь).

Если ВЫ решились купить мой скрипт то пройдите то пишите мне в скайп или на почту. После оплаты вышлю на ваши реквизиты.

Документация и скрипт постоянно обновляются. Всем купившим скрипт — обновления бесплатны в течении 1 года.

Обозначения в документации:

    Красный цвет — обязательный параметр функции;
    Синий цвет — необязательный параметр функции;
    Зеленый цвет — значение параметра функции по умолчанию;

История обновлений статьи:

    11.03.2016 — Обновлена процедура входа в систему. Фиксы после очередного апдейта QIWI сайта. Добавлена функция определения провайдера по его номеру getProviderByPhone. Убрана часть легаси кода.
    15.02.2016 — Добавлена возможность оплачивать услуги любого провайдера QIWI — payProvider
    11.02.2016 — Изменена процедура получения баланса. Теперь баланс возвращается по всем валютным счетам. Смотрите обновленную документацию loadBalance.
    11.02.2016 — Добавлена поддержка отправки средств в любой валюте на другие киви кошельки — transferMoney. По запросу предоставляется возможность оплаты любого QIWI провайдера. Как с подтверждением по СМС так и без.

Основное преимущество данного QIWI API над оригинальным от QIWI

Данный скрипт позволяет автоматизировать работу любого кошелька без необходимости подключения к API QIWI. Чтобы подключиться к API QIWI необходимо быть юридическим лицом и иметь легальный интернет магазин, который должны одобрить QIWI, заплатить им около 30 тысяч рублей. Затем платить комиссии за работу с этим API. По этому лучший вариант — это данный скрипт. Он работает безупречно!
Подключение QIWI API к своему проекту на PHP

Чтобы подключить скрипт к своему проекту, необходимо распаковать архив скрипта в папку с проектом сохраняя структуру размещения файлов и включить в свой проект файл QIWIControl.php.

    require_once("QIWIControl.php");

Алгоритм работы

Чтобы начать работу Вы должны понимать алгоритм работы скрипта. Он очень прост. Перед началом работы всегда необходимо создать объект класса QIWIControl, затем провести авторизацию под нужным кошельком вызвав метод login и далее в произвольном порядке вызывать любые доступные функции.
Список функций

Функции представлены в произвольном порядке.

    __construct — Конструктор класса;
    login — Авторизация в QIWI;
    getLastError — Получение кода и сообщения последней ошибки;
    loadHistoryDateRange — Загрузка истории транзакции по диапазону дат;
    loadHistoryToday — Загрузка истории транзакций за сегодня;
    loadHistoryYesterday — Загрузка истории транзакций за вчера;
    loadHistoryLastWeek — Загрузка истории транзакций за последнюю неделю;
    findTransaction — Найти транзакции по указанной сумме и комментарию;
    findTransactionByAmount — Найти транзакции только по указанной сумме;
    findTransactionByComment — Найти транзакции только по указанному комментарию;
    newOrder — Выписать новый счет;
    loadBills — Получить список счетов;
    loadBalance — Получить текущий баланс кошелька;
    transferMoney — Перевести деньги на другой счет QIWI;
    confirmSMSCode — Подтвердить транзакцию QIWI с помощью СМС кода;
    payProvider — Оплатить услуги провайдера QIWI;
    getProviderByPhone — Получить код провайдера QIWI по номеру телефона;

__construct

Конструктор класса. Всегда вызывается первым. Создает экземпляр класса.

Параметры:

    $id — Логин QIWI в формате 11 знаков с кодом страны. Можно использовать +. Например +79685554411 или 79685554411;
    $password — Пароль, для доступа к кошельку QIWI;
    $cookie_dir — Путь к папке, в которой будут храниться куки файлы, используемые для работы скрипта. По умолчанию cookie_data. На каждый кошелек предусмотрен свой куки файл, что позволяет одновременно работать с неограниченным числом кошельков;
    $proxy — Если хотите, чтобы работа с кошельком осуществлялась через HTTP/HTTPS прокси, то укажите в этом параметре хост:порт прокси сервера (например «myproxy.ru:3128»), который должен использоваться. По умолчанию false.
    $proxyAuth — Если прокси сервер требует авторизации, то укажите в этом параметре логин и пароль через двоеточие. Например «mylogin:mypassword». По умолчанию false.
    $debug_mode — Если Вы хотите, чтобы скрипт выводил в консоль (на страницу) процесс и результаты своей работы, то укажите этот параметр в true. По умолчанию false.

Пример создания класса:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);

login

Функция авторизации кошелька. Должна вызываться перед использованием любых других функций класса. Возвращает true или false в зависимости от результата авторизации. Детали авторизации можно узнать включим режим отладки в конструкторе класса. Функция работает без параметров, так как использует текущие настройки класса, которые были переданы в него с помощью конструктора.

Пример использования:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        die("Failed to login into QIWI wallet.");
    }

getLastError

Функция возвращает последнюю ошибку или false если нет ошибки. Если все же ошибка была, то возвращается массив с элементами code и message. В code содержится код ошибки, а в message ее текст. Данную функцию нужно вызывать после неудачного вызова другой функции класса (кроме конструктора) для того, чтобы получить детали ошибки, которая помешала достичь требуемого результата.

Например:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }

loadHistoryDateRange

Функция позволяет получить список транзакций по данному кошельку, которые имели место быть в указанном промежутке времени. Транзакции пакуются в массив и возвращаются как результат работы функции.

Параметры функции:

    $date_from — Дата начала промежутка времени. Формат даты «dd.mm.yyyy». Например «27.01.2016».
    $date_to — Дата окончания промежутка времени. Формат даты «dd.mm.yyyy». Например «27.01.2016».

Пример получения списка транзакций за указанный диапазон дат:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    $tr = $qiwi->loadHistoryDateRange("18.10.2015", "06.11.2015");
    print_r($tr);

Вывод данного примера будет иметь вид

Array
(
    [0] => Array
        (
            [id] => 2071000178869449 
            [status] => 1
            [statusText] => Success
            [date] => 06.11.2015
            [time] => 20:54:00
            [datetime] => 06.11.2015 20:54:00
            [provider] => Покупка TK KNAKER IVAKINO, карта 4693****2209
            [comment] => TK KNAKER IVAKINO>KHIMKI MEZHDU       RU
            [cash] => -188
        )

    [1] => Array
        (
            [id] => 2071000178708383 
            [status] => 1
            [statusText] => Success
            [date] => 06.11.2015
            [time] => 12:51:56
            [datetime] => 06.11.2015 12:51:56
            [provider] => Покупка TK KNAKER IVAKINO, карта 4693****2209
            [comment] => TK KNAKER IVAKINO>KHIMKI MEZHDU       RU
            [cash] => -194
        )

    [2] => Array
        (
            [id] => 2071000178511602 
            [status] => 1
            [statusText] => Success
            [date] => 05.11.2015
            [time] => 15:44:56
            [datetime] => 05.11.2015 15:44:56
            [provider] => Покупка TK KNAKER IVAKINO, карта 4693****2209
            [comment] => TK KNAKER IVAKINO>KHIMKI MEZHDU       RU
            [cash] => -575
        )

    [3] => Array
        (
            [id] => 1121100737337608 
            [status] => 1
            [statusText] => Success
            [date] => 05.11.2015
            [time] => 10:42:23
            [datetime] => 05.11.2015 10:42:23
            [provider] => Visa QIWI Wallet QIWI Bank
            [comment] => Пополнение или возврат платежа по QVC\QVP
            [cash] => 3553
        )
)

Формат транзакций используется также для дальнейших манипуляций с этим массивом. Например при помощи функции поиска транзакций.
loadHistoryToday

Функция аналогична loadHistoryDateRange, но только промежуток времени автоматически устанавливается на текущую дату.
loadHistoryYesterday

Функция аналогична loadHistoryDateRange, но только промежуток времени автоматически устанавливается на вчерашний день.
loadHistoryLastWeek

Функция аналогична loadHistoryDateRange, но только промежуток времени автоматически устанавливается на последние семь дней.
findTransaction

Функция позволяет провести поиск транзакций по параметру сумма и комментарий. Возвращает отфильтрованные транзакции, которые удовлетворяют требованиям. Выходной массив получается из фильтрации входного массива.

Параметры функции:

    $tr — Массив транзакций полученный из функций loadHistory*.
    $amount — Сумма или false, если мы не хотим фильтровать по этому параметру.
    $comment — Комментарий в формате RegExp (например «[A-Z]+«).  Выражение автоматически оборачивается в «/» с обеих сторон. Или false если мы не хотим искать по комментарию.

Пример вызова функции findTransaction для фильтрации транзакций с суммой платежа 596 рублей и комментарием, содержащим фразу APTEKA:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    $tr = $qiwi->loadHistoryDateRange("18.10.2015", "06.11.2015");
    $tr = $qiwi->findTransaction($tr, -596, "APTEKA");
    print_r($tr);

Вывод скрипта будет следующим:

Array
(
    [0] => Array
        (
            [id] => 2071000176482982 
            [status] => 1
            [statusText] => Success
            [date] => 26.10.2015
            [time] => 19:44:52
            [datetime] => 26.10.2015 19:44:52
            [provider] => Покупка APTEKA FORTE, карта 4693****2209
            [comment] => APTEKA FORTE>KHIMKI                   RU
            [cash] => -596
        )

)

findTransactionByAmount

Аналог функции findTransaction, принимающий только значение суммы и игнорирующий комментарии транзакции.
findTransactionByComment

Аналог функции findTransaction, принимающий только значение маски комментария и игнорирующий сумму транзакции.
newOrder

Функция предназначена для выписки нового счета.

Параметры:

    $to — Адрес киви кошелька, на который мы хотим выписать новый счет. Например «+380505554433«.
    $amount — Сумма, на которую мы хотим выписать счет. Например 2800. Значение должно быть положительным и больше нуля.
    $currency — Валюта, в которой мы хотим получить указанную сумму. Можно указывать RUB, USD, EUR.
    $comment — Комментарий к счету. Можем указывать а можем ничего не указывать.

Пример выписки нового счета:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    $qiwi->newOrder("+7968045422", 1500, "RUB", "Счет №22112210");

Функция возвращает или true в случае успеха или false в случае ошибки.
loadBills

Функция позволяет получать список счетов как выставленных на текущий кошелек (входящие счета), так и выставленных с текущего кошелька (исходящие счета). Структура счетов схожа со структурой транзакций. Если параметры $dateFrom и $dateTo не указаны то будут загружены счета только за текущий день. Результат работы функции — массив счетов строго определенного формата или false в случае ошибки.

Параметры функции:

    $mode — Режим работы со счетами. Если хотим получить и входящие и исходящие счета, то мы должны указать здесь значение QIWI_BILLS_MODE_INOUT, если хотим получить только входящие QIWI_BILLS_MODE_IN, исходящие QIWI_BILLS_MODE_OUT.
    $dateFrom — Дата начала поиска счетов. Будут загружены счета, созданные начиная с этой даты. Можно не указывать, если указан параметр $dateTo.
    $dateTo — Дата окончания поиска счетов. Будут загружены счета, созданные до этой даты, включительно. Можно не указывать, если указан параметр $dateFrom.

Пример работы функции:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    $tr = $qiwi->loadBills(QIWI_BILLS_MODE_INOUT, "15.12.2015", "16.12.2015");
    print_r($tr);

Результат работы скрипта:

Array
(
    [0] => Array
        (
            [id] => 6787569830
            [status] => 4
            [statusText] => Canceled
            [payDate] => 15.01.2016
            [from] => 
            [to] => 79680675327
            [creationDate] => 16.12.2015
            [comment] => Привет!
            [cash] => 15
        )

    [1] => Array
        (
            [id] => 6787517188
            [status] => 4
            [statusText] => Canceled
            [payDate] => 15.01.2016
            [from] => 
            [to] => 79680675327
            [creationDate] => 16.12.2015
            [comment] => 
            [cash] => 11
        )

    [2] => Array
        (
            [id] => 6787335314
            [status] => 4
            [statusText] => Canceled
            [payDate] => 15.01.2016
            [from] => 
            [to] => 71221123123
            [creationDate] => 16.12.2015
            [comment] => comment
            [cash] => 11
        )

    [3] => Array
        (
            [id] => 6787099352
            [status] => 4
            [statusText] => Canceled
            [payDate] => 15.01.2016
            [from] => Visa QIWI Wallet 79841545864
            [to] => 
            [creationDate] => 16.12.2015
            [comment] => 
            [cash] => 1
        )

    [4] => Array
        (
            [id] => 6786681911
            [status] => 1
            [statusText] => Paid
            [payDate] => 15.01.2016
            [from] => 
            [to] => 79841545864
            [creationDate] => 16.12.2015
            [comment] => Счет №2211334455
            [cash] => 1
        )

    [5] => Array
        (
            [id] => 6784361288
            [status] => 4
            [statusText] => Canceled
            [payDate] => 14.01.2016
            [from] => 
            [to] => 79680675327
            [creationDate] => 15.12.2015
            [comment] => Проверка выставления счета
            [cash] => 1
        )

)

loadBalance

Функция возвращает баланс QIWI кошельков во всех валютах в виде ассоциативного массива в числовом формате (не в текстовом) готовым к дальнейшим арифметическим манипуляциям. Не принимает никаких аргументов. В случае ошибки возвращает false.

Пример работы функции:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    if(($balance = $qiwi->loadBalance()) !== false){
        echo "BALANCE RUB={$balance['RUB']}\n";
        echo "BALANCE EUR={$balance['EUR']}\n";
        echo "BALANCE USD={$balance['USD']}\n";
    }else{
        die("Failed to get QIWI balance.\n");
    }

Вывод скрипта:

BALANCE RUB=11933.21
BALANCE EUR=0
BALANCE USD=0

transferMoney

Функция позволяет программно инициировать перевод средств на любой другой кошелек QIWI в любой валюте. Также если в кошельке присутствует защита по СМС, то потребуется вызвать после этой функции дополнительную функцию подтверждения по СМС. Если подтверждение по СМС отключено — то операция будет завершена немедленно.

Параметры функции:

    $to — Номер QIWI кошелька в формате +79681231122;
    $currency — Валюта перевода RUB, USD или EUR;
    $amount — Сумма перевода. Для дробных разделитель точка. Например «123.21«.
    $comment — Комментарий платежа. Можно не указывать.

Возвращаемое значение: Ассоциативный массив с кодом транзакции, с кодом подтверждения и статусом платежа. Номер транзакции id — это внутренний номер транзакции QIWI, код подтверждения paymentId — это код, который нужно передать в функцию confirmSMSCode для подтверждения оплаты кодом из СМС. Также этот код пойдет в историю платежей в графу код транзакции. Статус state[‘code’] — это статус платежа. Если AwaitingSMSConfirmation — это значит что ожидается подтверждение транзакции по СМС.

Пример использования функции:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    if(!($trInfo = $qiwi->transferMoney("+79680671122", "RUB", 1015.12, "Просто перевод"))){
        die("Failed to transfer money to another QIWI wallet.");
    }
    echo "Transaction info:";
    print_r($trInfo);

Вывод работы функции:

Array
(
    [id] => 7100329842
    [state] => Array
        (
            [code] => AwaitingSMSConfirmation
        )

    [paymentId] => 1455195743806
)

cofirmSMSCode

Функция подтверждает ранее инициированную транзакцию по СМС коду.

Параметры функции:

    $paymentId — Берется из другой функции, которая инициирует новую транзакцию. Например transferMoney;
    $smsCode — Код из СМС. Нужно брать с мобильного телефона или же из скрипта — который напрямую работает с виртуальной СИМ-картой;

Возвращаемые значения: Или true в случае успеха или false. Тогда ошибка сохраняется в lastErrorMsg и lastErrorCode.

Пример работы функции:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    if(!($trInfo = $qiwi->transferMoney("+79680671122", "RUB", 1015.12, "Просто перевод"))){
        die("Failed to transfer money to another QIWI wallet.");
    }
    $paymentId = $trInfo['paymentId'];
    //Код, пришедший по смс: 223344
    if($qiwi->confirmSMSCode($paymentId, '223344')){
        echo "Платеж успешно завершен!";
    }else{
        echo "Ошибка платежа: " . $qiwi->getLastError() . "\n";
    }

payProvider

Функция переводит средства указанному провайдеру. С помощью данной функции можно оплачивать услуги мобильных операторов, интернета, жкх и др.

Параметры функции:

    $provider_id — Числовой код провайдера. Можно взять на сайте QIWI;
    $currency — Валюта операции (RUB, USD, EUR, KAZ);
    $amount — Сумма операции в указанной валюте;
    $fields — Массив полей для передачи провайдеру;
    $comments — Комментарий к переводу;

Функция возвращает ассоциативный массив по аналогии с transferMoney.

Функции обертки:

    payBeeline($number, $currency, $amount, $comments);

Пример работы функции:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    //Пополнить билайн
    $fields = array("account" => "+79681112255");
    if(!($trInfo = $qiwi->payProvider(2, "RUB", 1015.12, $fields, "Пополняю счет себе"))){
        die("Failed to transfer money to QIWI provider.");
    }
    $paymentId = $trInfo['paymentId'];

getProviderByPhone

Функция возвращает числовой код провайдера QIWI для последующего вызова функции payProvider.

Параметры функции:

    $phone — Номер телефона в полном формате включая знак +. Например «+79684443311«.

Например Вам нужно пополнить счет телефона с номером +79685554411:

    require_once("QIWIControl.php");
    $qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
    if(!$qiwi->login()){
        if($err = $qiwi->getLastError()){
            die("Failed to login into QIWI Wallet: " . $err['message']);
        }
        die("Failed to login into QIWI wallet.");
    }
    $phone = "+79681112255";
    //Узнать код провайдера
    $pid = $qiwi->getProviderByPhone($phone);
    //Пополнить счет
    $fields = array("account" => "+79681112255");
    if(!($trInfo = $qiwi->payProvider($pid, "RUB", 1015.12, $fields, "Пополняю счет себе"))){
        die("Failed to transfer money to QIWI provider.");
    }
    $paymentId = $trInfo['paymentId'];



    wave 4 xbox 360 yota Интернет Mac Вещание Онлайн Егор Президент JPEG MovableType Настя Фильм Объектив PostgreSQL Обзор озеро FreeBSD Nikon скрипт EXIF Парсер Nikon 50mm f/1.4G AF-S Я D810 море Canon Parallels Автомобиль парк Зима exiftool Фотография QIWI Перемены Plugin Выборы Украина Лупан Телевидение Desktop VMWare yota интернет 4g activate.iso волна 4 

Архивы

    Февраль 2016 (8)
    Январь 2016 (1)
    Декабрь 2015 (1)
    Ноябрь 2015 (9)
    Октябрь 2015 (7)
    Март 2015 (1)
    Сентябрь 2012 (2)
    Август 2012 (1)
    Май 2012 (2)
    Июнь 2011 (1)
    Май 2011 (2)
    Апрель 2011 (6)
    Март 2011 (1)
    Декабрь 2010 (2)
    Ноябрь 2010 (1)
    Сентябрь 2010 (2)
    Июнь 2010 (8)
    Май 2010 (2)
    Апрель 2010 (2)
    Март 2010 (24)
    Январь 2010 (10)
    Ноябрь 2009 (1)
    Октябрь 2009 (8)
    Сентябрь 2009 (3)
    Август 2009 (41)
    Июль 2009 (45)
    Июнь 2009 (2)

Свежие записи

    Автоматическое удаление комментариев из Instagram по стоп-словам
    Парсер PetShop.ru (Скрипт PHP)
    Парсер BestChange — Курсы обмена валют (PHP)
    Ренегат / Renegade (1987)
    MC Mac OS X? Коллекция портов для Mac OS X — Rudix
