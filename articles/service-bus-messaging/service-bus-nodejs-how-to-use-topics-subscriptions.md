---
title: "aaaHow toouse Azure Service Bus témata a odběry s Node.js | Microsoft Docs"
description: "Zjistěte, jak toouse Service Bus témat a odběrů v Azure z aplikace Node.js."
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
ms.openlocfilehash: e8f6e7ad6ed16d844c408337ac9e50f990e3fafd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a><span data-ttu-id="a7f9a-103">Jak tooUse Service Bus témata a odběry s Node.js</span><span class="sxs-lookup"><span data-stu-id="a7f9a-103">How tooUse Service Bus topics and subscriptions with Node.js</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="a7f9a-104">Tato příručka popisuje, jak toouse Service Bus témata a odběry z aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-104">This guide describes how toouse Service Bus topics and subscriptions from Node.js applications.</span></span> <span data-ttu-id="a7f9a-105">Hello pokryté scénáře zahrnují **vytváření témat a odběrů**, **vytváření filtrů odběrů**, **odesílání zpráv** tooa tématu **přijetí zprávy z odběru**, a **odstranění témat a odběrů**.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-105">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="a7f9a-106">Další informace o tématech a odběrech najdete v tématu hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-106">For more information about topics and subscriptions, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="a7f9a-107">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="a7f9a-107">Create a Node.js application</span></span>
<span data-ttu-id="a7f9a-108">Vytvoření prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-108">Create a blank Node.js application.</span></span> <span data-ttu-id="a7f9a-109">Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvořit a nasadit tooan aplikace Node.js webu Azure], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí systému Windows Prostředí PowerShell nebo na webovém serveru pomocí služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-109">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooan Azure Web Site], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or Web Site with WebMatrix.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="a7f9a-110">Konfigurace vaší aplikace toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="a7f9a-110">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="a7f9a-111">toouse Service Bus, stáhněte si balíček Node.js Azure hello.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-111">toouse Service Bus, download hello Node.js Azure package.</span></span> <span data-ttu-id="a7f9a-112">Tento balíček obsahuje sadu knihoven, které komunikují s hello služby REST pro Service Bus.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-112">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="a7f9a-113">Použijte uzel balíček správce (NPM) tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="a7f9a-113">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="a7f9a-114">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows) **Terminálové** (Mac), nebo **Bash** (Unix), přejděte toohello složku, kde jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-114">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="a7f9a-115">Typ **npm nainstalujte azure** v hello příkazové okno, které by měl způsobit hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-115">Type **npm install azure** in hello command window, which should result in hello following output:</span></span>

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
3. <span data-ttu-id="a7f9a-116">Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="a7f9a-117">V této složce najít **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess témat sběrnice Service Bus.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-117">Inside that folder find the **azure** package, which contains hello libraries you need tooaccess Service Bus topics.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="a7f9a-118">Import modulu hello</span><span class="sxs-lookup"><span data-stu-id="a7f9a-118">Import hello module</span></span>
<span data-ttu-id="a7f9a-119">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="a7f9a-120">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="a7f9a-120">Set up a Service Bus connection</span></span>
<span data-ttu-id="a7f9a-121">Hello Azure modul čte proměnné prostředí hello `AZURE_SERVICEBUS_NAMESPACE` a `AZURE_SERVICEBUS_ACCESS_KEY` informace požadované tooconnect tooService sběrnice.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-121">hello Azure module reads hello environment variables `AZURE_SERVICEBUS_NAMESPACE` and `AZURE_SERVICEBUS_ACCESS_KEY` for information required tooconnect tooService Bus.</span></span> <span data-ttu-id="a7f9a-122">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-122">If these environment variables are not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="a7f9a-123">Příklad nastavení hello proměnných prostředí pro cloudové služby Azure, naleznete v části [Node.js Cloudová služba se úložiště][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="a7f9a-123">For an example of setting hello environment variables for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="a7f9a-124">Příklad nastavení hello proměnných prostředí pro web Azure, naleznete v části [webovou aplikaci Node.js s úložištěm][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="a7f9a-124">For an example of setting hello environment variables for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="a7f9a-125">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="a7f9a-125">Create a topic</span></span>
<span data-ttu-id="a7f9a-126">Hello **ServiceBusService** objekt vám umožní toowork s tématy.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-126">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="a7f9a-127">Následující kód vytvoří **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-127">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="a7f9a-128">Přidejte do horní části hello **server.js** souboru po hello příkaz tooimport hello azure modul:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-128">Add it near the top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="a7f9a-129">Při volání `createTopicIfNotExists` na hello **ServiceBusService** objektu hello určena, bude vrácen tématu (pokud existuje), nebo se vytvoří nové téma se zadaným názvem hello.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-129">By calling `createTopicIfNotExists` on hello **ServiceBusService** object, hello specified topic will be returned (if it exists,) or a new topic with hello specified name will be created.</span></span> <span data-ttu-id="a7f9a-130">Hello následující kód používá `createTopicIfNotExists` toocreate nebo připojení toohello téma s názvem `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-130">hello following code uses `createTopicIfNotExists` toocreate or connect toohello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

<span data-ttu-id="a7f9a-131">Hello `createServiceBusService` metoda také podporuje další možnosti, které umožňují toooverride výchozí téma nastavení jako hodnota time to live zpráva nebo téma maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-131">hello `createServiceBusService` method also supports additional options, which enable you toooverride default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="a7f9a-132">Hello následující příklad ilustruje hello tématu maximální velikost too5GB s časem toolive 1 minuta:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-132">hello following example sets hello maximum topic size too5GB with a time toolive of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="a7f9a-133">Filtry</span><span class="sxs-lookup"><span data-stu-id="a7f9a-133">Filters</span></span>
<span data-ttu-id="a7f9a-134">Volitelné filtrování operace může být použité toooperations provádí pomocí **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-134">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="a7f9a-135">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-135">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="a7f9a-136">Po provedení předzpracování na možnosti hello požadavku, hello volání metod `next`, předání zpětné volání s hello následující podpis:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-136">After performing preprocessing on hello request options, hello method calls `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="a7f9a-137">V této zpětného volání a po zpracování hello `returnObject` (hello odpovědi ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo vyvolat `finalCallback` , jinak hodnota tooend hello volání služby.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-137">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or invoke `finalCallback` otherwise, tooend hello service invocation.</span></span>

<span data-ttu-id="a7f9a-138">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-138">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="a7f9a-139">vytvoří následující Hello **ServiceBusService** objekt, který používá hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-139">hello following creates a **ServiceBusService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a><span data-ttu-id="a7f9a-140">Vytvořte odběr</span><span class="sxs-lookup"><span data-stu-id="a7f9a-140">Create subscriptions</span></span>
<span data-ttu-id="a7f9a-141">Odběry témat taky jsou vytvořeny pomocí hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-141">Topic subscriptions are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="a7f9a-142">Odběry mají názvy a můžou mít volitelné filtry, které omezuje hello sadu zpráv doručených virtuální fronty odběru toohello.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-142">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="a7f9a-143">Odběry jsou trvalé a bude pokračovat tooexist, dokud nebudou buď jejich nebo hello téma jejich jsou přidruženy, budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-143">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="a7f9a-144">Pokud vaše aplikace obsahuje logiku toocreate předplatné, musí nejprve zkontrolujte, zda hello předplatné už existuje s použitím `getSubscription` metoda.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-144">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using the `getSubscription` method.</span></span>
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="a7f9a-145">Vytvoření odběru s filtrem (MatchAll) výchozí hello</span><span class="sxs-lookup"><span data-stu-id="a7f9a-145">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="a7f9a-146">Hello **MatchAll** filtr je hello výchozí filtr, který se používá v případě, že při vytvoření nového předplatného je zadán žádný filtr.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-146">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="a7f9a-147">Když hello **MatchAll** filtr se používá, všechny zprávy publikované toohello tématu ukládány do virtuální fronty odběru.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-147">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="a7f9a-148">Hello následující příklad vytvoří odběr s názvem "AllMessages" a používá hello výchozí **MatchAll** filtru.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-148">hello following example creates a subscription named 'AllMessages' and uses hello default **MatchAll** filter.</span></span>

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="a7f9a-149">Vytvoření odběru s filtry</span><span class="sxs-lookup"><span data-stu-id="a7f9a-149">Create subscriptions with filters</span></span>
<span data-ttu-id="a7f9a-150">Můžete taky vytvořit filtry, které vám umožňují tooscope příjem zpráv odeslaných tooa tématu by měla objeví v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-150">You can also create filters that allow you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="a7f9a-151">je technologie Hello nejflexibilnější filtr, který odběry podporují **SqlFilter**, který implementuje je podmnožinou SQL92.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-151">hello most flexible type of filter supported by subscriptions is the **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="a7f9a-152">Filtry SQL pracují hello vlastnosti hello zpráv, které jsou publikované toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-152">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="a7f9a-153">Další podrobnosti o hello výrazy, které lze použít s filtrem SQL, projděte si hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-153">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="a7f9a-154">Filtry lze přidat předplatné tooa pomocí hello `createRule` metoda hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-154">Filters can be added tooa subscription by using hello `createRule` method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="a7f9a-155">Tato metoda umožňuje přidat nové předplatné tooan existující filtry.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-155">This method allows you to add new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="a7f9a-156">Protože se automaticky použije výchozí filtr hello tooall nových předplatných, je nutné nejprve odebrat hello výchozí filtr nebo **MatchAll** přepíše ostatní filtry, můžete určit.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-156">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="a7f9a-157">Hello výchozí pravidla můžete odebrat pomocí hello `deleteRule` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-157">You can remove hello default rule by using hello `deleteRule` method of the **ServiceBusService** object.</span></span>
>
>

<span data-ttu-id="a7f9a-158">Hello následující příklad vytvoří odběr s názvem `HighMessages` s **SqlFilter** který vybere jen zprávy, které mají vlastní `messagenumber` vlastnost větší než 3:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-158">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

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

<span data-ttu-id="a7f9a-159">Podobně hello následující příklad vytvoří odběr s názvem `LowMessages` s **SqlFilter** který vybere jen zprávy, které mají `messagenumber` vlastnost menší než nebo rovna too3:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-159">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

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

<span data-ttu-id="a7f9a-160">Když je nyní odeslána zpráva příliš`MyTopic`, bude vždy doručit do příjemci toohello `AllMessages` odběr tématu a odběru selektivně tooreceivers toohello `HighMessages` a `LowMessages` odběry témat (v závislosti na obsahu zprávy hello).</span><span class="sxs-lookup"><span data-stu-id="a7f9a-160">When a message is now sent too`MyTopic`, it will always be delivered to receivers subscribed toohello `AllMessages` topic subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="how-toosend-messages-tooa-topic"></a><span data-ttu-id="a7f9a-161">Jak toosend zprávy tooa tématu</span><span class="sxs-lookup"><span data-stu-id="a7f9a-161">How toosend messages tooa topic</span></span>
<span data-ttu-id="a7f9a-162">toosend tématu Service Bus zprávu tooa, musí vaše aplikace používat `sendTopicMessage` metoda hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-162">toosend a message tooa Service Bus topic, your application must use the `sendTopicMessage` method of hello **ServiceBusService** object.</span></span>
<span data-ttu-id="a7f9a-163">Témata sběrnice tooService jsou odesílány zprávy **BrokeredMessage** objekty.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-163">Messages sent tooService Bus topics are **BrokeredMessage** objects.</span></span>
<span data-ttu-id="a7f9a-164">**BrokeredMessage** objekty mají sadu standardních vlastností (jako například `Label` a `TimeToLive`), slovník, který je použité toohold vlastní vlastnosti specifické pro aplikace a tělo dat řetězce.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-164">**BrokeredMessage** objects have a set of standard properties (such as `Label` and `TimeToLive`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="a7f9a-165">Aplikace může nastavit hello textu hello zprávy pomocí předání řetězcovou hodnotu hello `sendTopicMessage` a potřebné standardní vlastnosti vyplní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-165">An application can set hello body of hello message by passing a string value to hello `sendTopicMessage` and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="a7f9a-166">Hello následující příklad ukazuje, jak toosend pět zkušebních zpráv do `MyTopic`.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-166">hello following example demonstrates how toosend five test messages to `MyTopic`.</span></span> <span data-ttu-id="a7f9a-167">Všimněte si, že hello `messagenumber` hodnotu vlastnosti každé zprávy se liší na hello iteraci smyčky hello (to určí, které odběry ji přijmou):</span><span class="sxs-lookup"><span data-stu-id="a7f9a-167">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

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

<span data-ttu-id="a7f9a-168">Témata Service Bus podporují maximální velikost zprávy 256 kB v hello [úrovně Standard](service-bus-premium-messaging.md) a 1 MB hello [úroveň Premium](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="a7f9a-168">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="a7f9a-169">Hello hlavičky, která zahrnuje hello standard a vlastnosti vlastní aplikace, může mít maximální velikost 64 KB.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-169">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="a7f9a-170">Neexistuje žádné omezení na hello počet zpráv držených v tématu, ale není na hello celková velikost hello zpráv držených v tématu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-170">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="a7f9a-171">Velikost tématu se definuje při vytvoření, maximální limit je 5 GB.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-171">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="a7f9a-172">Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="a7f9a-172">Receive messages from a subscription</span></span>
<span data-ttu-id="a7f9a-173">Přijímání zpráv z odběru pomocí `receiveSubscriptionMessage` metodu hello **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-173">Messages are received from a subscription using the `receiveSubscriptionMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="a7f9a-174">Ve výchozím nastavení jsou odstraněny zprávy z předplatného hello jsou pro čtení; ale můžete číst (funkce Náhled) a uzamčení uvítací zprávu bez odstranění z předplatného hello podle nastavení volitelný parametr hello `isPeekLock` příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-174">By default, messages are deleted from hello subscription as they are read; however, you can read (peek) and lock hello message without deleting it from hello subscription by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="a7f9a-175">výchozí chování Hello čtení a odstraňování uvítací zprávu jako součást operace příjmu je nejjednodušší model hello a funguje nejlépe pro scénáře, ve kterých aplikace může tolerovat hello události selhání se zpráva nezpracuje.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-175">hello default behavior of reading and deleting hello message as part of the receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="a7f9a-176">toounderstand, vezměte v úvahu scénář, ve kterém hello problémy příjemce přijímání požadavků a pak dojde k chybě před zpracováním ho.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-176">toounderstand this, consider a scenario in which the consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="a7f9a-177">Protože Service Bus bude označena hello zprávu jako spotřebovávanou, pak když aplikace hello restartuje a začne znovu přijímat zprávy, ji budou neuskutečnily uvítací zprávu, která byla spotřebované předchozí toohello havárií.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="a7f9a-178">Pokud hello `isPeekLock` parametr je nastaven příliš**true**, přijímat hello stane dvoufázového operace, takže je možné toosupport aplikace, které nemůžou tolerovat vynechání zpráv.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-178">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="a7f9a-179">Když Service Bus přijme požadavek, najde hello další zprávy toobe využívat, uzamkne ji tooprevent jinými spotřebiteli a vrátí ji toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span>
<span data-ttu-id="a7f9a-180">Po hello aplikace dokončí zpracování zprávy hello (nebo ji bezpečně uloží pro pozdější zpracování), tím potvrdí dokončení druhé fáze přijetí hello volání **deleteMessage** metoda a poskytování toobe zpráv Odstranit jako parametr.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of the receive process by calling **deleteMessage** method and providing the message toobe deleted as a parameter.</span></span> <span data-ttu-id="a7f9a-181">Hello **deleteMessage** metoda bude označit uvítací zprávu jako spotřebovávanou a jeho odebrání ze hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-181">hello **deleteMessage** method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="a7f9a-182">Hello následující příklad ukazuje, jak můžete obdržet zprávy a zpracování pomocí `receiveSubscriptionMessage`.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-182">hello following example demonstrates how messages can be received and processed using `receiveSubscriptionMessage`.</span></span> <span data-ttu-id="a7f9a-183">Hello příklad nejprve obdrží a odstraní zprávu z hello 'LowMessages' předplatného a pak obdrží zprávu z hello 'HighMessages' předplatnému pomocí `isPeekLock` nastavit tootrue.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-183">hello example first receives and deletes a message from hello 'LowMessages' subscription, and then receives a message from hello 'HighMessages' subscription using `isPeekLock` set tootrue.</span></span> <span data-ttu-id="a7f9a-184">Pak odstraní hello zprávu pomocí `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-184">It then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="a7f9a-185">Jak toohandle aplikace spadne a nečitelných zpráv</span><span class="sxs-lookup"><span data-stu-id="a7f9a-185">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="a7f9a-186">Service Bus poskytuje funkce toohelp, který elegantně zotavit z chyb v aplikaci nebo vyřešit potíže se zpracováním zprávy.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-186">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="a7f9a-187">Pokud přijímající aplikace nemůže tooprocess hello zprávy z nějakého důvodu a potom ji můžete volat hello `unlockMessage` metodu **ServiceBusService** objektu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-187">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="a7f9a-188">To bude způsobit toounlock sběrnice zpráv v rámci předplatného hello a nastavit jej jako dostupné toobe přijetí, buď pomocí hello stejné využívání aplikací nebo jinou spotřebitelskou aplikací.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-188">This will cause Service Bus toounlock the message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="a7f9a-189">Je také vypršení časového limitu zpráva uzamčená v odběru, a pokud aplikace hello selže tooprocess uvítací zprávu před hello zámku vyprší časový limit (například pokud hello aplikace spadne), pak uvítací zprávu odemkne Service Bus automaticky a je k dispozici toobe přijetí.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-189">There is also a timeout associated with a message locked within the subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="a7f9a-190">V hello událost, která hello aplikace spadne po zpracování uvítací zprávu, ale před hello `deleteMessage` metoda je volána, pak uvítací zprávu bude víckrát toohello aplikace odešle znovu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-190">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="a7f9a-191">To se často označuje jako *zpracování nejméně jednou*, který je každá zpráva se zpracuje alespoň jednou, ale v určitých situacích hello může doručit víckrát.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-191">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="a7f9a-192">Pokud hello scénář nemůže tolerovat zpracování duplicitní, měli vývojáři aplikace přidat další logiku tootheir aplikace toohandle víckrát doručené zprávy.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-192">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="a7f9a-193">To se často opírá **MessageId** vlastnost hello zprávy, která zůstane konstantní mezi pokusy o doručení.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-193">This is often achieved using the **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="a7f9a-194">Odstranění témat a odběrů</span><span class="sxs-lookup"><span data-stu-id="a7f9a-194">Delete topics and subscriptions</span></span>
<span data-ttu-id="a7f9a-195">Témata a odběry jsou trvalé a musí být explicitně odstranit buď prostřednictvím hello [portál Azure] [ Azure portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-195">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span>
<span data-ttu-id="a7f9a-196">Hello následující příklad ukazuje, jak s názvem toodelete hello tématu `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-196">hello following example demonstrates how toodelete hello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

<span data-ttu-id="a7f9a-197">Odstraní téma také odstraní všechny odběry, které jsou registrovány hello tématu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-197">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="a7f9a-198">Odběry se taky dají odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-198">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="a7f9a-199">Následující příklad ukazuje, jak toodelete předplatné s názvem `HighMessages` z hello `MyTopic` tématu:</span><span class="sxs-lookup"><span data-stu-id="a7f9a-199">The following example shows how toodelete a subscription named `HighMessages` from hello `MyTopic` topic:</span></span>

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a><span data-ttu-id="a7f9a-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7f9a-200">Next Steps</span></span>
<span data-ttu-id="a7f9a-201">Teď, když jste se naučili základy hello témat Service Bus, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-201">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="a7f9a-202">V tématu [fronty, témata a odběry][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="a7f9a-202">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="a7f9a-203">Reference k rozhraní API pro [SqlFilter][SqlFilter]</span><span class="sxs-lookup"><span data-stu-id="a7f9a-203">API reference for [SqlFilter][SqlFilter].</span></span>
* <span data-ttu-id="a7f9a-204">Navštivte hello [Azure SDK pro uzel] [ Azure SDK for Node] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a7f9a-204">Visit hello [Azure SDK for Node][Azure SDK for Node] repository on GitHub.</span></span>

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[vytvořit a nasadit tooan aplikace Node.js webu Azure]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
