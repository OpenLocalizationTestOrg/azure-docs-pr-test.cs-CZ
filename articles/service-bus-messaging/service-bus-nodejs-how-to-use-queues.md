---
title: "Jak používat fronty Service Bus v Node.js | Microsoft Docs"
description: "Naučte se používat fronty Service Bus v Azure z aplikace Node.js."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: fe2c02534996d99c190593a419a4823888f03d31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a><span data-ttu-id="017ee-103">Jak používat fronty Service Bus s Node.js</span><span class="sxs-lookup"><span data-stu-id="017ee-103">How to use Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="017ee-104">Tento článek popisuje, jak používat fronty Service Bus s Node.js.</span><span class="sxs-lookup"><span data-stu-id="017ee-104">This article describes how to use Service Bus queues with Node.js.</span></span> <span data-ttu-id="017ee-105">Ukázky jsou napsané v jazyce JavaScript a použít modul Node.js Azure.</span><span class="sxs-lookup"><span data-stu-id="017ee-105">The samples are written in JavaScript and use the Node.js Azure module.</span></span> <span data-ttu-id="017ee-106">Pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="017ee-106">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="017ee-107">Další informace o frontách najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="017ee-107">For more information on queues, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="017ee-108">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="017ee-108">Create a Node.js application</span></span>
<span data-ttu-id="017ee-109">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="017ee-109">Create a blank Node.js application.</span></span> <span data-ttu-id="017ee-110">Pokyny o tom, jak vytvořit aplikaci Node.js najdete v tématu [vytvoření a nasazení aplikace Node.js na web Azure][Create and deploy a Node.js application to an Azure Website], nebo [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="017ee-110">For instructions on how to create a Node.js application, see [Create and deploy a Node.js application to an Azure Website][Create and deploy a Node.js application to an Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="017ee-111">Konfigurace aplikace pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="017ee-111">Configure your application to use Service Bus</span></span>
<span data-ttu-id="017ee-112">Pokud chcete používat Azure Service Bus, stáhnout a použít balíček Node.js Azure.</span><span class="sxs-lookup"><span data-stu-id="017ee-112">To use Azure Service Bus, download and use the Node.js Azure package.</span></span> <span data-ttu-id="017ee-113">Tento balíček obsahuje sadu knihoven, které komunikují s služby REST pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="017ee-113">This package includes a set of libraries that communicate with the Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="017ee-114">Získat balíček pomocí uzlu balíček správce (NPM)</span><span class="sxs-lookup"><span data-stu-id="017ee-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="017ee-115">Použití **prostředí Windows PowerShell pro Node.js** příkazové okno a přejděte k **c:\\uzlu\\sbqueues\\WebRole1** složku, ve které jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="017ee-115">Use the **Windows PowerShell for Node.js** command window to navigate to the **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="017ee-116">Typ **npm nainstalujte azure** v okně příkazového řádku, což by měla vést ke výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="017ee-116">Type **npm install azure** in the command window, which should result in output similar to the following:</span></span>

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
3. <span data-ttu-id="017ee-117">Můžete ručně spustit **ls** příkazu ověřte, že **node_modules** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="017ee-117">You can manually run the **ls** command to verify that a **node_modules** folder was created.</span></span> <span data-ttu-id="017ee-118">V této složce najít **azure** balíček, který obsahuje knihovny, je třeba získat přístup fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="017ee-118">Inside that folder find the **azure** package, which contains the libraries you need to access Service Bus queues.</span></span>

### <a name="import-the-module"></a><span data-ttu-id="017ee-119">Naimportujte modul</span><span class="sxs-lookup"><span data-stu-id="017ee-119">Import the module</span></span>
<span data-ttu-id="017ee-120">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující do horní části **server.js** souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="017ee-120">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="017ee-121">Nastavit připojení k Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="017ee-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="017ee-122">Azure modul čte proměnnou prostředí `AZURE_SERVICEBUS_CONNECTION_STRING` získat informace požadované pro připojení k Service Bus.</span><span class="sxs-lookup"><span data-stu-id="017ee-122">The Azure module reads the environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` to obtain information required to connect to Service Bus.</span></span> <span data-ttu-id="017ee-123">Pokud není nastavena tato proměnná prostředí, musíte zadat informace o účtu při volání metody `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="017ee-123">If this environment variable is not set, you must specify the account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="017ee-124">Příklad nastavení proměnných prostředí v konfiguračním souboru pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="017ee-124">For an example of setting the environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="017ee-125">Příklad nastavení proměnných prostředí [portál Azure] [ Azure portal] web Azure, najdete v části [webovou aplikaci Node.js s úložištěm][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="017ee-125">For an example of setting the environment variables in the [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="017ee-126">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="017ee-126">Create a queue</span></span>
<span data-ttu-id="017ee-127">**ServiceBusService** objektu umožňuje pracovat s fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="017ee-127">The **ServiceBusService** object enables you to work with Service Bus queues.</span></span> <span data-ttu-id="017ee-128">Následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="017ee-128">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="017ee-129">Přidejte do horní části **server.js** souboru po příkazu k importu modulu Azure:</span><span class="sxs-lookup"><span data-stu-id="017ee-129">Add it near the top of the **server.js** file, after the statement to import the Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="017ee-130">Při volání `createQueueIfNotExists` na **ServiceBusService** objektu zadané frontě, je vrácen (pokud existuje), nebo se vytvoří novou frontu se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="017ee-130">By calling `createQueueIfNotExists` on the **ServiceBusService** object, the specified queue is returned (if it exists), or a new queue with the specified name is created.</span></span> <span data-ttu-id="017ee-131">Následující kód používá `createQueueIfNotExists` vytvořit nebo připojit do fronty s názvem `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="017ee-131">The following code uses `createQueueIfNotExists` to create or connect to the queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="017ee-132">`createServiceBusService` Metoda také podporuje další možnosti, které vám umožní přepsat výchozí nastavení fronty například čas zprávy do fronty za provozu nebo maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="017ee-132">The `createServiceBusService` method also supports additional options, which enable you to override default queue settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="017ee-133">Následující příklad nastaví na 5 GB a čas live (TTL) hodnotu 1 minuta maximální velikost fronty:</span><span class="sxs-lookup"><span data-stu-id="017ee-133">The following example sets the maximum queue size to 5 GB, and a time to live (TTL) value of 1 minute:</span></span>

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a><span data-ttu-id="017ee-134">Filtry</span><span class="sxs-lookup"><span data-stu-id="017ee-134">Filters</span></span>
<span data-ttu-id="017ee-135">Volitelné filtrování operace lze použít pro operace provedené pomocí **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="017ee-135">Optional filtering operations can be applied to operations performed using **ServiceBusService**.</span></span> <span data-ttu-id="017ee-136">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:</span><span class="sxs-lookup"><span data-stu-id="017ee-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="017ee-137">Až to jeho předběžné zpracování na možnosti žádost, musí volat metodu `next`, předání zpětné volání podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="017ee-137">After doing its pre-processing on the request options, the method must call `next`, passing a callback with the following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="017ee-138">V této zpětného volání a po zpracování `returnObject` (odpověď z požadavku na serveru), musíte buď vyvolání zpětné volání `next` pokud existuje pokračovat ve zpracovávání ostatní filtry nebo jednoduše vyvolat `finalCallback`, který končí volání služby.</span><span class="sxs-lookup"><span data-stu-id="017ee-138">In this callback, and after processing the `returnObject` (the response from the request to the server), the callback must either invoke `next` if it exists to continue processing other filters, or simply invoke `finalCallback`, which ends the service invocation.</span></span>

<span data-ttu-id="017ee-139">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, `ExponentialRetryPolicyFilter` a `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="017ee-139">Two filters that implement retry logic are included with the Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="017ee-140">Následující kód vytvoří `ServiceBusService` objekt, který používá `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="017ee-140">The following code creates a `ServiceBusService` object that uses the `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="017ee-141">Zasílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="017ee-141">Send messages to a queue</span></span>
<span data-ttu-id="017ee-142">K odeslání zprávy do fronty Service Bus, vaše aplikace volání `sendQueueMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="017ee-142">To send a message to a Service Bus queue, your application calls the `sendQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="017ee-143">Zprávy odeslané do (a přijaté z) jsou fronty Service Bus **BrokeredMessage** objekty a mají sadu standardních vlastností (jako například **popisek** a **TimeToLive**), slovník používaný pro udržení vlastních vlastností konkrétní aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="017ee-143">Messages sent to (and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="017ee-144">Aplikace může tělo zprávy nastavit předáním řetězec jako zprávy.</span><span class="sxs-lookup"><span data-stu-id="017ee-144">An application can set the body of the message by passing a string as the message.</span></span> <span data-ttu-id="017ee-145">Všechny požadované vlastnosti standardní se naplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="017ee-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="017ee-146">Následující příklad ukazuje, jak odeslat testovací zprávu do fronty s názvem `myqueue` pomocí `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="017ee-146">The following example demonstrates how to send a test message to the queue named `myqueue` using `sendQueueMessage`:</span></span>

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

<span data-ttu-id="017ee-147">Fronty Service Bus podporují maximální velikost zprávy 256 KB [na úrovni Standard](service-bus-premium-messaging.md) a 1 MB [na úrovni Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="017ee-147">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="017ee-148">Hlavička, která obsahuje standardní a vlastní vlastnosti aplikace, může mít velikost až 64 KB.</span><span class="sxs-lookup"><span data-stu-id="017ee-148">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="017ee-149">Počet zpráv držených ve frontě není omezený, ale celková velikost zpráv držených ve frontě omezená je.</span><span class="sxs-lookup"><span data-stu-id="017ee-149">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="017ee-150">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="017ee-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="017ee-151">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="017ee-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="017ee-152">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="017ee-152">Receive messages from a queue</span></span>
<span data-ttu-id="017ee-153">Přijímání zpráv z fronty pomocí `receiveQueueMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="017ee-153">Messages are received from a queue using the `receiveQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="017ee-154">Ve výchozím nastavení jsou odstraněny zprávy z fronty jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení zpráva bez odstranění z fronty nastavením volitelný parametr `isPeekLock` k **true**.</span><span class="sxs-lookup"><span data-stu-id="017ee-154">By default, messages are deleted from the queue as they are read; however, you can read (peek) and lock the message without deleting it from the queue by setting the optional parameter `isPeekLock` to **true**.</span></span>

<span data-ttu-id="017ee-155">Výchozí chování pro čtení a odstranění zprávy v rámci operace příjmu je nejjednodušší model a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="017ee-155">The default behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="017ee-156">Pro lepší vysvětlení si představte scénář, ve kterém spotřebitel vyšle požadavek na přijetí, ale než ji může zpracovat, dojde v něm k chybě a ukončí se.</span><span class="sxs-lookup"><span data-stu-id="017ee-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="017ee-157">Vzhledem k tomu, že Service Bus se už ale zprávu označila jako spotřebovávanou, pak když se aplikace restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily zprávu, která se spotřebovala před havárii.</span><span class="sxs-lookup"><span data-stu-id="017ee-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="017ee-158">Pokud `isPeekLock` parametr je nastaven na **true**, receive stane dvoufázového operaci, která umožňuje podporuje aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="017ee-158">If the `isPeekLock` parameter is set to **true**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="017ee-159">Když Service Bus přijme požadavek, najde zprávu, která je na řadě ke spotřebování, uzamkne ji proti spotřebování jinými spotřebiteli a vrátí ji do aplikace.</span><span class="sxs-lookup"><span data-stu-id="017ee-159">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="017ee-160">Když aplikace dokončí zpracování zprávy (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí volání `deleteMessage` metoda a poskytující zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="017ee-160">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `deleteMessage` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="017ee-161">`deleteMessage` Metoda označí zprávu jako spotřebovávanou a odstraní ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="017ee-161">The `deleteMessage` method marks the message as being consumed and removes it from the queue.</span></span>

<span data-ttu-id="017ee-162">Následující příklad ukazuje, jak přijímat a zpracovávat zprávy pomocí `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="017ee-162">The following example demonstrates how to receive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="017ee-163">V příkladu nejprve obdrží a odstraní zprávu a pak obdrží zprávu pomocí `isPeekLock` nastavena na **true**, pak odstraní zprávu pomocí `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="017ee-163">The example first receives and deletes a message, and then receives a message using `isPeekLock` set to **true**, then deletes the message using `deleteMessage`:</span></span>

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="017ee-164">Zpracování pádů aplikace a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="017ee-164">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="017ee-165">Service Bus poskytuje funkce, které vám pomůžou se elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="017ee-165">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="017ee-166">Pokud přijímající aplikace nedokáže zpracovat zprávu z nějakého důvodu, pak může zavolat `unlockMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="017ee-166">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="017ee-167">To způsobí, že Service Bus zprávu odemkne ve frontě a zpřístupní ji pro další přijetí, stejnou spotřebitelskou aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="017ee-167">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="017ee-168">Je také vypršení časového limitu přidružené zpráva uzamčená ve frontě, a pokud se nepodaří aplikace zprávu nezpracuje zámku vyprší časový limit (například pokud aplikace spadne), pak se Service Bus zprávu automaticky odemkne a zpřístupní ji pro další přijetí.</span><span class="sxs-lookup"><span data-stu-id="017ee-168">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="017ee-169">V případě, že aplikace spadne po zpracování zprávy, ale předtím, než `deleteMessage` metoda je volána, pak zpráva bude vysláním do aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="017ee-169">In the event that the application crashes after processing the message but before the `deleteMessage` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="017ee-170">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v některých situacích může doručit víckrát stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="017ee-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="017ee-171">Pokud daný scénář nemůže tolerovat zpracování víc než jednou, vývojáři aplikace by měli přidat další logiku navíc pro zpracování víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="017ee-171">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="017ee-172">To se často opírá **MessageId** vlastnosti zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="017ee-172">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="017ee-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="017ee-173">Next steps</span></span>
<span data-ttu-id="017ee-174">Další informace o frontách, najdete v následujících materiálech.</span><span class="sxs-lookup"><span data-stu-id="017ee-174">To learn more about queues, see the following resources.</span></span>

* <span data-ttu-id="017ee-175">[Fronty, témata a odběry][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="017ee-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="017ee-176">[Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="017ee-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="017ee-177">Středisku pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="017ee-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
