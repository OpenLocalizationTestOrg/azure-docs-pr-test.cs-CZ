---
title: "Jak používat fronty Service Bus s PHP | Microsoft Docs"
description: "Naučte se používat fronty Service Bus v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
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
ms.openlocfilehash: 3514812f7f087582035dad5d9a4d620652aa4da9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-php"></a><span data-ttu-id="d0d91-104">Jak používat fronty Service Bus s PHP</span><span class="sxs-lookup"><span data-stu-id="d0d91-104">How to use Service Bus queues with PHP</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="d0d91-105">Tento průvodce vám ukáže, jak používat fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="d0d91-105">This guide shows you how to use Service Bus queues.</span></span> <span data-ttu-id="d0d91-106">Ukázky jsou napsané v PHP a použití [Azure SDK pro jazyk PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d0d91-106">The samples are written in PHP and use the [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="d0d91-107">Pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="d0d91-107">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="d0d91-108">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="d0d91-108">Create a PHP application</span></span>
<span data-ttu-id="d0d91-109">Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob je odkazující na třídy v [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="d0d91-109">The only requirement for creating a PHP application that accesses the Azure Blob service is the referencing of classes in the [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="d0d91-110">Všechny nástroje pro vývoj vám pomůže vytvořit aplikaci nebo program Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="d0d91-110">You can use any development tools to create your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d91-111">Instalace PHP musí mít také [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.</span><span class="sxs-lookup"><span data-stu-id="d0d91-111">Your PHP installation must also have the [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="d0d91-112">V této příručce bude používat funkce služby, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="d0d91-112">In this guide, you will use service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="d0d91-113">Získání klienta Azure knihovny</span><span class="sxs-lookup"><span data-stu-id="d0d91-113">Get the Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="d0d91-114">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="d0d91-114">Configure your application to use Service Bus</span></span>
<span data-ttu-id="d0d91-115">Pokud chcete používat fronty Service Bus rozhraní API, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d0d91-115">To use the Service Bus queue APIs, do the following:</span></span>

1. <span data-ttu-id="d0d91-116">Reference souboru pomocí automatického zavaděče [require_once] [ require_once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="d0d91-116">Reference the autoloader file using the [require_once][require_once] statement.</span></span>
2. <span data-ttu-id="d0d91-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="d0d91-117">Reference any classes you might use.</span></span>

<span data-ttu-id="d0d91-118">Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-118">The following example shows how to include the autoloader file and reference the `ServicesBuilder` class.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d91-119">Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="d0d91-119">This example (and other examples in this article) assumes you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="d0d91-120">Pokud jste nainstalovali v knihovnách ručně nebo jako balíček HRUŠKAMI, musí odkazovat **WindowsAzure.php** automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="d0d91-120">If you installed the libraries manually or as a PEAR package, you must reference the **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="d0d91-121">V následujících příkladech `require_once` příkaz vždy se zobrazí, ale jenom ty třídy potřebné pro tento příklad provést odkazují.</span><span class="sxs-lookup"><span data-stu-id="d0d91-121">In the examples below, the `require_once` statement will always be shown, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="d0d91-122">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="d0d91-122">Set up a Service Bus connection</span></span>
<span data-ttu-id="d0d91-123">K vytvoření instance služby Service Bus klient, že máte platný připojovací řetězec v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="d0d91-123">To instantiate a Service Bus client, you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="d0d91-124">Kde `Endpoint` je obvykle ve formátu `[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="d0d91-124">Where `Endpoint` is typically of the format `[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="d0d91-125">Vytvoření libovolného klienta služby Azure, je nutné použít `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-125">To create any Azure service client you must use the `ServicesBuilder` class.</span></span> <span data-ttu-id="d0d91-126">Můžete:</span><span class="sxs-lookup"><span data-stu-id="d0d91-126">You can:</span></span>

* <span data-ttu-id="d0d91-127">Připojovací řetězec přímo jí předejte.</span><span class="sxs-lookup"><span data-stu-id="d0d91-127">Pass the connection string directly to it.</span></span>
* <span data-ttu-id="d0d91-128">Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="d0d91-128">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="d0d91-129">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="d0d91-129">By default it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="d0d91-130">Můžete přidat nové zdroje tím, že rozšíří `ConnectionStringSource` – třída</span><span class="sxs-lookup"><span data-stu-id="d0d91-130">You can add new sources by extending the `ConnectionStringSource` class</span></span>

<span data-ttu-id="d0d91-131">Příklady podle zde uvedeného je předaná přímo připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="d0d91-131">For the examples outlined here, the connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="d0d91-132">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="d0d91-132">Create a queue</span></span>
<span data-ttu-id="d0d91-133">Můžete provádět operace správy front služby Service Bus přes `ServiceBusRestProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-133">You can perform management operations for Service Bus queues via the `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="d0d91-134">A `ServiceBusRestProxy` objektu je vytvořený pomocí `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje tokenu oprávněními k její správě.</span><span class="sxs-lookup"><span data-stu-id="d0d91-134">A `ServiceBusRestProxy` object is constructed via the `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates the token permissions to manage it.</span></span>

<span data-ttu-id="d0d91-135">Následující příklad ukazuje, jak vytvořit instanci `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createQueue` vytvořit frontu s názvem `myqueue` v rámci `MySBNamespace` oboru názvů služby:</span><span class="sxs-lookup"><span data-stu-id="d0d91-135">The following example shows how to instantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createQueue` to create a queue named `myqueue` within a `MySBNamespace` service namespace:</span></span>

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
> <span data-ttu-id="d0d91-136">Můžete použít `listQueues` metodu `ServiceBusRestProxy` objekty, které chcete zkontrolovat, zda fronta se zadaným názvem již existuje v rámci oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d0d91-136">You can use the `listQueues` method on `ServiceBusRestProxy` objects to check if a queue with a specified name already exists within a namespace.</span></span>
> 
> 

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="d0d91-137">Zasílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="d0d91-137">Send messages to a queue</span></span>
<span data-ttu-id="d0d91-138">K odeslání zprávy do fronty Service Bus, vaše aplikace volání `ServiceBusRestProxy->sendQueueMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="d0d91-138">To send a message to a Service Bus queue, your application calls the `ServiceBusRestProxy->sendQueueMessage` method.</span></span> <span data-ttu-id="d0d91-139">Následující kód ukazuje, jak odeslat zprávu `myqueue` fronty vytvořili v rámci `MySBNamespace` oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="d0d91-139">The following code shows how to send a message to the `myqueue` queue previously created within the `MySBNamespace` service namespace.</span></span>

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

<span data-ttu-id="d0d91-140">Zprávy odeslané do (a přijaté z) sběrnice fronty jsou instance [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-140">Messages sent to (and received from ) Service Bus queues are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="d0d91-141">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních metod a vlastností, které jsou používané pro udržení vlastních vlastností aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="d0d91-141">[BrokeredMessage][BrokeredMessage] objects have a set of standard methods and properties that are used to hold custom application-specific properties, and a body of arbitrary application data.</span></span>

<span data-ttu-id="d0d91-142">Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="d0d91-142">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d0d91-143">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="d0d91-143">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d0d91-144">Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je.</span><span class="sxs-lookup"><span data-stu-id="d0d91-144">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="d0d91-145">Toto omezení velikost fronty je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d0d91-145">This upper limit on queue size is 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="d0d91-146">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="d0d91-146">Receive messages from a queue</span></span>

<span data-ttu-id="d0d91-147">Nejlepší způsob, jak přijmout zprávy z fronty je použití `ServiceBusRestProxy->receiveQueueMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="d0d91-147">The best way to receive messages from a queue is to use a `ServiceBusRestProxy->receiveQueueMessage` method.</span></span> <span data-ttu-id="d0d91-148">Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) a [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span><span class="sxs-lookup"><span data-stu-id="d0d91-148">Messages can be received in two different modes: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) and [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span></span> <span data-ttu-id="d0d91-149">Výchozí hodnota je **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="d0d91-149">**PeekLock** is the default.</span></span>

<span data-ttu-id="d0d91-150">Při použití [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) režimu přijímat je jednorázová operace; to znamená, když Service Bus přijme požadavek čtení zprávy ve frontě, označí zprávu jako spotřebovávanou a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0d91-150">When using [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a queue, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="d0d91-151">Režim [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat možnost, že v případě selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="d0d91-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="d0d91-152">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="d0d91-152">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="d0d91-153">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="d0d91-153">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="d0d91-154">Ve výchozím [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) režimu, přijímání zprávy se změní na dvě fáze operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="d0d91-154">In the default [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode, receiving a message becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d0d91-155">Když Service Bus přijme požadavek, najde zprávu, který se má používat, uzamkne ji proti spotřebování jinými odběrateli příjem ho a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0d91-155">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers from receiving it, and then returns it to the application.</span></span> <span data-ttu-id="d0d91-156">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí předávání přijaté zprávy do `ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="d0d91-156">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by passing the received message to `ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="d0d91-157">Když Service Bus uvidí `deleteMessage` volání, která se bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="d0d91-157">When Service Bus sees the `deleteMessage` call, it will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="d0d91-158">Následující příklad ukazuje, jak přijímat a zpracovávat zprávu pomocí [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) režimu (výchozím režimu).</span><span class="sxs-lookup"><span data-stu-id="d0d91-158">The following example shows how to receive and process a message using [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode (the default mode).</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d0d91-159">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="d0d91-159">How to handle application crashes and unreadable messages</span></span>

<span data-ttu-id="d0d91-160">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-160">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d0d91-161">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlockMessage` metoda na přijatou zprávu (místo `deleteMessage` metoda).</span><span class="sxs-lookup"><span data-stu-id="d0d91-161">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the received message (instead of the `deleteMessage` method).</span></span> <span data-ttu-id="d0d91-162">To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="d0d91-162">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d0d91-163">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak se Service Bus zprávu automaticky odemkne a zpřístupní ji pro další přijetí.</span><span class="sxs-lookup"><span data-stu-id="d0d91-163">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="d0d91-164">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `deleteMessage` požadavku a potom zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="d0d91-164">In the event that the application crashes after processing the message but before the `deleteMessage` request is issued, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="d0d91-165">To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou ale v některých situacích může být stejná zpráva víckrát.</span><span class="sxs-lookup"><span data-stu-id="d0d91-165">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="d0d91-166">Pokud scénář nemůže tolerovat zpracování duplicitní, se doporučuje následným přidáním další logiku pro aplikace pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="d0d91-166">If the scenario cannot tolerate duplicate processing, then adding additional logic to applications to handle duplicate message delivery is recommended.</span></span> <span data-ttu-id="d0d91-167">To se často opírá `getMessageId` metoda zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="d0d91-167">This is often achieved using the `getMessageId` method of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0d91-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0d91-168">Next steps</span></span>
<span data-ttu-id="d0d91-169">Teď, když jste se naučili základy front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="d0d91-169">Now that you've learned the basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="d0d91-170">Další informace najdete také naleznete [středisku pro vývojáře PHP](https://azure.microsoft.com/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="d0d91-170">For more information, also visit the [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span></span>

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


