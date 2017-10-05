---
title: "Používání úložiště Queue z PHP | Microsoft Docs"
description: "Naučte se používat službu Azure Queue storage vytvářet a odstraňovat fronty a vložit, získání a odstranění zprávy. Ukázky jsou napsané v jazyce PHP."
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
ms.openlocfilehash: 12ebb905184e74da534cd44e8314335145f7042d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-php"></a>Používání úložiště Queue z PHP
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služby Azure Queue storage. Ukázky jsou napsané pomocí třídy ze sady Windows SDK pro jazyk PHP. Pokryté scénáře zahrnují vkládání, prohlížení, získávání a odstraňování front zpráv, jakož i vytváření a odstraňování front.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k Azure Queue storage je odkazování třídy ze sady Azure SDK pro jazyk PHP z vašeho kódu. Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.

V tomto průvodci použijete funkcí fronty úložiště, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-the-azure-client-libraries"></a>Získat knihoven klienta Azure
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurace aplikace pro přístup k úložišti fronty
Pokud chcete používat rozhraní API pro Azure Queue storage, budete muset:

1. Odkaz na soubor automatického zavaděče pomocí [require_once] příkaz.
2. Referenční všechny třídy, které můžete použít.

Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.

> [!NOTE]
> Tento příklad (a další příklady v tomto článku) předpokládá, že instalaci PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali v knihovnách ručně, budete muset odkazovat `WindowsAzure.php` automatického zavaděče souboru.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

V následujících příkladech `require_once` příkaz zobrazí vždy, ale jenom ty třídy, které jsou potřebné pro tento příklad provést se bude odkazovat.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
K vytvoření instance klientem Azure Queue storage, musíte nejprve mít platný připojovací řetězec. Formát pro připojovací řetězec fronty služby je následující.

Pro přístup k službě za provozu:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Pro přístup k emulátoru úložiště:

```php
UseDevelopmentStorage=true
```

Pokud chcete vytvořit libovolného klienta služby Azure, budete muset použít **ServicesBuilder** třídy. Můžete použít některý z následujících postupů:

* Připojovací řetězec přímo jí předejte.
* Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:
  * Ve výchozím nastavení, je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.
  * Můžete přidat nové zdroje tím, že rozšíří **ConnectionStringSource** třídy.

Příklady podle zde uvedeného se předají připojovací řetězec, který přímo.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Vytvoření fronty
A **QueueRestProxy** objektu umožňuje vytvářet fronty pomocí **createQueue** metoda. Při vytváření fronty, můžete nastavit možnosti pro frontu, ale to tak není povinný. (Následující příklad ukazuje, jak nastavit metadata ve frontě.)

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
> Neměli byste tedy spoléhat na rozlišování malých a velkých písmen pro metadata klíče. Všechny klíče se načítají z službu na malá písmena.
> 
> 

## <a name="add-a-message-to-a-queue"></a>Přidání zprávy do fronty
Chcete-li přidat zprávu do fronty, použijte **QueueRestProxy -> createMessage**. Tato metoda přebírá název fronty, text zprávy a možnosti zprávy (které jsou volitelné).

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

## <a name="peek-at-the-next-message"></a>Zobrazení náhledu další zprávy
Můžete prohlížet zprávy (nebo zprávy) na předním fronty bez odebere ji z fronty voláním **QueueRestProxy -> peekMessages**. Ve výchozím nastavení **peekMessage** metoda vrátí do jedné zprávy, ale tuto hodnotu můžete změnit pomocí **PeekMessagesOptions -> setNumberOfMessages** metoda.

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

## <a name="de-queue-the-next-message"></a>Vyřazení další zprávy z fronty
Váš kód odebere zprávu z fronty ve dvou krocích. Nejprve volání **QueueRestProxy -> listMessages**, takže zprávu neviditelnou pro jakýkoli jiný kód, který je čtení z fronty. Ve výchozím nastavení zůstanou tato zpráva neviditelná po dobu 30 sekund. (Pokud zpráva není odstraněn z daného období, se bude zobrazovat na fronty znovu.) K dokončení odebrání zprávy z fronty, musí volat **QueueRestProxy -> deleteMessage**. Tento dvoukrokový proces odebrání zprávy zaručuje, že když kódu se nepodaří zpracovat zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a zkuste to znovu. Váš kód zavolá metodu **deleteMessage** pravým po zpracování zprávy.

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

## <a name="change-the-contents-of-a-queued-message"></a>Změna obsahu zpráv zařazených ve frontě
Můžete změnit obsah zprávy přímo ve frontě voláním **QueueRestProxy -> updateMessage**. Pokud zpráva představuje pracovní úlohu, mohli byste tuto funkci použít k aktualizaci stavu pracovních úloh. Následující kód aktualizuje zprávy ve frontě o nový obsah a nastaví limit viditelnosti na jiné 60 sekund. To umožňuje ušetřit tím stav práce, který je spojen s zprávu, a je klient získá další minutu, aby pokračovat v práci na zprávě. Tímto způsobem může sledovat vícekrokového pracovní postupy pro zprávy ve frontě, aniž by bylo nutné v případě, že krok zpracování z důvodu selhání hardwaru nebo softwaru selže, začít znovu od začátku. Obvykle byste udržovali také hodnotu počtu opakování, a pokud by se pokus o zpracování zprávy opakoval více než *n*krát, odstranili byste ji. Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.

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
Existují dva způsoby, že si můžete přizpůsobit načítání zpráv z fronty. Za prvé si můžete načíst dávku zpráv (až 32). Druhý můžete nastavit delší nebo kratší viditelnost vypršení časového limitu, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv. Následující příklad kódu používá **getMessages** metoda získat 16 zpráv v jednom volání. Pak se každá zpráva zpracuje pomocí **pro** smyčky. Také se pro každou zprávu nastaví časový limit neviditelnosti 5 minut.

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
Podle potřeby můžete získat odhadovaný počet zpráv ve frontě. **QueueRestProxy -> getQueueMetadata** metoda požádá službu front vrátit metadata o frontě. Volání **getApproximateMessageCount** metodu vráceného objektu poskytuje počet jsou počet zpráv ve frontě. Počet je pouze přibližné, protože zprávy můžete přidat nebo odebrat po služby front odpoví na žádost.

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
Chcete-li odstranit frontu a všechny zprávy ve frontě, zavolejte **QueueRestProxy -> deleteQueue** metoda.

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
Teď, když jste se naučili základy používání služby Azure Queue storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště:

* Přejděte [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).

Další informace naleznete také [středisku pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

