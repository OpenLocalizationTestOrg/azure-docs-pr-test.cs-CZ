---
title: aaaHow toouse Service Bus fronty s PHP | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 8cf233176029b679d172eaf713632087beca5e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-php"></a><span data-ttu-id="9ae35-104">Jak toouse Service Bus fronty s PHP</span><span class="sxs-lookup"><span data-stu-id="9ae35-104">How toouse Service Bus queues with PHP</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="9ae35-105">Tento průvodce vám ukáže, jak toouse fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9ae35-105">This guide shows you how toouse Service Bus queues.</span></span> <span data-ttu-id="9ae35-106">Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="9ae35-106">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="9ae35-107">Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="9ae35-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="9ae35-108">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="9ae35-108">Create a PHP application</span></span>
<span data-ttu-id="9ae35-109">Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob hello je hello odkazující na tříd v hello [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="9ae35-109">hello only requirement for creating a PHP application that accesses hello Azure Blob service is hello referencing of classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="9ae35-110">Můžete všechny toocreate nástroje pro vývoj aplikací nebo Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9ae35-110">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="9ae35-111">Instalace PHP musí mít také hello [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.</span><span class="sxs-lookup"><span data-stu-id="9ae35-111">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="9ae35-112">V této příručce bude používat funkce služby, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="9ae35-112">In this guide, you will use service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="9ae35-113">Získání knihovny klienta Azure hello</span><span class="sxs-lookup"><span data-stu-id="9ae35-113">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="9ae35-114">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="9ae35-114">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="9ae35-115">fronty Service Bus hello toouse rozhraní API, hello následující:</span><span class="sxs-lookup"><span data-stu-id="9ae35-115">toouse hello Service Bus queue APIs, do hello following:</span></span>

1. <span data-ttu-id="9ae35-116">Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require_once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ae35-116">Reference hello autoloader file using hello [require_once][require_once] statement.</span></span>
2. <span data-ttu-id="9ae35-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="9ae35-117">Reference any classes you might use.</span></span>

<span data-ttu-id="9ae35-118">Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-118">hello following example shows how tooinclude hello autoloader file and reference hello `ServicesBuilder` class.</span></span>

> [!NOTE]
> <span data-ttu-id="9ae35-119">Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="9ae35-119">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="9ae35-120">Pokud jste nainstalovali hello knihovny ručně nebo jako balíček HRUŠKAMI, musíte odkázat hello **WindowsAzure.php** automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="9ae35-120">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="9ae35-121">V následujících příkladech hello, hello `require_once` příkaz vždy se zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.</span><span class="sxs-lookup"><span data-stu-id="9ae35-121">In hello examples below, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="9ae35-122">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="9ae35-122">Set up a Service Bus connection</span></span>
<span data-ttu-id="9ae35-123">tooinstantiate Service Bus klient platný připojovací řetězec musí mít nejprve v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="9ae35-123">tooinstantiate a Service Bus client, you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="9ae35-124">Kde `Endpoint` je obvykle ve formátu hello `[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="9ae35-124">Where `Endpoint` is typically of hello format `[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="9ae35-125">toocreate libovolného klienta služby Azure, je nutné použít hello `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-125">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="9ae35-126">Můžete:</span><span class="sxs-lookup"><span data-stu-id="9ae35-126">You can:</span></span>

* <span data-ttu-id="9ae35-127">Předat hello připojovací řetězec přímo tooit.</span><span class="sxs-lookup"><span data-stu-id="9ae35-127">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="9ae35-128">Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="9ae35-128">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="9ae35-129">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="9ae35-129">By default it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="9ae35-130">Můžete přidat nové zdroje rozšířením hello `ConnectionStringSource` – třída</span><span class="sxs-lookup"><span data-stu-id="9ae35-130">You can add new sources by extending hello `ConnectionStringSource` class</span></span>

<span data-ttu-id="9ae35-131">Zde uvedené příklady hello je předaná přímo hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="9ae35-131">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="9ae35-132">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="9ae35-132">Create a queue</span></span>
<span data-ttu-id="9ae35-133">Můžete provádět operace správy front služby Service Bus přes hello `ServiceBusRestProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-133">You can perform management operations for Service Bus queues via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="9ae35-134">A `ServiceBusRestProxy` je objekt vytvořený prostřednictvím hello `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje hello tokenu oprávnění toomanage ho.</span><span class="sxs-lookup"><span data-stu-id="9ae35-134">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="9ae35-135">Následující příklad ukazuje, jak Hello tooinstantiate `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createQueue` toocreate frontu s názvem `myqueue` v rámci `MySBNamespace` oboru názvů služby:</span><span class="sxs-lookup"><span data-stu-id="9ae35-135">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createQueue` toocreate a queue named `myqueue` within a `MySBNamespace` service namespace:</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> <span data-ttu-id="9ae35-136">Můžete použít hello `listQueues` metodu `ServiceBusRestProxy` objekty toocheck Pokud fronta se zadaným názvem již existuje v rámci oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9ae35-136">You can use hello `listQueues` method on `ServiceBusRestProxy` objects toocheck if a queue with a specified name already exists within a namespace.</span></span>
> 
> 

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="9ae35-137">Odesílat zprávy fronty tooa</span><span class="sxs-lookup"><span data-stu-id="9ae35-137">Send messages tooa queue</span></span>
<span data-ttu-id="9ae35-138">toosend fronty Service Bus zprávu tooa aplikace volá hello `ServiceBusRestProxy->sendQueueMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="9ae35-138">toosend a message tooa Service Bus queue, your application calls hello `ServiceBusRestProxy->sendQueueMessage` method.</span></span> <span data-ttu-id="9ae35-139">Hello následující kód ukazuje, jak toosend zpráva toohello `myqueue` fronty vytvořili v rámci `MySBNamespace` oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="9ae35-139">hello following code shows how toosend a message toohello `myqueue` queue previously created within the `MySBNamespace` service namespace.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="9ae35-140">Zprávy odeslané příliš (a přijaté z) fronty Service Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-140">Messages sent too(and received from ) Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="9ae35-141">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních metod a vlastností, které jsou používané toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="9ae35-141">[BrokeredMessage][BrokeredMessage] objects have a set of standard methods and properties that are used toohold custom application-specific properties, and a body of arbitrary application data.</span></span>

<span data-ttu-id="9ae35-142">Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="9ae35-142">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="9ae35-143">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="9ae35-143">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="9ae35-144">Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě.</span><span class="sxs-lookup"><span data-stu-id="9ae35-144">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="9ae35-145">Toto omezení velikost fronty je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="9ae35-145">This upper limit on queue size is 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="9ae35-146">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="9ae35-146">Receive messages from a queue</span></span>

<span data-ttu-id="9ae35-147">Hello nejlepší způsob, jak tooreceive zprávy z fronty je toouse `ServiceBusRestProxy->receiveQueueMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="9ae35-147">hello best way tooreceive messages from a queue is toouse a `ServiceBusRestProxy->receiveQueueMessage` method.</span></span> <span data-ttu-id="9ae35-148">Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) a [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span><span class="sxs-lookup"><span data-stu-id="9ae35-148">Messages can be received in two different modes: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) and [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span></span> <span data-ttu-id="9ae35-149">**PeekLock** je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="9ae35-149">**PeekLock** is hello default.</span></span>

<span data-ttu-id="9ae35-150">Při použití [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) režimu přijímat je jednorázová operace; to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ae35-150">When using [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="9ae35-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="9ae35-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="9ae35-152">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="9ae35-152">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="9ae35-153">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="9ae35-153">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="9ae35-154">Ve výchozím nastavení hello [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) režimu, přijímání zprávy se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="9ae35-154">In hello default [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="9ae35-155">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent dalšími uživateli, příjem a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ae35-155">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers from receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="9ae35-156">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces příliš předáním hello přijata zpráva`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="9ae35-156">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="9ae35-157">Když Service Bus uvidí hello `deleteMessage` volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="9ae35-157">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="9ae35-158">Následující příklad ukazuje, jak Hello tooreceive a zpracovat zprávu pomocí [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) hello výchozí (režim).</span><span class="sxs-lookup"><span data-stu-id="9ae35-158">hello following example shows how tooreceive and process a message using [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode (hello default mode).</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set hello receive mode tooPeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="9ae35-159">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="9ae35-159">How toohandle application crashes and unreadable messages</span></span>

<span data-ttu-id="9ae35-160">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-160">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="9ae35-161">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` na hello přijal zprávu (místo hello `deleteMessage` metoda).</span><span class="sxs-lookup"><span data-stu-id="9ae35-161">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="9ae35-162">To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="9ae35-162">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="9ae35-163">Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="9ae35-163">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="9ae35-164">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="9ae35-164">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="9ae35-165">To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="9ae35-165">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="9ae35-166">Pokud hello scénář nemůže tolerovat zpracování duplicitní pak přidáte další se doporučuje logiku tooapplications toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="9ae35-166">If hello scenario cannot tolerate duplicate processing, then adding additional logic tooapplications toohandle duplicate message delivery is recommended.</span></span> <span data-ttu-id="9ae35-167">To se často opírá hello `getMessageId` metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="9ae35-167">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ae35-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ae35-168">Next steps</span></span>
<span data-ttu-id="9ae35-169">Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="9ae35-169">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="9ae35-170">Další informace najdete také navštívit hello [středisku pro vývojáře PHP](https://azure.microsoft.com/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="9ae35-170">For more information, also visit hello [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span></span>

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


