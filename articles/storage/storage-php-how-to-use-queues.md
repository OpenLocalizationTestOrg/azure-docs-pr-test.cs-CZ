---
title: "aaaHow toouse úložiště Queue z PHP | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure Queue storage služby toocreate a odstranění fronty a vložení, získání a odstranění zprávy. Ukázky jsou napsané v jazyce PHP."
documentationcenter: php
services: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7582b208-4851-4489-a74a-bb952569f55b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 5034faf3b5f28f72d5b56ac1ce7a5723be697ce6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-php"></a>Jak toouse úložiště Queue z PHP
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage. Ukázky Hello jsou zapsány pomocí třídy z hello Windows SDK pro jazyk PHP. Hello pokryté scénáře zahrnují vkládání, prohlížení, získávání a odstraňování front zpráv, a také vytváření a odstraňování front.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Hello pouze požadavek pro vytvoření aplikace PHP, který přistupuje k Azure Queue storage je hello odkazující na třídy z hello Azure SDK pro jazyk PHP z vašeho kódu. Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.

V tomto průvodci použijete funkcí fronty úložiště, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-hello-azure-client-libraries"></a>Získání knihovny klienta Azure hello
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a>Konfigurace vaší aplikace tooaccess Queue storage
hello toouse rozhraní API pro Azure Queue storage, potřebujete:

1. Referenční dokumentace hello automatického zavaděče souboru pomocí hello [require_once] příkaz.
2. Referenční všechny třídy, které můžete použít.

Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.

> [!NOTE]
> Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali hello knihovny ručně, budete potřebovat tooreference hello `WindowsAzure.php` automatického zavaděče souboru.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

V následujících příkladech hello, hello `require_once` příkaz zobrazí vždy, ale pouze hello třídy, které jsou nezbytné pro tooexecute příklad hello se bude odkazovat.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
tooinstantiate klientem Azure Queue storage, že máte platný připojovací řetězec. Hello formát pro připojovací řetězec hello fronty služby je následující.

Pro přístup k službě za provozu:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Pro přístup k hello emulátor úložiště:

```php
UseDevelopmentStorage=true
```

toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy. Můžete použít některý z následujících techniky hello:

* Předat hello připojovací řetězec přímo tooit.
* Použití **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:
  * Ve výchozím nastavení, je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.
  * Můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** třídy.

Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Vytvoření fronty
A **QueueRestProxy** objektu umožňuje vytvářet fronty pomocí hello **createQueue** metoda. Při vytvoření fronty, můžete nastavit možnosti na hello fronty, ale to tak není povinný. (hello Příklad dole ukazuje, jak tooset metadata ve frontě.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueRestProxy->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Neměli byste tedy spoléhat na rozlišování malých a velkých písmen pro metadata klíče. Všechny klíče se načítají z hello služby na malá písmena.
> 
> 

## <a name="add-a-message-tooa-queue"></a>Přidat tooa fronty zpráv
použít tooadd tooa front zpráv, **QueueRestProxy -> createMessage**. Hello metoda přebírá název fronty hello text zprávy hello a možnosti zprávy (které jsou volitelné).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Create message.
    $builder = new ServicesBuilder();
    $queueRestProxy->createMessage("myqueue", "Hello World!");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-hello-next-message"></a>Zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy (nebo zprávy) na popředí hello fronty bez odebere ji z fronty hello voláním **QueueRestProxy -> peekMessages**. Ve výchozím nastavení, hello **peekMessage** metoda vrátí do jedné zprávy, ale tuto hodnotu můžete změnit pomocí hello **PeekMessagesOptions -> setNumberOfMessages** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueRestProxy->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-hello-next-message"></a>Zrušte hello další zprávu ve frontě
Váš kód odebere zprávu z fronty ve dvou krocích. Nejprve volání **QueueRestProxy -> listMessages**, takže hello zpráva neviditelná tooany jiný kód, který je čtení z fronty hello. Ve výchozím nastavení zůstanou tato zpráva neviditelná po dobu 30 sekund. (Pokud na toto časové období není odstraněn uvítací zprávu, se bude zobrazovat ve frontě hello znovu.) toofinish odebírání uvítací zprávu z fronty hello, musí volat **QueueRestProxy -> deleteMessage**. Tento dvoukrokový proces odebrání zprávy zaručuje, když vaše tooprocess kódu nezdaří, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejné zprávy a zkuste to znovu. Váš kód zavolá metodu **deleteMessage** hned po zpracování zprávy hello.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-hello-contents-of-a-queued-message"></a>Změna obsahu zpráv zařazených ve frontě hello
Můžete změnit obsah zprávy přímo ve frontě hello hello voláním **QueueRestProxy -> updateMessage**. Pokud hello zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate hello stav hello pracovní úlohy. Hello následující kód aktualizuje zprávy fronty hello nový obsah a nastaví tooextend časový limit viditelnosti hello jiné 60 sekund. To umožňuje ušetřit hello stav práce, který je spojen s uvítací zprávu, a nabízí další minutu toocontinue pracující na uvítací zprávu klienta hello. Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru. Obvykle byste udržovali také počet opakování, a pokud hello zpráva se pokus o více než  *n*  krát, odstranili byste ji. Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueRestProxy->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-de-queuing-messages"></a>Další možností pro vyřazování zpráv z fronty
Existují dva způsoby, že si můžete přizpůsobit načítání zpráv z fronty. Nejprve můžete načíst dávku zpráv (až too32). Druhý můžete nastavit delší nebo kratší viditelnost vypršení časového limitu, aby měl váš kód, informace nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá hello **getMessages** metoda tooget 16 zpráv v jednom volání. Pak se každá zpráva zpracuje pomocí **pro** smyčky. Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueRestProxy->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a>Získání délky fronty
Můžete získat odhad hello počet zpráv ve frontě. Hello **QueueRestProxy -> getQueueMetadata** metoda požádá hello fronty služby tooreturn metadata o hello fronty. Volání hello **getApproximateMessageCount** metoda na hello vrátila objekt poskytuje počet jsou počet zpráv ve frontě. počet Hello je pouze přibližné, protože zprávy můžete přidat nebo odebrat po hello fronty služby odpoví tooyour požadavku.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueRestProxy->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a>Odstranění fronty
toodelete frontu a všechny zprávy hello v něm volat hello **QueueRestProxy -> deleteQueue** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Delete queue.
    $queueRestProxy->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání hello služby Azure Queue storage, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště:

* Navštivte hello [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).

Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

