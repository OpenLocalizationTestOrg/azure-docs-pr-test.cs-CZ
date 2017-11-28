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
# <a name="how-toouse-queue-storage-from-php"></a><span data-ttu-id="f21f5-104">Jak toouse úložiště Queue z PHP</span><span class="sxs-lookup"><span data-stu-id="f21f5-104">How toouse Queue storage from PHP</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="f21f5-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="f21f5-105">Overview</span></span>
<span data-ttu-id="f21f5-106">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="f21f5-106">This guide will show you how tooperform common scenarios by using hello Azure Queue storage service.</span></span> <span data-ttu-id="f21f5-107">Ukázky Hello jsou zapsány pomocí třídy z hello Windows SDK pro jazyk PHP.</span><span class="sxs-lookup"><span data-stu-id="f21f5-107">hello samples are written via classes from hello Windows SDK for PHP.</span></span> <span data-ttu-id="f21f5-108">Hello pokryté scénáře zahrnují vkládání, prohlížení, získávání a odstraňování front zpráv, a také vytváření a odstraňování front.</span><span class="sxs-lookup"><span data-stu-id="f21f5-108">hello covered scenarios include inserting, peeking, getting, and deleting queue messages, as well as creating and deleting queues.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="f21f5-109">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="f21f5-109">Create a PHP application</span></span>
<span data-ttu-id="f21f5-110">Hello pouze požadavek pro vytvoření aplikace PHP, který přistupuje k Azure Queue storage je hello odkazující na třídy z hello Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="f21f5-110">hello only requirement for creating a PHP application that accesses Azure Queue storage is hello referencing of classes from hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="f21f5-111">Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="f21f5-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="f21f5-112">V tomto průvodci použijete funkcí fronty úložiště, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="f21f5-112">In this guide, you will use Queue storage features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="f21f5-113">Získání knihovny klienta Azure hello</span><span class="sxs-lookup"><span data-stu-id="f21f5-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="f21f5-114">Konfigurace vaší aplikace tooaccess Queue storage</span><span class="sxs-lookup"><span data-stu-id="f21f5-114">Configure your application tooaccess Queue storage</span></span>
<span data-ttu-id="f21f5-115">hello toouse rozhraní API pro Azure Queue storage, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="f21f5-115">toouse hello APIs for Azure Queue storage, you need to:</span></span>

1. <span data-ttu-id="f21f5-116">Referenční dokumentace hello automatického zavaděče souboru pomocí hello [require_once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="f21f5-116">Reference hello autoloader file by using hello [require_once] statement.</span></span>
2. <span data-ttu-id="f21f5-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="f21f5-117">Reference any classes that you might use.</span></span>

<span data-ttu-id="f21f5-118">Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="f21f5-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="f21f5-119">Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="f21f5-119">This example (and other examples in this article) assumes that you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="f21f5-120">Pokud jste nainstalovali hello knihovny ručně, budete potřebovat tooreference hello `WindowsAzure.php` automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="f21f5-120">If you installed hello libraries manually, you will need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

<span data-ttu-id="f21f5-121">V následujících příkladech hello, hello `require_once` příkaz zobrazí vždy, ale pouze hello třídy, které jsou nezbytné pro tooexecute příklad hello se bude odkazovat.</span><span class="sxs-lookup"><span data-stu-id="f21f5-121">In hello examples below, hello `require_once` statement will be shown always, but only hello classes that are necessary for hello example tooexecute will be referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="f21f5-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="f21f5-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="f21f5-123">tooinstantiate klientem Azure Queue storage, že máte platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f21f5-123">tooinstantiate an Azure Queue storage client, you must first have a valid connection string.</span></span> <span data-ttu-id="f21f5-124">Hello formát pro připojovací řetězec hello fronty služby je následující.</span><span class="sxs-lookup"><span data-stu-id="f21f5-124">hello format for hello queue service connection string is as follows.</span></span>

<span data-ttu-id="f21f5-125">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="f21f5-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="f21f5-126">Pro přístup k hello emulátor úložiště:</span><span class="sxs-lookup"><span data-stu-id="f21f5-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="f21f5-127">toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="f21f5-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="f21f5-128">Můžete použít některý z následujících techniky hello:</span><span class="sxs-lookup"><span data-stu-id="f21f5-128">You can use either of hello following techniques:</span></span>

* <span data-ttu-id="f21f5-129">Předat hello připojovací řetězec přímo tooit.</span><span class="sxs-lookup"><span data-stu-id="f21f5-129">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="f21f5-130">Použití **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="f21f5-130">Use **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="f21f5-131">Ve výchozím nastavení, je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f21f5-131">By default, it comes with support for one external source—environmental variables.</span></span>
  * <span data-ttu-id="f21f5-132">Můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** třídy.</span><span class="sxs-lookup"><span data-stu-id="f21f5-132">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="f21f5-133">Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="f21f5-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="f21f5-134">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="f21f5-134">Create a queue</span></span>
<span data-ttu-id="f21f5-135">A **QueueRestProxy** objektu umožňuje vytvářet fronty pomocí hello **createQueue** metoda.</span><span class="sxs-lookup"><span data-stu-id="f21f5-135">A **QueueRestProxy** object lets you create a queue by using hello **createQueue** method.</span></span> <span data-ttu-id="f21f5-136">Při vytvoření fronty, můžete nastavit možnosti na hello fronty, ale to tak není povinný.</span><span class="sxs-lookup"><span data-stu-id="f21f5-136">When creating a queue, you can set options on hello queue, but doing so is not required.</span></span> <span data-ttu-id="f21f5-137">(hello Příklad dole ukazuje, jak tooset metadata ve frontě.)</span><span class="sxs-lookup"><span data-stu-id="f21f5-137">(hello example below shows how tooset metadata on a queue.)</span></span>

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
> <span data-ttu-id="f21f5-138">Neměli byste tedy spoléhat na rozlišování malých a velkých písmen pro metadata klíče.</span><span class="sxs-lookup"><span data-stu-id="f21f5-138">You should not rely on case sensitivity for metadata keys.</span></span> <span data-ttu-id="f21f5-139">Všechny klíče se načítají z hello služby na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="f21f5-139">All keys are read from hello service in lowercase.</span></span>
> 
> 

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="f21f5-140">Přidat tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="f21f5-140">Add a message tooa queue</span></span>
<span data-ttu-id="f21f5-141">použít tooadd tooa front zpráv, **QueueRestProxy -> createMessage**.</span><span class="sxs-lookup"><span data-stu-id="f21f5-141">tooadd a message tooa queue, use **QueueRestProxy->createMessage**.</span></span> <span data-ttu-id="f21f5-142">Hello metoda přebírá název fronty hello text zprávy hello a možnosti zprávy (které jsou volitelné).</span><span class="sxs-lookup"><span data-stu-id="f21f5-142">hello method takes hello queue name, hello message text, and message options (which are optional).</span></span>

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

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="f21f5-143">Zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="f21f5-143">Peek at hello next message</span></span>
<span data-ttu-id="f21f5-144">Můžete prohlížet zprávy (nebo zprávy) na popředí hello fronty bez odebere ji z fronty hello voláním **QueueRestProxy -> peekMessages**.</span><span class="sxs-lookup"><span data-stu-id="f21f5-144">You can peek at a message (or messages) at hello front of a queue without removing it from hello queue by calling **QueueRestProxy->peekMessages**.</span></span> <span data-ttu-id="f21f5-145">Ve výchozím nastavení, hello **peekMessage** metoda vrátí do jedné zprávy, ale tuto hodnotu můžete změnit pomocí hello **PeekMessagesOptions -> setNumberOfMessages** metoda.</span><span class="sxs-lookup"><span data-stu-id="f21f5-145">By default, hello **peekMessage** method returns a single message, but you can change that value by using hello **PeekMessagesOptions->setNumberOfMessages** method.</span></span>

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

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="f21f5-146">Zrušte hello další zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="f21f5-146">De-queue hello next message</span></span>
<span data-ttu-id="f21f5-147">Váš kód odebere zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="f21f5-147">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="f21f5-148">Nejprve volání **QueueRestProxy -> listMessages**, takže hello zpráva neviditelná tooany jiný kód, který je čtení z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="f21f5-148">First, you call **QueueRestProxy->listMessages**, which makes hello message invisible tooany other code that's reading from hello queue.</span></span> <span data-ttu-id="f21f5-149">Ve výchozím nastavení zůstanou tato zpráva neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="f21f5-149">By default, this message will stay invisible for 30 seconds.</span></span> <span data-ttu-id="f21f5-150">(Pokud na toto časové období není odstraněn uvítací zprávu, se bude zobrazovat ve frontě hello znovu.) toofinish odebírání uvítací zprávu z fronty hello, musí volat **QueueRestProxy -> deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="f21f5-150">(If hello message is not deleted in this time period, it will become visible on hello queue again.) toofinish removing hello message from hello queue, you must call **QueueRestProxy->deleteMessage**.</span></span> <span data-ttu-id="f21f5-151">Tento dvoukrokový proces odebrání zprávy zaručuje, když vaše tooprocess kódu nezdaří, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejné zprávy a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="f21f5-151">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="f21f5-152">Váš kód zavolá metodu **deleteMessage** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="f21f5-152">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

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

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="f21f5-153">Změna obsahu zpráv zařazených ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="f21f5-153">Change hello contents of a queued message</span></span>
<span data-ttu-id="f21f5-154">Můžete změnit obsah zprávy přímo ve frontě hello hello voláním **QueueRestProxy -> updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="f21f5-154">You can change hello contents of a message in-place in hello queue by calling **QueueRestProxy->updateMessage**.</span></span> <span data-ttu-id="f21f5-155">Pokud hello zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate hello stav hello pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="f21f5-155">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="f21f5-156">Hello následující kód aktualizuje zprávy fronty hello nový obsah a nastaví tooextend časový limit viditelnosti hello jiné 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="f21f5-156">hello following code updates hello queue message with new contents, and it sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="f21f5-157">To umožňuje ušetřit hello stav práce, který je spojen s uvítací zprávu, a nabízí další minutu toocontinue pracující na uvítací zprávu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f21f5-157">This saves hello state of work that's associated with hello message, and it gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="f21f5-158">Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="f21f5-158">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="f21f5-159">Obvykle byste udržovali také počet opakování, a pokud hello zpráva se pokus o více než  *n*  krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="f21f5-159">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="f21f5-160">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="f21f5-160">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="additional-options-for-de-queuing-messages"></a><span data-ttu-id="f21f5-161">Další možností pro vyřazování zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="f21f5-161">Additional options for de-queuing messages</span></span>
<span data-ttu-id="f21f5-162">Existují dva způsoby, že si můžete přizpůsobit načítání zpráv z fronty.</span><span class="sxs-lookup"><span data-stu-id="f21f5-162">There are two ways that you can customize message retrieval from a queue.</span></span> <span data-ttu-id="f21f5-163">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="f21f5-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="f21f5-164">Druhý můžete nastavit delší nebo kratší viditelnost vypršení časového limitu, aby měl váš kód, informace nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="f21f5-164">Second, you can set a longer or shorter visibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="f21f5-165">Hello následující příklad kódu používá hello **getMessages** metoda tooget 16 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="f21f5-165">hello following code example uses hello **getMessages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="f21f5-166">Pak se každá zpráva zpracuje pomocí **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="f21f5-166">Then it processes each message by using a **for** loop.</span></span> <span data-ttu-id="f21f5-167">Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="f21f5-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

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

## <a name="get-queue-length"></a><span data-ttu-id="f21f5-168">Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="f21f5-168">Get queue length</span></span>
<span data-ttu-id="f21f5-169">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="f21f5-169">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="f21f5-170">Hello **QueueRestProxy -> getQueueMetadata** metoda požádá hello fronty služby tooreturn metadata o hello fronty.</span><span class="sxs-lookup"><span data-stu-id="f21f5-170">hello **QueueRestProxy->getQueueMetadata** method asks hello queue service tooreturn metadata about hello queue.</span></span> <span data-ttu-id="f21f5-171">Volání hello **getApproximateMessageCount** metoda na hello vrátila objekt poskytuje počet jsou počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="f21f5-171">Calling hello **getApproximateMessageCount** method on hello returned object provides a count of how many messages are in a queue.</span></span> <span data-ttu-id="f21f5-172">počet Hello je pouze přibližné, protože zprávy můžete přidat nebo odebrat po hello fronty služby odpoví tooyour požadavku.</span><span class="sxs-lookup"><span data-stu-id="f21f5-172">hello count is only approximate because messages can be added or removed after hello queue service responds tooyour request.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="f21f5-173">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="f21f5-173">Delete a queue</span></span>
<span data-ttu-id="f21f5-174">toodelete frontu a všechny zprávy hello v něm volat hello **QueueRestProxy -> deleteQueue** metoda.</span><span class="sxs-lookup"><span data-stu-id="f21f5-174">toodelete a queue and all hello messages in it, call hello **QueueRestProxy->deleteQueue** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f21f5-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f21f5-175">Next steps</span></span>
<span data-ttu-id="f21f5-176">Teď, když jste se naučili základy používání hello služby Azure Queue storage, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště:</span><span class="sxs-lookup"><span data-stu-id="f21f5-176">Now that you've learned hello basics of Azure Queue storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="f21f5-177">Navštivte hello [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="f21f5-177">Visit hello [Azure Storage Team blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>

<span data-ttu-id="f21f5-178">Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="f21f5-178">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

