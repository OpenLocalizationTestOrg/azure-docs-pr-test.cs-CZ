---
title: "Jak používat témata Service Bus s PHP | Microsoft Docs"
description: "Naučte se používat témata Service Bus s PHP v Azure."
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
ms.openlocfilehash: afa9efcb6335786198021ec81dd087287c39bda9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-php"></a><span data-ttu-id="1d322-103">Jak používat témata a odběry Service Bus s PHP</span><span class="sxs-lookup"><span data-stu-id="1d322-103">How to use Service Bus topics and subscriptions with PHP</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="1d322-104">Tento článek ukazuje, jak používat témata a odběry Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1d322-104">This article shows you how to use Service Bus topics and subscriptions.</span></span> <span data-ttu-id="1d322-105">Ukázky jsou napsané v PHP a použití [Azure SDK pro jazyk PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="1d322-105">The samples are written in PHP and use the [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="1d322-106">Pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv do tématu**, **přijímání zpráv z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="1d322-106">The scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages to a topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="1d322-107">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="1d322-107">Create a PHP application</span></span>
<span data-ttu-id="1d322-108">Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Blob je referenční třídy v [Azure SDK pro jazyk PHP](../php-download-sdk.md) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="1d322-108">The only requirement for creating a PHP application that accesses the Azure Blob service is to reference classes in the [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="1d322-109">Všechny nástroje pro vývoj vám pomůže vytvořit aplikaci nebo program Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="1d322-109">You can use any development tools to create your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="1d322-110">Instalace PHP musí mít také [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.</span><span class="sxs-lookup"><span data-stu-id="1d322-110">Your PHP installation must also have the [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="1d322-111">Tento článek popisuje, jak používat funkce služby, které může být volána v rámci aplikace PHP místně nebo v kódu běžící v rámci webu, role pracovního procesu nebo webové role Azure.</span><span class="sxs-lookup"><span data-stu-id="1d322-111">This article describes how to use service features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="1d322-112">Získání klienta Azure knihovny</span><span class="sxs-lookup"><span data-stu-id="1d322-112">Get the Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="1d322-113">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="1d322-113">Configure your application to use Service Bus</span></span>
<span data-ttu-id="1d322-114">Použití API pro Service Bus:</span><span class="sxs-lookup"><span data-stu-id="1d322-114">To use the Service Bus APIs:</span></span>

1. <span data-ttu-id="1d322-115">Reference souboru pomocí automatického zavaděče [require_once] [ require-once] příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d322-115">Reference the autoloader file using the [require_once][require-once] statement.</span></span>
2. <span data-ttu-id="1d322-116">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="1d322-116">Reference any classes you might use.</span></span>

<span data-ttu-id="1d322-117">Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServiceBusService** třídy.</span><span class="sxs-lookup"><span data-stu-id="1d322-117">The following example shows how to include the autoloader file and reference the **ServiceBusService** class.</span></span>

> [!NOTE]
> <span data-ttu-id="1d322-118">Tento příklad (a další příklady v tomto článku) předpokládá, že jste nainstalovali PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="1d322-118">This example (and other examples in this article) assumes you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="1d322-119">Pokud jste nainstalovali v knihovnách ručně nebo jako balíček HRUŠKAMI, musí odkazovat **WindowsAzure.php** automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="1d322-119">If you installed the libraries manually or as a PEAR package, you must reference the **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="1d322-120">V následujících příkladech `require_once` příkaz vždy se zobrazí, ale jenom ty třídy potřebné pro tento příklad provést odkazují.</span><span class="sxs-lookup"><span data-stu-id="1d322-120">In the following examples, the `require_once` statement will always be shown, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="1d322-121">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="1d322-121">Set up a Service Bus connection</span></span>
<span data-ttu-id="1d322-122">K vytvoření instance služby Service Bus klient Nejdřív musíte mít platný připojovací řetězec v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="1d322-122">To instantiate a Service Bus client you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="1d322-123">Kde `Endpoint` je obvykle ve formátu `https://[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="1d322-123">Where `Endpoint` is typically of the format `https://[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="1d322-124">Vytvoření libovolného klienta služby Azure, je nutné použít `ServicesBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="1d322-124">To create any Azure service client you must use the `ServicesBuilder` class.</span></span> <span data-ttu-id="1d322-125">Můžete:</span><span class="sxs-lookup"><span data-stu-id="1d322-125">You can:</span></span>

* <span data-ttu-id="1d322-126">Připojovací řetězec přímo jí předejte.</span><span class="sxs-lookup"><span data-stu-id="1d322-126">Pass the connection string directly to it.</span></span>
* <span data-ttu-id="1d322-127">Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="1d322-127">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="1d322-128">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d322-128">By default it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="1d322-129">Můžete přidat nové zdroje tím, že rozšíří `ConnectionStringSource` třídy.</span><span class="sxs-lookup"><span data-stu-id="1d322-129">You can add new sources by extending the `ConnectionStringSource` class.</span></span>

<span data-ttu-id="1d322-130">Příklady podle zde uvedeného je předaná přímo připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1d322-130">For the examples outlined here, the connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a><span data-ttu-id="1d322-131">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="1d322-131">Create a topic</span></span>
<span data-ttu-id="1d322-132">Můžete provádět operace správy témat sběrnice Service Bus přes `ServiceBusRestProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="1d322-132">You can perform management operations for Service Bus topics via the `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="1d322-133">A `ServiceBusRestProxy` objektu je vytvořený pomocí `ServicesBuilder::createServiceBusService` metoda factory řetězcem odpovídající připojení, který zapouzdřuje tokenu oprávněními k její správě.</span><span class="sxs-lookup"><span data-stu-id="1d322-133">A `ServiceBusRestProxy` object is constructed via the `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates the token permissions to manage it.</span></span>

<span data-ttu-id="1d322-134">Následující příklad ukazuje, jak vytvořit instanci `ServiceBusRestProxy` a volání `ServiceBusRestProxy->createTopic` vytvořit téma s názvem `mytopic` v rámci `MySBNamespace` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="1d322-134">The following example shows how to instantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createTopic` to create a topic named `mytopic` within a `MySBNamespace` namespace:</span></span>

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
> <span data-ttu-id="1d322-135">Můžete použít `listTopics` metodu `ServiceBusRestProxy` objekty, které chcete zkontrolovat, pokud téma se zadaným názvem již existuje v rámci oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="1d322-135">You can use the `listTopics` method on `ServiceBusRestProxy` objects to check if a topic with a specified name already exists within a service namespace.</span></span>
> 
> 

## <a name="create-a-subscription"></a><span data-ttu-id="1d322-136">Vytvoření odběru</span><span class="sxs-lookup"><span data-stu-id="1d322-136">Create a subscription</span></span>
<span data-ttu-id="1d322-137">Odběry témat taky jsou vytvořeny pomocí `ServiceBusRestProxy->createSubscription` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d322-137">Topic subscriptions are also created with the `ServiceBusRestProxy->createSubscription` method.</span></span> <span data-ttu-id="1d322-138">Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv odesílaných do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="1d322-138">Subscriptions are named and can have an optional filter that restricts the set of messages passed to the subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="1d322-139">Vytvoření odběru s výchozím filtrem (MatchAll).</span><span class="sxs-lookup"><span data-stu-id="1d322-139">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="1d322-140">Filtr **MatchAll** je výchozí filtr, který se použije v případě, že při vytváření nového odběru nezadáte žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="1d322-140">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="1d322-141">Když **MatchAll** filtr se používá, všechny zprávy publikované do tématu jsou umístěny do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="1d322-141">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="1d322-142">Následující příklad vytvoří odběr s názvem 'mysubscription' a používá výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="1d322-142">The following example creates a subscription named 'mysubscription' and uses the default **MatchAll** filter.</span></span>

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

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="1d322-143">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="1d322-143">Create subscriptions with filters</span></span>
<span data-ttu-id="1d322-144">Můžete taky vytvořit filtry, které vám umožní zprávy odeslané do tématu zobrazit v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="1d322-144">You can also set up filters that enable you to specify which messages sent to a topic should appear within a specific topic subscription.</span></span> <span data-ttu-id="1d322-145">Nejflexibilnější filtr, který odběry podporují je [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="1d322-145">The most flexible type of filter supported by subscriptions is the [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), which implements a subset of SQL92.</span></span> <span data-ttu-id="1d322-146">Filtry SQL pracují s vlastnostmi zpráv publikované do tématu.</span><span class="sxs-lookup"><span data-stu-id="1d322-146">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="1d322-147">Další informace o SqlFilters najdete v tématu [SqlFilter.SqlExpression vlastnost][sqlfilter].</span><span class="sxs-lookup"><span data-stu-id="1d322-147">For more information about SqlFilters, see [SqlFilter.SqlExpression Property][sqlfilter].</span></span>

> [!NOTE]
> <span data-ttu-id="1d322-148">Každé pravidlo na předplatném zpracuje příchozí zprávy nezávisle, přidávání jejich výsledek zprávy k předplatnému.</span><span class="sxs-lookup"><span data-stu-id="1d322-148">Each rule on a subscription processes incoming messages independently, adding their result messages to the subscription.</span></span> <span data-ttu-id="1d322-149">Kromě toho má každý nový odběr výchozí **pravidlo** objekt s filtr, který přidá všechny zprávy z tématu k předplatnému.</span><span class="sxs-lookup"><span data-stu-id="1d322-149">In addition, each new subscription has a default **Rule** object with a filter that adds all messages from the topic to the subscription.</span></span> <span data-ttu-id="1d322-150">Pokud chcete přijímat pouze zprávy odpovídající vašemu filtru, musíte odebrat výchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1d322-150">To receive only messages matching your filter, you must remove the default rule.</span></span> <span data-ttu-id="1d322-151">Výchozí pravidla můžete odebrat pomocí `ServiceBusRestProxy->deleteRule` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d322-151">You can remove the default rule by using the `ServiceBusRestProxy->deleteRule` method.</span></span>
> 
> 

<span data-ttu-id="1d322-152">Následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `MessageNumber` vlastnost větší než 3.</span><span class="sxs-lookup"><span data-stu-id="1d322-152">The following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `MessageNumber` property greater than 3.</span></span> <span data-ttu-id="1d322-153">V tématu [odeslání zprávy do tématu](#send-messages-to-a-topic) informace o přidání vlastních vlastností do zprávy.</span><span class="sxs-lookup"><span data-stu-id="1d322-153">See [Send messages to a topic](#send-messages-to-a-topic) for information about adding custom properties to messages.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

<span data-ttu-id="1d322-154">Všimněte si, že tento kód vyžaduje použití další oboru názvů: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span><span class="sxs-lookup"><span data-stu-id="1d322-154">Note that this code requires the use of an additional namespace: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span></span>

<span data-ttu-id="1d322-155">Podobně platí, tento příklad vytvoří odběr s názvem `LowMessages` s `SqlFilter` který vybere jen zprávy, které mají `MessageNumber` vlastnost menší než nebo rovné 3.</span><span class="sxs-lookup"><span data-stu-id="1d322-155">Similarly, the following example creates a subscription named `LowMessages` with a `SqlFilter` that only selects messages that have a `MessageNumber` property less than or equal to 3.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

<span data-ttu-id="1d322-156">Teď, když je odeslána zpráva `mytopic` tématu, vždy se dodá příjemci `mysubscription` předplatného a selektivně příjemcům přihlásit k odběru `HighMessages` a `LowMessages` odběry (v závislosti Při obsahu zprávy).</span><span class="sxs-lookup"><span data-stu-id="1d322-156">Now, when a message is sent to the `mytopic` topic, it is always delivered to receivers subscribed to the `mysubscription` subscription, and selectively delivered to receivers subscribed to the `HighMessages` and `LowMessages` subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="1d322-157">Odeslání zprávy do tématu</span><span class="sxs-lookup"><span data-stu-id="1d322-157">Send messages to a topic</span></span>
<span data-ttu-id="1d322-158">K odeslání zprávy do tématu Service Bus, vaše aplikace volání `ServiceBusRestProxy->sendTopicMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d322-158">To send a message to a Service Bus topic, your application calls the `ServiceBusRestProxy->sendTopicMessage` method.</span></span> <span data-ttu-id="1d322-159">Následující kód ukazuje, jak odeslat zprávu `mytopic` vytvořili v tématu `MySBNamespace` oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="1d322-159">The following code shows how to send a message to the `mytopic` topic previously created within the `MySBNamespace` service namespace.</span></span>

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

<span data-ttu-id="1d322-160">Zprávy odeslané do témat Service Bus jsou instance [BrokeredMessage] [ BrokeredMessage] třídy.</span><span class="sxs-lookup"><span data-stu-id="1d322-160">Messages sent to Service Bus topics are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="1d322-161">[BrokeredMessage] [ BrokeredMessage] objekty mají sadu standardních vlastností a metod, jakož i vlastnosti, které lze použít pro udržení vlastních vlastností specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d322-161">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties and methods, as well as properties that can be used to hold custom application-specific properties.</span></span> <span data-ttu-id="1d322-162">Následující příklad ukazuje, jak odeslat 5 zkušebních zpráv do `mytopic` tématu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1d322-162">The following example shows how to send 5 test messages to the `mytopic` topic previously created.</span></span> <span data-ttu-id="1d322-163">`setProperty` Metoda se používá k přidání vlastní vlastnosti (`MessageNumber`) pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1d322-163">The `setProperty` method is used to add a custom property (`MessageNumber`) to each message.</span></span> <span data-ttu-id="1d322-164">Všimněte si, že `MessageNumber` vlastnost hodnota se liší u každé zprávy (Tato hodnota slouží k určení, které odběry dostávat, jak je znázorněno v [vytvořit odběr](#create-a-subscription) část):</span><span class="sxs-lookup"><span data-stu-id="1d322-164">Note that the `MessageNumber` property value varies on each message (you can use this value to determine which subscriptions receive it, as shown in the [Create a subscription](#create-a-subscription) section):</span></span>

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

<span data-ttu-id="1d322-165">Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="1d322-165">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="1d322-166">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="1d322-166">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="1d322-167">Počet zpráv držených v tématu není omezený, ale celková velikost zpráv držených v tématu omezená je.</span><span class="sxs-lookup"><span data-stu-id="1d322-167">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="1d322-168">Toto omezení velikost tématu je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="1d322-168">This upper limit on topic size is 5 GB.</span></span> <span data-ttu-id="1d322-169">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="1d322-169">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="1d322-170">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="1d322-170">Receive messages from a subscription</span></span>
<span data-ttu-id="1d322-171">Nejlepší způsob, jak přijmout zprávy z odběru je použití `ServiceBusRestProxy->receiveSubscriptionMessage` metoda.</span><span class="sxs-lookup"><span data-stu-id="1d322-171">The best way to receive messages from a subscription is to use a `ServiceBusRestProxy->receiveSubscriptionMessage` method.</span></span> <span data-ttu-id="1d322-172">Můžete obdržet zprávy ve dvou různých režimech: [ *ReceiveAndDelete* a *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span><span class="sxs-lookup"><span data-stu-id="1d322-172">Messages can be received in two different modes: [*ReceiveAndDelete* and *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span></span> <span data-ttu-id="1d322-173">Výchozí hodnota je **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="1d322-173">**PeekLock** is the default.</span></span>

<span data-ttu-id="1d322-174">Při použití režimu [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) je přijetí jednorázová operace – tzn. když Service Bus přijme požadavek na čtení zprávy v odběru, označí zprávu jako spotřebovávanou a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d322-174">When using the [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a subscription, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="1d322-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * režimu je nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="1d322-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mode is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="1d322-176">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="1d322-176">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="1d322-177">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="1d322-177">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="1d322-178">Ve výchozím [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu, přijímání zprávy se změní na dvě fáze operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="1d322-178">In the default [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receiving a message becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="1d322-179">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d322-179">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="1d322-180">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí předávání přijaté zprávy do `ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="1d322-180">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by passing the received message to `ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="1d322-181">Když Service Bus uvidí `deleteMessage` volání, která se bude označí zprávu jako spotřebovávanou a odebrat ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="1d322-181">When Service Bus sees the `deleteMessage` call, it will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="1d322-182">Následující příklad ukazuje, jak přijímat a zpracovávat zprávu pomocí [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu (výchozím režimu).</span><span class="sxs-lookup"><span data-stu-id="1d322-182">The following example shows how to receive and process a message using [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode (the default mode).</span></span> 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="1d322-183">Postupy: zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="1d322-183">How to: handle application crashes and unreadable messages</span></span>
<span data-ttu-id="1d322-184">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="1d322-184">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="1d322-185">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlockMessage` metoda na přijatou zprávu (místo `deleteMessage` metoda).</span><span class="sxs-lookup"><span data-stu-id="1d322-185">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the received message (instead of the `deleteMessage` method).</span></span> <span data-ttu-id="1d322-186">To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="1d322-186">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="1d322-187">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak se Service Bus zprávu automaticky odemkne a zpřístupní ji pro další přijetí.</span><span class="sxs-lookup"><span data-stu-id="1d322-187">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="1d322-188">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `deleteMessage` požadavku a potom zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="1d322-188">In the event that the application crashes after processing the message but before the `deleteMessage` request is issued, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="1d322-189">To se často označuje jako *nejméně jednou* zpracování; to znamená, že každá zpráva se zpracuje alespoň jednou ale v některých situacích může být stejná zpráva víckrát.</span><span class="sxs-lookup"><span data-stu-id="1d322-189">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="1d322-190">Pokud scénář nemůže tolerovat zpracování duplicitní, pak vývojáři aplikace by měla přidat další logiku aplikace pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="1d322-190">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to applications to handle duplicate message delivery.</span></span> <span data-ttu-id="1d322-191">To se často opírá `getMessageId` metoda zprávy, která zůstává konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="1d322-191">This is often achieved using the `getMessageId` method of the message, which remains constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="1d322-192">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="1d322-192">Delete topics and subscriptions</span></span>
<span data-ttu-id="1d322-193">Chcete-li odstranit tématu nebo předplatného, použijte `ServiceBusRestProxy->deleteTopic` nebo `ServiceBusRestProxy->deleteSubscripton` metody, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="1d322-193">To delete a topic or a subscription, use the `ServiceBusRestProxy->deleteTopic` or the `ServiceBusRestProxy->deleteSubscripton` methods, respectively.</span></span> <span data-ttu-id="1d322-194">Všimněte si, že se odstraní téma také odstraní všechny odběry registrované k tomuto tématu.</span><span class="sxs-lookup"><span data-stu-id="1d322-194">Note that deleting a topic also deletes any subscriptions that are registered with the topic.</span></span>

<span data-ttu-id="1d322-195">Následující příklad ukazuje, jak odstranit téma s názvem `mytopic` a jeho registrované odběry.</span><span class="sxs-lookup"><span data-stu-id="1d322-195">The following example shows how to delete a topic named `mytopic` and its registered subscriptions.</span></span>

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

<span data-ttu-id="1d322-196">Pomocí `deleteSubscription` metodu, můžete odstranit odběr nezávisle:</span><span class="sxs-lookup"><span data-stu-id="1d322-196">By using the `deleteSubscription` method, you can delete a subscription independently:</span></span>

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a><span data-ttu-id="1d322-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d322-197">Next steps</span></span>
<span data-ttu-id="1d322-198">Teď, když jste se naučili základy front Service Bus, najdete v části [fronty, témata a odběry] [ Queues, topics, and subscriptions] Další informace.</span><span class="sxs-lookup"><span data-stu-id="1d322-198">Now that you've learned the basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
