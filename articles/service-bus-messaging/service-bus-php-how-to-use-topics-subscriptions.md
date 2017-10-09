---
title: "témata Service Bus toouse aaaHow s PHP | Microsoft Docs"
description: "Zjistěte, jak toouse témat sběrnice Service Bus s PHP v Azure."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: 0ca8625fa3edc5854c0d6c1c2f6adab6a2d42f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a><span data-ttu-id="2b37a-103">Jak toouse Service Bus témata a odběry s PHP</span><span class="sxs-lookup"><span data-stu-id="2b37a-103">How toouse Service Bus topics and subscriptions with PHP</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="2b37a-104">Tento článek ukazuje, jak toouse Service Bus témat a odběrů.</span><span class="sxs-lookup"><span data-stu-id="2b37a-104">This article shows you how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="2b37a-105">Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="2b37a-105">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="2b37a-106">Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv tooa tématu**, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="2b37a-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="2b37a-107">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="2b37a-107">Create a PHP application</span></span>
<span data-ttu-id="2b37a-108">Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob hello je tooreference třídy v hello [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-108">hello only requirement for creating a PHP application that accesses hello Azure Blob service is tooreference classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="2b37a-109">Můžete všechny toocreate nástroje pro vývoj aplikací nebo Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="2b37a-109">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="2b37a-110">Instalace PHP musí mít také hello [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.</span><span class="sxs-lookup"><span data-stu-id="2b37a-110">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="2b37a-111">Tento článek popisuje, jak toouse služby funkce, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-111">This article describes how toouse service features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="2b37a-112">Získání knihovny klienta Azure hello</span><span class="sxs-lookup"><span data-stu-id="2b37a-112">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="2b37a-113">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="2b37a-113">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="2b37a-114">toouse hello API pro Service Bus:</span><span class="sxs-lookup"><span data-stu-id="2b37a-114">toouse hello Service Bus APIs:</span></span>

1. <span data-ttu-id="2b37a-115">Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require-once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="2b37a-115">Reference hello autoloader file using hello [require_once][require-once] statement.</span></span>
2. <span data-ttu-id="2b37a-116">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="2b37a-116">Reference any classes you might use.</span></span>

<span data-ttu-id="2b37a-117">Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServiceBusService** třídy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-117">hello following example shows how tooinclude hello autoloader file and reference hello **ServiceBusService** class.</span></span>

> [!NOTE]
> <span data-ttu-id="2b37a-118">Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="2b37a-118">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="2b37a-119">Pokud jste nainstalovali hello knihovny ručně nebo jako balíček HRUŠKAMI, musíte odkázat hello **WindowsAzure.php** automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="2b37a-119">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="2b37a-120">V následující příklady hello, hello `require_once` příkaz vždy se zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.</span><span class="sxs-lookup"><span data-stu-id="2b37a-120">In hello following examples, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="2b37a-121">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="2b37a-121">Set up a Service Bus connection</span></span>
<span data-ttu-id="2b37a-122">tooinstantiate Service Bus klienta, musíte nejdřív mají platný připojovací řetězec v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="2b37a-122">tooinstantiate a Service Bus client you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="2b37a-123">Kde `Endpoint` je obvykle ve formátu hello `https://[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="2b37a-123">Where `Endpoint` is typically of hello format `https://[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="2b37a-124">toocreate libovolného klienta služby Azure, je nutné použít hello `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-124">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="2b37a-125">Můžete:</span><span class="sxs-lookup"><span data-stu-id="2b37a-125">You can:</span></span>

* <span data-ttu-id="2b37a-126">Předat hello připojovací řetězec přímo tooit.</span><span class="sxs-lookup"><span data-stu-id="2b37a-126">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="2b37a-127">Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="2b37a-127">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="2b37a-128">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2b37a-128">By default it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="2b37a-129">Můžete přidat nové zdroje rozšířením hello `ConnectionStringSource` třídy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-129">You can add new sources by extending hello `ConnectionStringSource` class.</span></span>

<span data-ttu-id="2b37a-130">Zde uvedené příklady hello je předaná přímo hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2b37a-130">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a><span data-ttu-id="2b37a-131">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="2b37a-131">Create a topic</span></span>
<span data-ttu-id="2b37a-132">Můžete provádět operace správy témat sběrnice Service Bus přes hello `ServiceBusRestProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-132">You can perform management operations for Service Bus topics via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="2b37a-133">A `ServiceBusRestProxy` je objekt vytvořený prostřednictvím hello `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje hello tokenu oprávnění toomanage ho.</span><span class="sxs-lookup"><span data-stu-id="2b37a-133">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="2b37a-134">Následující příklad ukazuje, jak Hello tooinstantiate `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createTopic` toocreate téma s názvem `mytopic` v rámci `MySBNamespace` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="2b37a-134">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createTopic` toocreate a topic named `mytopic` within a `MySBNamespace` namespace:</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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
> <span data-ttu-id="2b37a-135">Můžete použít hello `listTopics` metodu `ServiceBusRestProxy` objekty toocheck Pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="2b37a-135">You can use hello `listTopics` method on `ServiceBusRestProxy` objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>
> 
> 

## <a name="create-a-subscription"></a><span data-ttu-id="2b37a-136">Vytvoření odběru</span><span class="sxs-lookup"><span data-stu-id="2b37a-136">Create a subscription</span></span>
<span data-ttu-id="2b37a-137">Odběry témat taky jsou vytvořeny pomocí hello `ServiceBusRestProxy->createSubscription` metoda.</span><span class="sxs-lookup"><span data-stu-id="2b37a-137">Topic subscriptions are also created with hello `ServiceBusRestProxy->createSubscription` method.</span></span> <span data-ttu-id="2b37a-138">Odběry mají názvy a můžou mít volitelné filtry, které omezují skupinu zpráv předávaných virtuální fronty odběru toohello hello.</span><span class="sxs-lookup"><span data-stu-id="2b37a-138">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="2b37a-139">Vytvoření odběru s filtrem (MatchAll) výchozí hello</span><span class="sxs-lookup"><span data-stu-id="2b37a-139">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="2b37a-140">Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="2b37a-140">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="2b37a-141">Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru hello.</span><span class="sxs-lookup"><span data-stu-id="2b37a-141">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="2b37a-142">Hello následující příklad vytvoří odběr s názvem 'mysubscription' a používá hello výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="2b37a-142">hello following example creates a subscription named 'mysubscription' and uses hello default **MatchAll** filter.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
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

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="2b37a-143">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="2b37a-143">Create subscriptions with filters</span></span>
<span data-ttu-id="2b37a-144">Můžete také nastavit filtry, které umožňují toospecify příjem zpráv odeslaných tooa tématu by měl být použit v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-144">You can also set up filters that enable you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span> <span data-ttu-id="2b37a-145">Hello nejflexibilnější filtr, který odběry podporují je hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="2b37a-145">hello most flexible type of filter supported by subscriptions is hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), which implements a subset of SQL92.</span></span> <span data-ttu-id="2b37a-146">Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-146">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="2b37a-147">Další informace o SqlFilters najdete v tématu [SqlFilter.SqlExpression vlastnost][sqlfilter].</span><span class="sxs-lookup"><span data-stu-id="2b37a-147">For more information about SqlFilters, see [SqlFilter.SqlExpression Property][sqlfilter].</span></span>

> [!NOTE]
> <span data-ttu-id="2b37a-148">Každé pravidlo na předplatném zpracuje příchozí zprávy nezávisle, přidání svoje předplatné toohello výsledek zprávy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-148">Each rule on a subscription processes incoming messages independently, adding their result messages toohello subscription.</span></span> <span data-ttu-id="2b37a-149">Kromě toho má každý nový odběr výchozí **pravidlo** objekt s filtr, který přidá všechny zprávy z odběru toohello tématu hello.</span><span class="sxs-lookup"><span data-stu-id="2b37a-149">In addition, each new subscription has a default **Rule** object with a filter that adds all messages from hello topic toohello subscription.</span></span> <span data-ttu-id="2b37a-150">tooreceive pouze zprávy odpovídající vašemu filtru, musíte odebrat hello výchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2b37a-150">tooreceive only messages matching your filter, you must remove hello default rule.</span></span> <span data-ttu-id="2b37a-151">Hello výchozí pravidla můžete odebrat pomocí hello `ServiceBusRestProxy->deleteRule` metoda.</span><span class="sxs-lookup"><span data-stu-id="2b37a-151">You can remove hello default rule by using hello `ServiceBusRestProxy->deleteRule` method.</span></span>
> 
> 

<span data-ttu-id="2b37a-152">Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `MessageNumber` vlastnost větší než 3.</span><span class="sxs-lookup"><span data-stu-id="2b37a-152">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `MessageNumber` property greater than 3.</span></span> <span data-ttu-id="2b37a-153">V tématu [odeslání zprávy tooa tématu](#send-messages-to-a-topic) informace o přidání vlastních vlastností toomessages.</span><span class="sxs-lookup"><span data-stu-id="2b37a-153">See [Send messages tooa topic](#send-messages-to-a-topic) for information about adding custom properties toomessages.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

<span data-ttu-id="2b37a-154">Všimněte si, že tento kód vyžaduje použití hello další oboru názvů: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span><span class="sxs-lookup"><span data-stu-id="2b37a-154">Note that this code requires hello use of an additional namespace: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span></span>

<span data-ttu-id="2b37a-155">Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s `SqlFilter` který vybere jen zprávy, které mají `MessageNumber` vlastnost menší než nebo rovna too3.</span><span class="sxs-lookup"><span data-stu-id="2b37a-155">Similarly, hello following example creates a subscription named `LowMessages` with a `SqlFilter` that only selects messages that have a `MessageNumber` property less than or equal too3.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

<span data-ttu-id="2b37a-156">Teď, když je odeslána zpráva toohello `mytopic` tématu, vždy se dodá tooreceivers odběru toohello `mysubscription` předplatného a selektivně tooreceivers odběru toohello `HighMessages` a `LowMessages` (odběrů v závislosti na obsahu zprávy hello).</span><span class="sxs-lookup"><span data-stu-id="2b37a-156">Now, when a message is sent toohello `mytopic` topic, it is always delivered tooreceivers subscribed toohello `mysubscription` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="2b37a-157">Odeslání zprávy tooa tématu</span><span class="sxs-lookup"><span data-stu-id="2b37a-157">Send messages tooa topic</span></span>
<span data-ttu-id="2b37a-158">toosend tématu Service Bus zprávu tooa aplikace volá hello `ServiceBusRestProxy->sendTopicMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="2b37a-158">toosend a message tooa Service Bus topic, your application calls hello `ServiceBusRestProxy->sendTopicMessage` method.</span></span> <span data-ttu-id="2b37a-159">Hello následující kód ukazuje, jak toosend zpráva toohello `mytopic` vytvořili v tématu `MySBNamespace` oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="2b37a-159">hello following code shows how toosend a message toohello `mytopic` topic previously created within the `MySBNamespace` service namespace.</span></span>

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
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

<span data-ttu-id="2b37a-160">Zprávy odeslané témata tooService Bus jsou instance třídy hello [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-160">Messages sent tooService Bus topics are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="2b37a-161">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností a metod, jakož i vlastnosti, které se dají použít toohold vlastní vlastnosti specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2b37a-161">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties and methods, as well as properties that can be used toohold custom application-specific properties.</span></span> <span data-ttu-id="2b37a-162">Hello následující příklad ukazuje, jak testovací toosend 5 zprávy toohello `mytopic` tématu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2b37a-162">hello following example shows how toosend 5 test messages toohello `mytopic` topic previously created.</span></span> <span data-ttu-id="2b37a-163">Hello `setProperty` metoda je použité tooadd vlastní vlastnosti (`MessageNumber`) tooeach zprávy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-163">hello `setProperty` method is used tooadd a custom property (`MessageNumber`) tooeach message.</span></span> <span data-ttu-id="2b37a-164">Všimněte si, že hello `MessageNumber` vlastnost hodnota se liší u každé zprávy (můžete použít tuto hodnotu toodetermine, které odběry ji, přijmou, jak je znázorněno v hello [vytvořit odběr](#create-a-subscription) část):</span><span class="sxs-lookup"><span data-stu-id="2b37a-164">Note that hello `MessageNumber` property value varies on each message (you can use this value toodetermine which subscriptions receive it, as shown in hello [Create a subscription](#create-a-subscription) section):</span></span>

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

<span data-ttu-id="2b37a-165">Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="2b37a-165">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="2b37a-166">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="2b37a-166">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="2b37a-167">Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-167">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="2b37a-168">Toto omezení velikost tématu je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="2b37a-168">This upper limit on topic size is 5 GB.</span></span> <span data-ttu-id="2b37a-169">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="2b37a-169">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="2b37a-170">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="2b37a-170">Receive messages from a subscription</span></span>
<span data-ttu-id="2b37a-171">Hello nejlepší způsob, jak tooreceive zprávy z odběru je toouse `ServiceBusRestProxy->receiveSubscriptionMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="2b37a-171">hello best way tooreceive messages from a subscription is toouse a `ServiceBusRestProxy->receiveSubscriptionMessage` method.</span></span> <span data-ttu-id="2b37a-172">Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* a *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span><span class="sxs-lookup"><span data-stu-id="2b37a-172">Messages can be received in two different modes: [*ReceiveAndDelete* and *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span></span> <span data-ttu-id="2b37a-173">**PeekLock** je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="2b37a-173">**PeekLock** is hello default.</span></span>

<span data-ttu-id="2b37a-174">Při použití hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu přijímat je jednorázová operace; to znamená, když Service Bus přijme požadavek čtení zprávy v odběru, označí uvítací zprávu jako spotřebovávanou a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b37a-174">When using hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a subscription, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="2b37a-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * režimu je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="2b37a-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="2b37a-176">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="2b37a-176">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="2b37a-177">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="2b37a-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="2b37a-178">Ve výchozím nastavení hello [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu, přijímání zprávy se změní na dvě fáze operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="2b37a-178">In hello default [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="2b37a-179">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b37a-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="2b37a-180">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces příliš předáním hello přijata zpráva`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="2b37a-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="2b37a-181">Když Service Bus uvidí hello `deleteMessage` volání, která se bude označit uvítací zprávu jako spotřebovávanou a odeberte ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="2b37a-181">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="2b37a-182">Následující příklad ukazuje, jak Hello tooreceive a zpracovat zprávu pomocí [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) hello výchozí (režim).</span><span class="sxs-lookup"><span data-stu-id="2b37a-182">hello following example shows how tooreceive and process a message using [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode (hello default mode).</span></span> 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode tooPeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="2b37a-183">Postupy: zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="2b37a-183">How to: handle application crashes and unreadable messages</span></span>
<span data-ttu-id="2b37a-184">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-184">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="2b37a-185">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` na hello přijal zprávu (místo hello `deleteMessage` metoda).</span><span class="sxs-lookup"><span data-stu-id="2b37a-185">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="2b37a-186">To bude způsobit, že Service Bus toounlock uvítací zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="2b37a-186">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="2b37a-187">Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak Service Bus odemknutím uvítací zprávu automaticky a nastavit jej jako dostupné toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="2b37a-187">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="2b37a-188">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` požadavku a potom uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-188">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="2b37a-189">To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou, ale v některých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="2b37a-189">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="2b37a-190">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tooapplications toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="2b37a-190">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tooapplications toohandle duplicate message delivery.</span></span> <span data-ttu-id="2b37a-191">To se často opírá hello `getMessageId` metoda hello zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="2b37a-191">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="2b37a-192">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="2b37a-192">Delete topics and subscriptions</span></span>
<span data-ttu-id="2b37a-193">toodelete a tématu nebo předplatného, použijte hello `ServiceBusRestProxy->deleteTopic` nebo hello `ServiceBusRestProxy->deleteSubscripton` metody, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2b37a-193">toodelete a topic or a subscription, use hello `ServiceBusRestProxy->deleteTopic` or hello `ServiceBusRestProxy->deleteSubscripton` methods, respectively.</span></span> <span data-ttu-id="2b37a-194">Všimněte si, že se odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.</span><span class="sxs-lookup"><span data-stu-id="2b37a-194">Note that deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span>

<span data-ttu-id="2b37a-195">Hello následující příklad ukazuje, jak toodelete téma s názvem `mytopic` a jeho registrované odběry.</span><span class="sxs-lookup"><span data-stu-id="2b37a-195">hello following example shows how toodelete a topic named `mytopic` and its registered subscriptions.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
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

<span data-ttu-id="2b37a-196">Pomocí hello `deleteSubscription` metodu, můžete odstranit odběr nezávisle:</span><span class="sxs-lookup"><span data-stu-id="2b37a-196">By using hello `deleteSubscription` method, you can delete a subscription independently:</span></span>

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a><span data-ttu-id="2b37a-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b37a-197">Next steps</span></span>
<span data-ttu-id="2b37a-198">Teď, když jste se naučili základy hello front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="2b37a-198">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
