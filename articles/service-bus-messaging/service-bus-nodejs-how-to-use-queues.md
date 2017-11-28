---
title: aaaHow toouse Service Bus fronty v Node.js | Microsoft Docs
description: "Zjistěte, jak fronty toouse Service Bus v Azure z aplikace Node.js."
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
ms.openlocfilehash: c55354b2061c41aba1093cc3f12ce2a1bc37a3cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-nodejs"></a><span data-ttu-id="1f17f-103">Jak toouse Service Bus fronty s Node.js</span><span class="sxs-lookup"><span data-stu-id="1f17f-103">How toouse Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="1f17f-104">Tento článek popisuje, jak toouse Service Bus fronty s Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f17f-104">This article describes how toouse Service Bus queues with Node.js.</span></span> <span data-ttu-id="1f17f-105">Hello ukázky jsou napsané v jazyce JavaScript a používají hello modul Node.js Azure.</span><span class="sxs-lookup"><span data-stu-id="1f17f-105">hello samples are written in JavaScript and use hello Node.js Azure module.</span></span> <span data-ttu-id="1f17f-106">Hello pokryté scénáře zahrnují **vytváření front**, **odesílání a přijímání zpráv**, a **odstraňování front**.</span><span class="sxs-lookup"><span data-stu-id="1f17f-106">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="1f17f-107">Další informace o frontách najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="1f17f-107">For more information on queues, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="1f17f-108">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="1f17f-108">Create a Node.js application</span></span>
<span data-ttu-id="1f17f-109">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f17f-109">Create a blank Node.js application.</span></span> <span data-ttu-id="1f17f-110">Pokyny najdete v části toocreate aplikace Node.js, [vytvořit a nasadit tooan aplikace Node.js webu Azure][Create and deploy a Node.js application tooan Azure Website], nebo [Node.js cloudové služby] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f17f-110">For instructions on how toocreate a Node.js application, see [Create and deploy a Node.js application tooan Azure Website][Create and deploy a Node.js application tooan Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="1f17f-111">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="1f17f-111">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="1f17f-112">toouse Azure Service Bus, stáhnout a použít balíček Node.js Azure hello.</span><span class="sxs-lookup"><span data-stu-id="1f17f-112">toouse Azure Service Bus, download and use hello Node.js Azure package.</span></span> <span data-ttu-id="1f17f-113">Tento balíček obsahuje sadu knihoven, které komunikují s hello služby REST pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1f17f-113">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="1f17f-114">Použijte uzel balíček správce (NPM) tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="1f17f-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="1f17f-115">Použití hello **prostředí Windows PowerShell pro Node.js** příkazového okna toonavigate toohello **c:\\uzlu\\sbqueues\\WebRole1** složku, ve které jste vytvořili vaší ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f17f-115">Use hello **Windows PowerShell for Node.js** command window toonavigate toohello **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="1f17f-116">Typ **npm nainstalujte azure** v hello příkazové okno, které by měl způsobit výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="1f17f-116">Type **npm install azure** in hello command window, which should result in output similar toohello following:</span></span>

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
3. <span data-ttu-id="1f17f-117">Můžete ručně spustit hello **ls** tooverify příkaz, **node_modules** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="1f17f-117">You can manually run hello **ls** command tooverify that a **node_modules** folder was created.</span></span> <span data-ttu-id="1f17f-118">V této složce najít hello **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1f17f-118">Inside that folder find hello **azure** package, which contains hello libraries you need tooaccess Service Bus queues.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="1f17f-119">Import modulu hello</span><span class="sxs-lookup"><span data-stu-id="1f17f-119">Import hello module</span></span>
<span data-ttu-id="1f17f-120">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="1f17f-120">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="1f17f-121">Nastavit připojení k Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="1f17f-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="1f17f-122">Hello Azure modul čte proměnnou prostředí hello `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain informace požadované tooconnect tooService sběrnice.</span><span class="sxs-lookup"><span data-stu-id="1f17f-122">hello Azure module reads hello environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain information required tooconnect tooService Bus.</span></span> <span data-ttu-id="1f17f-123">Pokud není nastavena tato proměnná prostředí, musíte zadat informace o účtu hello při volání metody `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="1f17f-123">If this environment variable is not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="1f17f-124">Příklad nastavení proměnných prostředí hello v konfiguračním souboru pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="1f17f-124">For an example of setting hello environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="1f17f-125">Příklad nastavení proměnných prostředí hello v hello [portál Azure] [ Azure portal] web Azure, najdete v části [webovou aplikaci Node.js s úložištěm] [ Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="1f17f-125">For an example of setting hello environment variables in hello [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="1f17f-126">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="1f17f-126">Create a queue</span></span>
<span data-ttu-id="1f17f-127">Hello **ServiceBusService** objekt vám umožní toowork pomocí front Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1f17f-127">hello **ServiceBusService** object enables you toowork with Service Bus queues.</span></span> <span data-ttu-id="1f17f-128">Hello následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-128">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="1f17f-129">Přidat v hello horní části hello **server.js** souboru po hello příkaz tooimport hello modulu Azure:</span><span class="sxs-lookup"><span data-stu-id="1f17f-129">Add it near hello top of hello **server.js** file, after hello statement tooimport hello Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="1f17f-130">Při volání `createQueueIfNotExists` na hello **ServiceBusService** objektu hello určena fronty je vrácen (pokud existuje), nebo se vytvoří novou frontu se zadaným názvem hello.</span><span class="sxs-lookup"><span data-stu-id="1f17f-130">By calling `createQueueIfNotExists` on hello **ServiceBusService** object, hello specified queue is returned (if it exists), or a new queue with hello specified name is created.</span></span> <span data-ttu-id="1f17f-131">Hello následující kód používá `createQueueIfNotExists` toocreate nebo připojení toohello frontu s názvem `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="1f17f-131">hello following code uses `createQueueIfNotExists` toocreate or connect toohello queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="1f17f-132">Hello `createServiceBusService` metoda také podporuje další možnosti, které umožňují toooverride výchozí nastavení fronty například velikost fronty toolive nebo maximální doba zprávy.</span><span class="sxs-lookup"><span data-stu-id="1f17f-132">hello `createServiceBusService` method also supports additional options, which enable you toooverride default queue settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="1f17f-133">Hello následující příklad ilustruje hello fronty maximální velikost too5 GB a hodnotu času toolive (TTL), 1 minuta:</span><span class="sxs-lookup"><span data-stu-id="1f17f-133">hello following example sets hello maximum queue size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="1f17f-134">Filtry</span><span class="sxs-lookup"><span data-stu-id="1f17f-134">Filters</span></span>
<span data-ttu-id="1f17f-135">Volitelné filtrování operace může být použité toooperations provádí pomocí **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="1f17f-135">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="1f17f-136">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:</span><span class="sxs-lookup"><span data-stu-id="1f17f-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="1f17f-137">Až to jeho předběžné zpracování na možnosti hello žádost, musí volat metoda hello `next`, předání zpětné volání s hello následující podpis:</span><span class="sxs-lookup"><span data-stu-id="1f17f-137">After doing its pre-processing on hello request options, hello method must call `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="1f17f-138">V této zpětného volání a po zpracování hello `returnObject` (hello odpovědi ze serveru toohello požadavek hello), musíte buď vyvolání zpětného volání hello `next` pokud existuje toocontinue zpracování ostatní filtry, nebo jednoduše vyvolání `finalCallback`, které končí volání služby Hello.</span><span class="sxs-lookup"><span data-stu-id="1f17f-138">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback must either invoke `next` if it exists toocontinue processing other filters, or simply invoke `finalCallback`, which ends hello service invocation.</span></span>

<span data-ttu-id="1f17f-139">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, `ExponentialRetryPolicyFilter` a `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="1f17f-139">Two filters that implement retry logic are included with hello Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="1f17f-140">Hello následující kód vytvoří `ServiceBusService` objekt, který používá hello `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="1f17f-140">hello following code creates a `ServiceBusService` object that uses hello `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="1f17f-141">Odesílat zprávy fronty tooa</span><span class="sxs-lookup"><span data-stu-id="1f17f-141">Send messages tooa queue</span></span>
<span data-ttu-id="1f17f-142">toosend fronty Service Bus zprávu tooa aplikace volá hello `sendQueueMessage` metodu hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-142">toosend a message tooa Service Bus queue, your application calls hello `sendQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="1f17f-143">Zprávy odeslané příliš (a přijaté z) jsou fronty služby Service Bus **BrokeredMessage** objekty a mají sadu standardních vlastností (jako například **popisek** a **TimeToLive**), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo s libovolnými aplikačními daty.</span><span class="sxs-lookup"><span data-stu-id="1f17f-143">Messages sent too(and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="1f17f-144">Aplikace můžete nastavit hello těla zprávy hello předáním řetězec jako uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-144">An application can set hello body of hello message by passing a string as hello message.</span></span> <span data-ttu-id="1f17f-145">Všechny požadované vlastnosti standardní se naplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1f17f-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="1f17f-146">Hello následující příklad ukazuje, jak toosend zkušební zprávu toohello frontu s názvem `myqueue` pomocí `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="1f17f-146">hello following example demonstrates how toosend a test message toohello queue named `myqueue` using `sendQueueMessage`:</span></span>

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

<span data-ttu-id="1f17f-147">Fronty Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="1f17f-147">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="1f17f-148">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="1f17f-148">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="1f17f-149">Neexistuje žádné omezení na hello počet zpráv držených ve frontě, ale není na hello celková velikost hello zpráv držených ve frontě.</span><span class="sxs-lookup"><span data-stu-id="1f17f-149">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="1f17f-150">Velikost fronty se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="1f17f-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="1f17f-151">Další informace o kvótách najdete v tématu [Service Bus kvóty][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="1f17f-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="1f17f-152">Příjem zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="1f17f-152">Receive messages from a queue</span></span>
<span data-ttu-id="1f17f-153">Přijímání zpráv z fronty pomocí hello `receiveQueueMessage` metodu hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-153">Messages are received from a queue using hello `receiveQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="1f17f-154">Ve výchozím nastavení jsou odstraněny zprávy z fronty hello jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z fronty hello podle nastavení volitelný parametr hello `isPeekLock` příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="1f17f-154">By default, messages are deleted from hello queue as they are read; however, you can read (peek) and lock hello message without deleting it from hello queue by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="1f17f-155">Hello výchozí chování čtení a odstranění uvítací zprávu jako součást hello přijímat operaci je hello nejjednodušší model a funguje nejlépe ve scénářích, kde aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="1f17f-155">hello default behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="1f17f-156">toounderstand, představte si třeba situaci v problémy, které příjemce hello hello přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="1f17f-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="1f17f-157">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="1f17f-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="1f17f-158">Pokud hello `isPeekLock` parametr je nastaven příliš**true**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="1f17f-158">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="1f17f-159">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f17f-159">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="1f17f-160">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze hello hello přijímat proces voláním `deleteMessage` metoda a poskytování toobe hello zprávu odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="1f17f-160">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `deleteMessage` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="1f17f-161">Hello `deleteMessage` metoda označí uvítací zprávu jako spotřebovávanou a odstraní ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="1f17f-161">hello `deleteMessage` method marks hello message as being consumed and removes it from hello queue.</span></span>

<span data-ttu-id="1f17f-162">Hello následující příklad ukazuje, jak se proces a tooreceive zprávy, pomocí `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="1f17f-162">hello following example demonstrates how tooreceive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="1f17f-163">Hello příklad nejprve obdrží a odstraní zprávu a pak obdrží zprávu pomocí `isPeekLock` nastavit příliš**true**, pak odstraní hello zprávu pomocí `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="1f17f-163">hello example first receives and deletes a message, and then receives a message using `isPeekLock` set too**true**, then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="1f17f-164">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="1f17f-164">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="1f17f-165">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="1f17f-165">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="1f17f-166">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` metodu hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-166">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="1f17f-167">To bude způsobit toounlock Service Bus zprávu ve frontě hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="1f17f-167">This will cause Service Bus toounlock the message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="1f17f-168">Je také vypršení časového limitu přidružené zpráva uzamčená v rámci hello fronty, a pokud selže hello aplikace, které tooprocess hello zprávy před hello zámku vyprší časový limit (např. Pokud hello aplikace spadne), pak Service Bus se automatické odemknutí uvítací zprávu a nastavte jej k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="1f17f-168">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="1f17f-169">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="1f17f-169">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="1f17f-170">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="1f17f-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="1f17f-171">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="1f17f-171">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="1f17f-172">To se často opírá hello **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="1f17f-172">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f17f-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f17f-173">Next steps</span></span>
<span data-ttu-id="1f17f-174">toolearn Další informace o frontách, najdete v části hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="1f17f-174">toolearn more about queues, see hello following resources.</span></span>

* <span data-ttu-id="1f17f-175">[Fronty, témata a odběry][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="1f17f-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="1f17f-176">[Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="1f17f-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="1f17f-177">Středisko pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="1f17f-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
