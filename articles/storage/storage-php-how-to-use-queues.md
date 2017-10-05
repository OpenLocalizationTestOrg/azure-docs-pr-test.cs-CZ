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
ms.openlocfilehash: 3c8f799a917cfc9d74412d90f27f2ea8c21265d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-php"></a><span data-ttu-id="67c64-104">Používání úložiště Queue z PHP</span><span class="sxs-lookup"><span data-stu-id="67c64-104">How to use Queue storage from PHP</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="67c64-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="67c64-105">Overview</span></span>
<span data-ttu-id="67c64-106">Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="67c64-106">This guide will show you how to perform common scenarios by using the Azure Queue storage service.</span></span> <span data-ttu-id="67c64-107">Ukázky jsou napsané pomocí třídy ze sady Windows SDK pro jazyk PHP.</span><span class="sxs-lookup"><span data-stu-id="67c64-107">The samples are written via classes from the Windows SDK for PHP.</span></span> <span data-ttu-id="67c64-108">Pokryté scénáře zahrnují vkládání, prohlížení, získávání a odstraňování front zpráv, jakož i vytváření a odstraňování front.</span><span class="sxs-lookup"><span data-stu-id="67c64-108">The covered scenarios include inserting, peeking, getting, and deleting queue messages, as well as creating and deleting queues.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="67c64-109">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="67c64-109">Create a PHP application</span></span>
<span data-ttu-id="67c64-110">Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k Azure Queue storage je odkazování třídy ze sady Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="67c64-110">The only requirement for creating a PHP application that accesses Azure Queue storage is the referencing of classes from the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="67c64-111">Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="67c64-111">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="67c64-112">V tomto průvodci použijete funkcí fronty úložiště, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="67c64-112">In this guide, you will use Queue storage features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="67c64-113">Získat knihoven klienta Azure</span><span class="sxs-lookup"><span data-stu-id="67c64-113">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="67c64-114">Konfigurace aplikace pro přístup k úložišti fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-114">Configure your application to access Queue storage</span></span>
<span data-ttu-id="67c64-115">Pokud chcete používat rozhraní API pro Azure Queue storage, budete muset:</span><span class="sxs-lookup"><span data-stu-id="67c64-115">To use the APIs for Azure Queue storage, you need to:</span></span>

1. <span data-ttu-id="67c64-116">Odkaz na soubor automatického zavaděče pomocí [require_once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="67c64-116">Reference the autoloader file by using the [require_once] statement.</span></span>
2. <span data-ttu-id="67c64-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="67c64-117">Reference any classes that you might use.</span></span>

<span data-ttu-id="67c64-118">Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="67c64-118">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="67c64-119">Tento příklad (a další příklady v tomto článku) předpokládá, že instalaci PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="67c64-119">This example (and other examples in this article) assumes that you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="67c64-120">Pokud jste nainstalovali v knihovnách ručně, budete muset odkazovat `WindowsAzure.php` automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="67c64-120">If you installed the libraries manually, you will need to reference the `WindowsAzure.php` autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

<span data-ttu-id="67c64-121">V následujících příkladech `require_once` příkaz zobrazí vždy, ale jenom ty třídy, které jsou potřebné pro tento příklad provést se bude odkazovat.</span><span class="sxs-lookup"><span data-stu-id="67c64-121">In the examples below, the `require_once` statement will be shown always, but only the classes that are necessary for the example to execute will be referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="67c64-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="67c64-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="67c64-123">K vytvoření instance klientem Azure Queue storage, musíte nejprve mít platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="67c64-123">To instantiate an Azure Queue storage client, you must first have a valid connection string.</span></span> <span data-ttu-id="67c64-124">Formát pro připojovací řetězec fronty služby je následující.</span><span class="sxs-lookup"><span data-stu-id="67c64-124">The format for the queue service connection string is as follows.</span></span>

<span data-ttu-id="67c64-125">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="67c64-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="67c64-126">Pro přístup k emulátoru úložiště:</span><span class="sxs-lookup"><span data-stu-id="67c64-126">For accessing the emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="67c64-127">Pokud chcete vytvořit libovolného klienta služby Azure, budete muset použít **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="67c64-127">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="67c64-128">Můžete použít některý z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="67c64-128">You can use either of the following techniques:</span></span>

* <span data-ttu-id="67c64-129">Připojovací řetězec přímo jí předejte.</span><span class="sxs-lookup"><span data-stu-id="67c64-129">Pass the connection string directly to it.</span></span>
* <span data-ttu-id="67c64-130">Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="67c64-130">Use **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="67c64-131">Ve výchozím nastavení, je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="67c64-131">By default, it comes with support for one external source—environmental variables.</span></span>
  * <span data-ttu-id="67c64-132">Můžete přidat nové zdroje tím, že rozšíří **ConnectionStringSource** třídy.</span><span class="sxs-lookup"><span data-stu-id="67c64-132">You can add new sources by extending the **ConnectionStringSource** class.</span></span>

<span data-ttu-id="67c64-133">Příklady podle zde uvedeného se předají připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="67c64-133">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="67c64-134">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-134">Create a queue</span></span>
<span data-ttu-id="67c64-135">A **QueueRestProxy** objektu umožňuje vytvářet fronty pomocí **createQueue** metoda.</span><span class="sxs-lookup"><span data-stu-id="67c64-135">A **QueueRestProxy** object lets you create a queue by using the **createQueue** method.</span></span> <span data-ttu-id="67c64-136">Při vytváření fronty, můžete nastavit možnosti pro frontu, ale to tak není povinný.</span><span class="sxs-lookup"><span data-stu-id="67c64-136">When creating a queue, you can set options on the queue, but doing so is not required.</span></span> <span data-ttu-id="67c64-137">(Následující příklad ukazuje, jak nastavit metadata ve frontě.)</span><span class="sxs-lookup"><span data-stu-id="67c64-137">(The example below shows how to set metadata on a queue.)</span></span>

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
> <span data-ttu-id="67c64-138">Neměli byste tedy spoléhat na rozlišování malých a velkých písmen pro metadata klíče.</span><span class="sxs-lookup"><span data-stu-id="67c64-138">You should not rely on case sensitivity for metadata keys.</span></span> <span data-ttu-id="67c64-139">Všechny klíče se načítají z službu na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="67c64-139">All keys are read from the service in lowercase.</span></span>
> 
> 

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="67c64-140">Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-140">Add a message to a queue</span></span>
<span data-ttu-id="67c64-141">Chcete-li přidat zprávu do fronty, použijte **QueueRestProxy -> createMessage**.</span><span class="sxs-lookup"><span data-stu-id="67c64-141">To add a message to a queue, use **QueueRestProxy->createMessage**.</span></span> <span data-ttu-id="67c64-142">Tato metoda přebírá název fronty, text zprávy a možnosti zprávy (které jsou volitelné).</span><span class="sxs-lookup"><span data-stu-id="67c64-142">The method takes the queue name, the message text, and message options (which are optional).</span></span>

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

## <a name="peek-at-the-next-message"></a><span data-ttu-id="67c64-143">Zobrazení náhledu další zprávy</span><span class="sxs-lookup"><span data-stu-id="67c64-143">Peek at the next message</span></span>
<span data-ttu-id="67c64-144">Můžete prohlížet zprávy (nebo zprávy) na předním fronty bez odebere ji z fronty voláním **QueueRestProxy -> peekMessages**.</span><span class="sxs-lookup"><span data-stu-id="67c64-144">You can peek at a message (or messages) at the front of a queue without removing it from the queue by calling **QueueRestProxy->peekMessages**.</span></span> <span data-ttu-id="67c64-145">Ve výchozím nastavení **peekMessage** metoda vrátí do jedné zprávy, ale tuto hodnotu můžete změnit pomocí **PeekMessagesOptions -> setNumberOfMessages** metoda.</span><span class="sxs-lookup"><span data-stu-id="67c64-145">By default, the **peekMessage** method returns a single message, but you can change that value by using the **PeekMessagesOptions->setNumberOfMessages** method.</span></span>

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

## <a name="de-queue-the-next-message"></a><span data-ttu-id="67c64-146">Vyřazení další zprávy z fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-146">De-queue the next message</span></span>
<span data-ttu-id="67c64-147">Váš kód odebere zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="67c64-147">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="67c64-148">Nejprve volání **QueueRestProxy -> listMessages**, takže zprávu neviditelnou pro jakýkoli jiný kód, který je čtení z fronty.</span><span class="sxs-lookup"><span data-stu-id="67c64-148">First, you call **QueueRestProxy->listMessages**, which makes the message invisible to any other code that's reading from the queue.</span></span> <span data-ttu-id="67c64-149">Ve výchozím nastavení zůstanou tato zpráva neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="67c64-149">By default, this message will stay invisible for 30 seconds.</span></span> <span data-ttu-id="67c64-150">(Pokud zpráva není odstraněn z daného období, se bude zobrazovat na fronty znovu.) K dokončení odebrání zprávy z fronty, musí volat **QueueRestProxy -> deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="67c64-150">(If the message is not deleted in this time period, it will become visible on the queue again.) To finish removing the message from the queue, you must call **QueueRestProxy->deleteMessage**.</span></span> <span data-ttu-id="67c64-151">Tento dvoukrokový proces odebrání zprávy zaručuje, že když kódu se nepodaří zpracovat zprávu z důvodu selhání hardwaru nebo softwaru, jiná instance vašeho kódu můžete stejnou zprávu získat a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="67c64-151">This two-step process of removing a message assures that when your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="67c64-152">Váš kód zavolá metodu **deleteMessage** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="67c64-152">Your code calls **deleteMessage** right after the message has been processed.</span></span>

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

## <a name="change-the-contents-of-a-queued-message"></a><span data-ttu-id="67c64-153">Změna obsahu zpráv zařazených ve frontě</span><span class="sxs-lookup"><span data-stu-id="67c64-153">Change the contents of a queued message</span></span>
<span data-ttu-id="67c64-154">Můžete změnit obsah zprávy přímo ve frontě voláním **QueueRestProxy -> updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="67c64-154">You can change the contents of a message in-place in the queue by calling **QueueRestProxy->updateMessage**.</span></span> <span data-ttu-id="67c64-155">Pokud zpráva představuje pracovní úlohu, mohli byste tuto funkci použít k aktualizaci stavu pracovních úloh.</span><span class="sxs-lookup"><span data-stu-id="67c64-155">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="67c64-156">Následující kód aktualizuje zprávy ve frontě o nový obsah a nastaví limit viditelnosti na jiné 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="67c64-156">The following code updates the queue message with new contents, and it sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="67c64-157">To umožňuje ušetřit tím stav práce, který je spojen s zprávu, a je klient získá další minutu, aby pokračovat v práci na zprávě.</span><span class="sxs-lookup"><span data-stu-id="67c64-157">This saves the state of work that's associated with the message, and it gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="67c64-158">Tímto způsobem může sledovat vícekrokového pracovní postupy pro zprávy ve frontě, aniž by bylo nutné v případě, že krok zpracování z důvodu selhání hardwaru nebo softwaru selže, začít znovu od začátku.</span><span class="sxs-lookup"><span data-stu-id="67c64-158">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="67c64-159">Obvykle byste udržovali také hodnotu počtu opakování, a pokud by se pokus o zpracování zprávy opakoval více než *n*krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="67c64-159">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="67c64-160">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="67c64-160">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="additional-options-for-de-queuing-messages"></a><span data-ttu-id="67c64-161">Další možností pro vyřazování zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-161">Additional options for de-queuing messages</span></span>
<span data-ttu-id="67c64-162">Existují dva způsoby, že si můžete přizpůsobit načítání zpráv z fronty.</span><span class="sxs-lookup"><span data-stu-id="67c64-162">There are two ways that you can customize message retrieval from a queue.</span></span> <span data-ttu-id="67c64-163">Za prvé si můžete načíst dávku zpráv (až 32).</span><span class="sxs-lookup"><span data-stu-id="67c64-163">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="67c64-164">Druhý můžete nastavit delší nebo kratší viditelnost vypršení časového limitu, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="67c64-164">Second, you can set a longer or shorter visibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="67c64-165">Následující příklad kódu používá **getMessages** metoda získat 16 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="67c64-165">The following code example uses the **getMessages** method to get 16 messages in one call.</span></span> <span data-ttu-id="67c64-166">Pak se každá zpráva zpracuje pomocí **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="67c64-166">Then it processes each message by using a **for** loop.</span></span> <span data-ttu-id="67c64-167">Také se pro každou zprávu nastaví časový limit neviditelnosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="67c64-167">It also sets the invisibility timeout to five minutes for each message.</span></span>

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

## <a name="get-queue-length"></a><span data-ttu-id="67c64-168">Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-168">Get queue length</span></span>
<span data-ttu-id="67c64-169">Podle potřeby můžete získat odhadovaný počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="67c64-169">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="67c64-170">**QueueRestProxy -> getQueueMetadata** metoda požádá službu front vrátit metadata o frontě.</span><span class="sxs-lookup"><span data-stu-id="67c64-170">The **QueueRestProxy->getQueueMetadata** method asks the queue service to return metadata about the queue.</span></span> <span data-ttu-id="67c64-171">Volání **getApproximateMessageCount** metodu vráceného objektu poskytuje počet jsou počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="67c64-171">Calling the **getApproximateMessageCount** method on the returned object provides a count of how many messages are in a queue.</span></span> <span data-ttu-id="67c64-172">Počet je pouze přibližné, protože zprávy můžete přidat nebo odebrat po služby front odpoví na žádost.</span><span class="sxs-lookup"><span data-stu-id="67c64-172">The count is only approximate because messages can be added or removed after the queue service responds to your request.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="67c64-173">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="67c64-173">Delete a queue</span></span>
<span data-ttu-id="67c64-174">Chcete-li odstranit frontu a všechny zprávy ve frontě, zavolejte **QueueRestProxy -> deleteQueue** metoda.</span><span class="sxs-lookup"><span data-stu-id="67c64-174">To delete a queue and all the messages in it, call the **QueueRestProxy->deleteQueue** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="67c64-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67c64-175">Next steps</span></span>
<span data-ttu-id="67c64-176">Teď, když jste se naučili základy používání služby Azure Queue storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště:</span><span class="sxs-lookup"><span data-stu-id="67c64-176">Now that you've learned the basics of Azure Queue storage, follow these links to learn about more complex storage tasks:</span></span>

* <span data-ttu-id="67c64-177">Přejděte [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="67c64-177">Visit the [Azure Storage Team blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>

<span data-ttu-id="67c64-178">Další informace naleznete také [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="67c64-178">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
<span data-ttu-id="67c64-179">[require_once]: http://www.php.net/manual/en/function.require-once.php</span><span class="sxs-lookup"><span data-stu-id="67c64-179">[require_once]: http://www.php.net/manual/en/function.require-once.php</span></span>
[Azure Portal]: https://portal.azure.com

