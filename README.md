


require_once("QIWIControl.php");
$qiwi = new QIWIControl("+79681234433", "myPwd1", "cookie_data", "payproxys.ru:3128", "account1:accPWD2211", true);
if(!$qiwi->login()){
    if($err = $qiwi->getLastError()){
        die("Failed to login into QIWI Wallet: " . $err['message']);
    }
    die("Failed to login into QIWI wallet.");
}
$qiwi->newOrder("+7968045422", 1500, "RUB", "Счет №22112210");


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





Array ( 
[0] => Array ( [id] => 6787569830 [status] => 4 [statusText] => Canceled [payDate] => 15.01.2016 [from] => [to] => 79680675327 [creationDate] => 16.12.2015 [comment] => Привет! [cash] => 15 ) 
[1] => Array ( [id] => 6787517188 [status] => 4 [statusText] => Canceled [payDate] => 15.01.2016 [from] => [to] => 79680675327 [creationDate] => 16.12.2015 [comment] => [cash] => 11 ) 
[2] => Array ( [id] => 6787335314 [status] => 4 [statusText] => Canceled [payDate] => 15.01.2016 [from] => [to] => 71221123123 [creationDate] => 16.12.2015 [comment] => comment [cash] => 11 )
[3] => Array ( [id] => 6787099352 [status] => 4 [statusText] => Canceled [payDate] => 15.01.2016 [from] => Visa QIWI Wallet 79841545864 [to] => [creationDate] => 16.12.2015 [comment] => [cash] => 1 )
[4] => Array ( [id] => 6786681911 [status] => 1 [statusText] => Paid [payDate] => 15.01.2016 [from] => [to] => 79841545864 [creationDate] => 16.12.2015 [comment] => Счет №2211334455 [cash] => 1 ) [5] => Array ( [id] => 6784361288 [status] => 4 [statusText] => Canceled [payDate] => 14.01.2016 [from] => [to] => 79680675327 [creationDate] => 15.12.2015 [comment] => Проверка выставления счета [cash] => 1 ) )



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

