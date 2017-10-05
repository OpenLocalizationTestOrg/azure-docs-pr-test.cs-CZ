---
title: "Jak používat témata a předplatné Azure Service Bus s Node.js | Microsoft Docs"
description: "Naučte se používat témata a odběry Service Bus v Azure z aplikace Node.js."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 24ae9b80f75531c5e4a84c3b4a6666a6f8a83d2c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a><span data-ttu-id="7e780-103">Postup použití služby Service Bus témata a odběry s Node.js</span><span class="sxs-lookup"><span data-stu-id="7e780-103">How to Use Service Bus topics and subscriptions with Node.js</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="7e780-104">Tato příručka popisuje, jak používat témata Service Bus a odběry z aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="7e780-104">This guide describes how to use Service Bus topics and subscriptions from Node.js applications.</span></span> <span data-ttu-id="7e780-105">Pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv** do tématu, **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="7e780-105">The scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages** to a topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="7e780-106">Další informace o tématech a odběrech najdete v části [Další kroky](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="7e780-106">For more information about topics and subscriptions, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="7e780-107">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="7e780-107">Create a Node.js application</span></span>
<span data-ttu-id="7e780-108">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="7e780-108">Create a blank Node.js application.</span></span> <span data-ttu-id="7e780-109">Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvoření a nasazení aplikace Node.js na webovou stránku Azure], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí systému Windows Prostředí PowerShell nebo na webovém serveru pomocí služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="7e780-109">For instructions on creating a Node.js application, see [Create and deploy a Node.js application to an Azure Web Site], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or Web Site with WebMatrix.</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="7e780-110">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="7e780-110">Configure your application to use Service Bus</span></span>
<span data-ttu-id="7e780-111">K použití služby Service Bus, stáhněte si balíček Node.js Azure.</span><span class="sxs-lookup"><span data-stu-id="7e780-111">To use Service Bus, download the Node.js Azure package.</span></span> <span data-ttu-id="7e780-112">Tento balíček obsahuje sadu knihoven, které komunikují s služby REST pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="7e780-112">This package includes a set of libraries that communicate with the Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="7e780-113">Získat balíček pomocí uzlu balíček správce (NPM)</span><span class="sxs-lookup"><span data-stu-id="7e780-113">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="7e780-114">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte do složky, které jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7e780-114">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="7e780-115">Typ **npm nainstalujte azure** v okně příkazového řádku, což by měla vést ke následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7e780-115">Type **npm install azure** in the command window, which should result in the following output:</span></span>

   ```
       azure@0.7.5 node_modules\azure
   ├── dateformat@1.0.2-1.2.3
   ├── xmlbuilder@0.4.2
   ├── node-uuid@1.2.0
   ├── mime@1.2.9
   ├── underscore@1.4.4
   ├── validator@1.1.1
   ├── tunnel@0.0.2
   ├── wns@0.5.3
   ├── xml2js@0.2.7 (sax@0.5.2)
   └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
   ```
3. <span data-ttu-id="7e780-116">Můžete ručně spustit **ls** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="7e780-116">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="7e780-117">V této složce najít **azure** balíček, který obsahuje knihovny, je třeba získat přístup témat sběrnice Service Bus.</span><span class="sxs-lookup"><span data-stu-id="7e780-117">Inside that folder find the **azure** package, which contains the libraries you need to access Service Bus topics.</span></span>

### <a name="import-the-module"></a><span data-ttu-id="7e780-118">Naimportujte modul</span><span class="sxs-lookup"><span data-stu-id="7e780-118">Import the module</span></span>
<span data-ttu-id="7e780-119">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující do horní části **server.js** souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="7e780-119">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="7e780-120">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="7e780-120">Set up a Service Bus connection</span></span>
<span data-ttu-id="7e780-121">Azure modul čte proměnné prostředí `AZURE_SERVICEBUS_NAMESPACE` a `AZURE_SERVICEBUS_ACCESS_KEY` informace požadované pro připojení k Service Bus.</span><span class="sxs-lookup"><span data-stu-id="7e780-121">The Azure module reads the environment variables `AZURE_SERVICEBUS_NAMESPACE` and `AZURE_SERVICEBUS_ACCESS_KEY` for information required to connect to Service Bus.</span></span> <span data-ttu-id="7e780-122">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu při volání metody `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="7e780-122">If these environment variables are not set, you must specify the account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="7e780-123">Příklad nastavení proměnných prostředí pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="7e780-123">For an example of setting the environment variables for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="7e780-124">Příklad nastavení proměnných prostředí pro web Azure, naleznete v části [webovou aplikaci Node.js s úložištěm][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="7e780-124">For an example of setting the environment variables for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="7e780-125">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="7e780-125">Create a topic</span></span>
<span data-ttu-id="7e780-126">**ServiceBusService** objektu umožňuje pracovat s tématy.</span><span class="sxs-lookup"><span data-stu-id="7e780-126">The **ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="7e780-127">Následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-127">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="7e780-128">Přidejte do horní části **server.js** souboru po příkazu k importu modulu azure:</span><span class="sxs-lookup"><span data-stu-id="7e780-128">Add it near the top of the **server.js** file, after the statement to import the azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="7e780-129">Při volání `createTopicIfNotExists` na **ServiceBusService** objektu zadaného tématu bude vrácen (pokud existuje), nebo se vytvoří nové téma se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="7e780-129">By calling `createTopicIfNotExists` on the **ServiceBusService** object, the specified topic will be returned (if it exists,) or a new topic with the specified name will be created.</span></span> <span data-ttu-id="7e780-130">Následující kód používá `createTopicIfNotExists` vytvořit nebo připojit k tématu s názvem `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="7e780-130">The following code uses `createTopicIfNotExists` to create or connect to the topic named `MyTopic`:</span></span>

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

<span data-ttu-id="7e780-131">`createServiceBusService` Metoda také podporuje další možnosti, které vám umožní přepsat výchozí nastavení téma například zpráva hodnota time to live nebo téma maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="7e780-131">The `createServiceBusService` method also supports additional options, which enable you to override default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="7e780-132">Následující příklad nastaví téma maximální velikost 5 GB s časem TTL 1 minuta:</span><span class="sxs-lookup"><span data-stu-id="7e780-132">The following example sets the maximum topic size to 5GB with a time to live of 1 minute:</span></span>

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a><span data-ttu-id="7e780-133">Filtry</span><span class="sxs-lookup"><span data-stu-id="7e780-133">Filters</span></span>
<span data-ttu-id="7e780-134">Volitelné filtrování operace lze použít pro operace provedené pomocí **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="7e780-134">Optional filtering operations can be applied to operations performed using **ServiceBusService**.</span></span> <span data-ttu-id="7e780-135">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:</span><span class="sxs-lookup"><span data-stu-id="7e780-135">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="7e780-136">Po provedení předzpracování na žádost o možnostech, volá metodu `next`, předání zpětné volání podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="7e780-136">After performing preprocessing on the request options, the method calls `next`, passing a callback with the following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="7e780-137">V této zpětného volání a po zpracování `returnObject` (odpověď z požadavku na serveru), zpětné volání musí vyvolat další, pokud existuje pokračovat ve zpracovávání ostatní filtry nebo vyvolat `finalCallback` , jinak hodnota na konec volání služby.</span><span class="sxs-lookup"><span data-stu-id="7e780-137">In this callback, and after processing the `returnObject` (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or invoke `finalCallback` otherwise, to end the service invocation.</span></span>

<span data-ttu-id="7e780-138">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="7e780-138">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="7e780-139">Vytvoří následující **ServiceBusService** objekt, který používá **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="7e780-139">The following creates a **ServiceBusService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a><span data-ttu-id="7e780-140">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="7e780-140">Create subscriptions</span></span>
<span data-ttu-id="7e780-141">Odběry témat taky jsou vytvořeny pomocí **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-141">Topic subscriptions are also created with the **ServiceBusService** object.</span></span> <span data-ttu-id="7e780-142">Odběry mají názvy a můžou mít volitelné filtry, které omezují výběr zpráv doručených do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="7e780-142">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="7e780-143">Odběry jsou trvalé a bude dál existovat, dokud buď jejich nebo tématu jsou spojeny s, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="7e780-143">Subscriptions are persistent and will continue to exist until either they, or the topic they are associated with, are deleted.</span></span> <span data-ttu-id="7e780-144">Pokud vaše aplikace obsahuje logiku pro vytvoření odběru, musí nejprve zkontrolujte, zda předplatné už existuje s použitím `getSubscription` metoda.</span><span class="sxs-lookup"><span data-stu-id="7e780-144">If your application contains logic to create a subscription, it should first check if the subscription already exists by using the `getSubscription` method.</span></span>
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="7e780-145">Vytvoření odběru s výchozím filtrem (MatchAll).</span><span class="sxs-lookup"><span data-stu-id="7e780-145">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="7e780-146">Filtr **MatchAll** je výchozí filtr, který se použije v případě, že při vytváření nového odběru nezadáte žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="7e780-146">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="7e780-147">Když **MatchAll** filtr se používá, všechny zprávy publikované do tématu jsou umístěny do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="7e780-147">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="7e780-148">Následující příklad vytvoří odběr s názvem "AllMessages" a používá výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="7e780-148">The following example creates a subscription named 'AllMessages' and uses the default **MatchAll** filter.</span></span>

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="7e780-149">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="7e780-149">Create subscriptions with filters</span></span>
<span data-ttu-id="7e780-150">Můžete taky vytvořit filtry, které umožňují, že jste do oboru, který zprávy odeslané do tématu by měla objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="7e780-150">You can also create filters that allow you to scope which messages sent to a topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="7e780-151">Nejflexibilnější filtr, který odběry podporují je **SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="7e780-151">The most flexible type of filter supported by subscriptions is the **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="7e780-152">Filtry SQL pracují s vlastnostmi zpráv publikované do tématu.</span><span class="sxs-lookup"><span data-stu-id="7e780-152">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="7e780-153">Další podrobnosti o výrazy, které lze použít s filtrem SQL, projděte si [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.</span><span class="sxs-lookup"><span data-stu-id="7e780-153">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="7e780-154">Filtry lze přidat na předplatné s použitím `createRule` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-154">Filters can be added to a subscription by using the `createRule` method of the **ServiceBusService** object.</span></span> <span data-ttu-id="7e780-155">Tato metoda umožňuje přidat nové filtry k existujícímu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="7e780-155">This method allows you to add new filters to an existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="7e780-156">Protože výchozí filtr je automaticky použita pro všechny nové odběry, je nutno nejprve odstranit výchozí filtr nebo **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="7e780-156">Because the default filter is applied automatically to all new subscriptions, you must first remove the default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="7e780-157">Výchozí pravidla můžete odebrat pomocí `deleteRule` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-157">You can remove the default rule by using the `deleteRule` method of the **ServiceBusService** object.</span></span>
>
>

<span data-ttu-id="7e780-158">Následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="7e780-158">The following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

<span data-ttu-id="7e780-159">Podobně platí, tento příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna na 3:</span><span class="sxs-lookup"><span data-stu-id="7e780-159">Similarly, the following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal to 3:</span></span>

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

<span data-ttu-id="7e780-160">Když je nyní odeslána zpráva `MyTopic`, budou doručeny vždy příjemci `AllMessages` odběr tématu a selektivně příjemcům přihlásit k odběru `HighMessages` a `LowMessages` (odběry tématu v závislosti na obsahu zprávy).</span><span class="sxs-lookup"><span data-stu-id="7e780-160">When a message is now sent to `MyTopic`, it will always be delivered to receivers subscribed to the `AllMessages` topic subscription, and selectively delivered to receivers subscribed to the `HighMessages` and `LowMessages` topic subscriptions (depending upon the message content).</span></span>

## <a name="how-to-send-messages-to-a-topic"></a><span data-ttu-id="7e780-161">Odesílání zpráv do tématu</span><span class="sxs-lookup"><span data-stu-id="7e780-161">How to send messages to a topic</span></span>
<span data-ttu-id="7e780-162">K odeslání zprávy do tématu Service Bus, musí vaše aplikace používat `sendTopicMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-162">To send a message to a Service Bus topic, your application must use the `sendTopicMessage` method of the **ServiceBusService** object.</span></span>
<span data-ttu-id="7e780-163">Zprávy odeslané do témat Service Bus jsou **BrokeredMessage** objekty.</span><span class="sxs-lookup"><span data-stu-id="7e780-163">Messages sent to Service Bus topics are **BrokeredMessage** objects.</span></span>
<span data-ttu-id="7e780-164">**BrokeredMessage** objekty mají sadu standardních vlastností (jako například `Label` a `TimeToLive`), slovník používaný pro udržení vlastních vlastností specifické pro aplikaci a obsah řetězec dat.</span><span class="sxs-lookup"><span data-stu-id="7e780-164">**BrokeredMessage** objects have a set of standard properties (such as `Label` and `TimeToLive`), a dictionary that is used to hold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="7e780-165">Aplikace může tělo zprávy nastavit předáním řetězcové hodnoty `sendTopicMessage` a potřebné standardní vlastnosti vyplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7e780-165">An application can set the body of the message by passing a string value to the `sendTopicMessage` and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="7e780-166">Následující příklad ukazuje, jak odeslat pět zkušebních zpráv do `MyTopic`.</span><span class="sxs-lookup"><span data-stu-id="7e780-166">The following example demonstrates how to send five test messages to `MyTopic`.</span></span> <span data-ttu-id="7e780-167">Všimněte si, že `messagenumber` hodnota vlastnosti každé zprávy se liší na iteraci smyčky (to určí, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="7e780-167">Note that the `messagenumber` property value of each message varies on the iteration of the loop (this will determine which subscriptions receive it):</span></span>

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

<span data-ttu-id="7e780-168">Témata Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="7e780-168">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="7e780-169">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="7e780-169">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="7e780-170">Počet zpráv držených v tématu není omezený, ale celková velikost zpráv držených v tématu omezená je.</span><span class="sxs-lookup"><span data-stu-id="7e780-170">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="7e780-171">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="7e780-171">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="7e780-172">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="7e780-172">Receive messages from a subscription</span></span>
<span data-ttu-id="7e780-173">Přijímání zpráv z odběru pomocí `receiveSubscriptionMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-173">Messages are received from a subscription using the `receiveSubscriptionMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="7e780-174">Ve výchozím nastavení jsou odstraněny zprávy z odběru jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení zpráva bez odstranění z předplatného nastavením volitelný parametr `isPeekLock` k **true**.</span><span class="sxs-lookup"><span data-stu-id="7e780-174">By default, messages are deleted from the subscription as they are read; however, you can read (peek) and lock the message without deleting it from the subscription by setting the optional parameter `isPeekLock` to **true**.</span></span>

<span data-ttu-id="7e780-175">Výchozí chování pro čtení a odstranění zprávy v rámci operace příjmu je nejjednodušší model a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="7e780-175">The default behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="7e780-176">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="7e780-176">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="7e780-177">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="7e780-177">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="7e780-178">Pokud `isPeekLock` parametr je nastaven na **true**, receive stane dvoufázového operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="7e780-178">If the `isPeekLock` parameter is set to **true**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="7e780-179">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e780-179">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span>
<span data-ttu-id="7e780-180">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání **deleteMessage** metoda a poskytující zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="7e780-180">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **deleteMessage** method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="7e780-181">**DeleteMessage** metoda bude označí zprávu jako spotřebovávanou a odebrat ji z odběru.</span><span class="sxs-lookup"><span data-stu-id="7e780-181">The **deleteMessage** method will mark the message as being consumed and remove it from the subscription.</span></span>

<span data-ttu-id="7e780-182">Následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receiveSubscriptionMessage`.</span><span class="sxs-lookup"><span data-stu-id="7e780-182">The following example demonstrates how messages can be received and processed using `receiveSubscriptionMessage`.</span></span> <span data-ttu-id="7e780-183">V příkladu nejprve obdrží a odstraní zprávu z předplatného 'LowMessages' a potom pomocí předplatného 'HighMessages' přijme zprávu o `isPeekLock` nastaven na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7e780-183">The example first receives and deletes a message from the 'LowMessages' subscription, and then receives a message from the 'HighMessages' subscription using `isPeekLock` set to true.</span></span> <span data-ttu-id="7e780-184">Pak odstraní zprávu pomocí `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="7e780-184">It then deletes the message using `deleteMessage`:</span></span>

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="7e780-185">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="7e780-185">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="7e780-186">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e780-186">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="7e780-187">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlockMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="7e780-187">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="7e780-188">To způsobí, že Service Bus zprávu odemkne v odběru a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="7e780-188">This will cause Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="7e780-189">Je také vypršení časového limitu zpráva uzamčená v odběru, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak Service Bus zprávu automaticky odemkne a ji zpřístupní k přijetí.</span><span class="sxs-lookup"><span data-stu-id="7e780-189">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="7e780-190">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `deleteMessage` metoda je volána, pak zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="7e780-190">In the event that the application crashes after processing the message but before the `deleteMessage` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="7e780-191">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="7e780-191">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="7e780-192">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e780-192">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="7e780-193">To se často opírá **MessageId** vlastnosti zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="7e780-193">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="7e780-194">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="7e780-194">Delete topics and subscriptions</span></span>
<span data-ttu-id="7e780-195">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="7e780-195">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span>
<span data-ttu-id="7e780-196">Následující příklad ukazuje, jak odstranit téma s názvem `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="7e780-196">The following example demonstrates how to delete the topic named `MyTopic`:</span></span>

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

<span data-ttu-id="7e780-197">Odstraní téma také odstraní všechny odběry registrované k tomuto tématu.</span><span class="sxs-lookup"><span data-stu-id="7e780-197">Deleting a topic will also delete any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="7e780-198">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="7e780-198">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="7e780-199">Následující příklad ukazuje, jak odstranit odběr s názvem `HighMessages` z `MyTopic` tématu:</span><span class="sxs-lookup"><span data-stu-id="7e780-199">The following example shows how to delete a subscription named `HighMessages` from the `MyTopic` topic:</span></span>

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a><span data-ttu-id="7e780-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e780-200">Next Steps</span></span>
<span data-ttu-id="7e780-201">Teď, když jste se naučili základy témat sběrnice Service Bus, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="7e780-201">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="7e780-202">V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="7e780-202">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="7e780-203">Reference k rozhraní API pro [SqlFilter][SqlFilter]</span><span class="sxs-lookup"><span data-stu-id="7e780-203">API reference for [SqlFilter][SqlFilter].</span></span>
* <span data-ttu-id="7e780-204">Přejděte [Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7e780-204">Visit the [Azure SDK for Node][Azure SDK for Node] repository on GitHub.</span></span>

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[vytvoření a nasazení aplikace Node.js na webovou stránku Azure]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
