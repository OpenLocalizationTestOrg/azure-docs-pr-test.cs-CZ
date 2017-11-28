---
title: "aaaAzure funkce fronty úložiště vazby | Microsoft Docs"
description: Pochopit, jak se aktivuje toouse Azure Storage a vazeb v Azure Functions.
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 438b4f63e823149072c86fdefa7e15bfd2a2c4df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-queue-storage-bindings"></a><span data-ttu-id="bf238-104">Azure Queue Storage funkce vazby</span><span class="sxs-lookup"><span data-stu-id="bf238-104">Azure Functions Queue Storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="bf238-105">Tento článek popisuje, jak tooconfigure a kód Azure Queue storage vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="bf238-105">This article describes how tooconfigure and code Azure Queue storage bindings in Azure Functions.</span></span> <span data-ttu-id="bf238-106">Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="bf238-106">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="bf238-107">Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="bf238-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a><span data-ttu-id="bf238-108">Aktivace fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="bf238-108">Queue storage trigger</span></span>
<span data-ttu-id="bf238-109">Hello Azure Queue storage aktivační událost umožňuje toomonitor fronty úložiště pro nové zprávy a toothem reagovat.</span><span class="sxs-lookup"><span data-stu-id="bf238-109">hello Azure Queue storage trigger enables you toomonitor a queue storage for new messages and react toothem.</span></span> 

<span data-ttu-id="bf238-110">Zadejte aktivační události fronty pomocí hello **integrací** kartě hello funkce portálu.</span><span class="sxs-lookup"><span data-stu-id="bf238-110">Define a queue trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="bf238-111">Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="bf238-111">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="bf238-112">Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf238-112">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="bf238-113">V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf238-113">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<span data-ttu-id="bf238-114">Další nastavení lze zadat do [host.json soubor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther doladit aktivační procedury fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf238-114">Additional settings can be provided in a [host.json file](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther fine-tune queue storage triggers.</span></span> <span data-ttu-id="bf238-115">Například můžete změnit interval dotazování hello fronty v host.json.</span><span class="sxs-lookup"><span data-stu-id="bf238-115">For example, you can change hello queue polling interval in host.json.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a><span data-ttu-id="bf238-116">Pomocí aktivační procedury fronty</span><span class="sxs-lookup"><span data-stu-id="bf238-116">Using a queue trigger</span></span>
<span data-ttu-id="bf238-117">Ve funkcích Node.js, přístup k hello fronty dat pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="bf238-117">In Node.js functions, access hello queue data using `context.bindings.<name>`.</span></span>


<span data-ttu-id="bf238-118">V rozhraní .NET funkce, přístup k datové části hello fronty pomocí parametru metody, jako třeba `CloudQueueMessage paramName`.</span><span class="sxs-lookup"><span data-stu-id="bf238-118">In .NET functions, access hello queue payload using a method parameter such as `CloudQueueMessage paramName`.</span></span> <span data-ttu-id="bf238-119">Zde `paramName` je hodnota hello jste zadali v hello [konfigurace aktivační události](#trigger).</span><span class="sxs-lookup"><span data-stu-id="bf238-119">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="bf238-120">uvítací zprávu fronty lze deserializovat tooany hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="bf238-120">hello queue message can be deserialized tooany of hello following types:</span></span>

* <span data-ttu-id="bf238-121">Objektů POCO objekt.</span><span class="sxs-lookup"><span data-stu-id="bf238-121">POCO object.</span></span> <span data-ttu-id="bf238-122">Použijte, pokud datová část fronty hello je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="bf238-122">Use if hello queue payload is a JSON object.</span></span> <span data-ttu-id="bf238-123">Hello Functions runtime deserializuje datovou část hello do objektu objektů POCO hello.</span><span class="sxs-lookup"><span data-stu-id="bf238-123">hello Functions runtime deserializes hello payload into hello POCO object.</span></span> 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a><span data-ttu-id="bf238-124">Metadata frontě aktivační události</span><span class="sxs-lookup"><span data-stu-id="bf238-124">Queue trigger metadata</span></span>
<span data-ttu-id="bf238-125">Aktivace fronty Hello nabízí několik vlastností metadat.</span><span class="sxs-lookup"><span data-stu-id="bf238-125">hello queue trigger provides several metadata properties.</span></span> <span data-ttu-id="bf238-126">Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu.</span><span class="sxs-lookup"><span data-stu-id="bf238-126">These properties can be used as part of binding expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="bf238-127">Hello hodnoty mají hello stejnou sémantiku jako [ `CloudQueueMessage` ].</span><span class="sxs-lookup"><span data-stu-id="bf238-127">hello values have hello same semantics as [`CloudQueueMessage`].</span></span>

* <span data-ttu-id="bf238-128">**QueueTrigger** -datové frontu (pokud platný řetězec)</span><span class="sxs-lookup"><span data-stu-id="bf238-128">**QueueTrigger** - queue payload (if a valid string)</span></span>
* <span data-ttu-id="bf238-129">**DequeueCount** – typ `int`.</span><span class="sxs-lookup"><span data-stu-id="bf238-129">**DequeueCount** - Type `int`.</span></span> <span data-ttu-id="bf238-130">počet pokusů, které se tato zpráva byla vyjmutou. Hello.</span><span class="sxs-lookup"><span data-stu-id="bf238-130">hello number of times this message has been dequeued.</span></span>
* <span data-ttu-id="bf238-131">**ExpirationTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="bf238-131">**ExpirationTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="bf238-132">Hello čas vypršení platnosti zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="bf238-132">hello time that hello message expires.</span></span>
* <span data-ttu-id="bf238-133">**ID** – typ `string`.</span><span class="sxs-lookup"><span data-stu-id="bf238-133">**Id** - Type `string`.</span></span> <span data-ttu-id="bf238-134">ID zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="bf238-134">Queue message ID.</span></span>
* <span data-ttu-id="bf238-135">**InsertionTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="bf238-135">**InsertionTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="bf238-136">čas Hello hello zpráva byla přidána toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="bf238-136">hello time that hello message was added toohello queue.</span></span>
* <span data-ttu-id="bf238-137">**NextVisibleTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="bf238-137">**NextVisibleTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="bf238-138">čas Hello hello zprávy budou vedle viditelné.</span><span class="sxs-lookup"><span data-stu-id="bf238-138">hello time that hello message will next be visible.</span></span>
* <span data-ttu-id="bf238-139">**Vlastnosti PopReceipt** – typ `string`.</span><span class="sxs-lookup"><span data-stu-id="bf238-139">**PopReceipt** - Type `string`.</span></span> <span data-ttu-id="bf238-140">uvítací zprávu pop přijetí.</span><span class="sxs-lookup"><span data-stu-id="bf238-140">hello message's pop receipt.</span></span>

<span data-ttu-id="bf238-141">V tématu Jak toouse hello metadata fronty v [aktivační událost vzorku](#triggersample).</span><span class="sxs-lookup"><span data-stu-id="bf238-141">See how toouse hello queue metadata in [Trigger sample](#triggersample).</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="bf238-142">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="bf238-142">Trigger sample</span></span>
<span data-ttu-id="bf238-143">Předpokládejme, že máte hello následující function.json, který definuje aktivační procedury fronty:</span><span class="sxs-lookup"><span data-stu-id="bf238-143">Suppose you have hello following function.json that defines a queue trigger:</span></span>

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

<span data-ttu-id="bf238-144">V tématu vzorku hello konkrétní jazyk, který načte a protokoly metadata fronty.</span><span class="sxs-lookup"><span data-stu-id="bf238-144">See hello language-specific sample that retrieves and logs queue metadata.</span></span>

* [<span data-ttu-id="bf238-145">C#</span><span class="sxs-lookup"><span data-stu-id="bf238-145">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="bf238-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="bf238-146">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="bf238-147">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="bf238-147">Trigger sample in C#</span></span> #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="bf238-148">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="bf238-148">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a><span data-ttu-id="bf238-149">Zpracování poškozených fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="bf238-149">Handling poison queue messages</span></span>
<span data-ttu-id="bf238-150">Pokud se nezdaří aktivační funkce fronty, Azure Functions opakuje této funkce si toofive, které časy dané frontě zprávy, včetně hello nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="bf238-150">When a queue trigger function fails, Azure Functions retries that function up toofive times for a given queue message, including hello first try.</span></span> <span data-ttu-id="bf238-151">Pokud selžou všechny pět pokusy, hello functions runtime přidá zprávu tooa fronty úložiště s názvem  *&lt;originalqueuename >-poškozených*.</span><span class="sxs-lookup"><span data-stu-id="bf238-151">If all five attempts fail, hello functions runtime adds a message tooa queue storage named *&lt;originalqueuename>-poison*.</span></span> <span data-ttu-id="bf238-152">Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="bf238-152">You can write a function tooprocess messages from hello poison queue by logging them or sending a  notification that manual attention is needed.</span></span> 

<span data-ttu-id="bf238-153">poškozených zpráv toohandle ručně, zkontrolujte hello `dequeueCount` hello zprávy fronty (v tématu [frontě aktivační události metadata](#meta)).</span><span class="sxs-lookup"><span data-stu-id="bf238-153">toohandle poison messages manually, check hello `dequeueCount` of hello queue message (see [Queue trigger metadata](#meta)).</span></span>

<a name="output"></a>

## <a name="queue-storage-output-binding"></a><span data-ttu-id="bf238-154">Vazba výstupní fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="bf238-154">Queue storage output binding</span></span>
<span data-ttu-id="bf238-155">Vazba vám umožní toowrite zprávy tooa fronty výstupu Hello fronty Azure storage.</span><span class="sxs-lookup"><span data-stu-id="bf238-155">hello Azure queue storage output binding enables you toowrite messages tooa queue.</span></span> 

<span data-ttu-id="bf238-156">Definování fronty výstupu vazby pomocí hello **integrací** kartě hello funkce portálu.</span><span class="sxs-lookup"><span data-stu-id="bf238-156">Define a queue output binding using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="bf238-157">Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="bf238-157">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="bf238-158">Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf238-158">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="bf238-159">V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf238-159">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a><span data-ttu-id="bf238-160">Pomocí fronty výstup vazby</span><span class="sxs-lookup"><span data-stu-id="bf238-160">Using a queue output binding</span></span>
<span data-ttu-id="bf238-161">Ve funkcích Node.js přistupujete hello výstupní fronty pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="bf238-161">In Node.js functions, you access hello output queue using `context.bindings.<name>`.</span></span>

<span data-ttu-id="bf238-162">V rozhraní .NET funkce můžete výstup tooany hello následující typy.</span><span class="sxs-lookup"><span data-stu-id="bf238-162">In .NET functions, you can output tooany of hello following types.</span></span> <span data-ttu-id="bf238-163">Pokud je parametr typu `T`, `T` musí být jeden z typů výstup hello podporována, jako například `string` nebo objektů POCO.</span><span class="sxs-lookup"><span data-stu-id="bf238-163">When there is a type parameter `T`, `T` must be one of hello supported output types, such as `string` or a POCO.</span></span>

* <span data-ttu-id="bf238-164">`out T`(serializovanou jako JSON)</span><span class="sxs-lookup"><span data-stu-id="bf238-164">`out T` (serialized as JSON)</span></span>
* `out string`
* `out byte[]`
* <span data-ttu-id="bf238-165">`out` [`CloudQueueMessage`]</span><span class="sxs-lookup"><span data-stu-id="bf238-165">`out` [`CloudQueueMessage`]</span></span> 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

<span data-ttu-id="bf238-166">Návratový typ metody hello můžete také použít jako hello výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="bf238-166">You can also use hello method return type as hello output binding.</span></span>

<a name="outputsample"></a>

## <a name="queue-output-sample"></a><span data-ttu-id="bf238-167">Ukázka výstupní fronty</span><span class="sxs-lookup"><span data-stu-id="bf238-167">Queue output sample</span></span>
<span data-ttu-id="bf238-168">Následující Hello *function.json* definuje aktivační procedury HTTP s frontou výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="bf238-168">hello following *function.json* defines an HTTP trigger with a queue output binding:</span></span>

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

<span data-ttu-id="bf238-169">V tématu vzorku hello konkrétní jazyk, který výstupy zprávu fronty s hello příchozí datové části protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf238-169">See hello language-specific sample that outputs a queue message with hello incoming HTTP payload.</span></span>

* [<span data-ttu-id="bf238-170">C#</span><span class="sxs-lookup"><span data-stu-id="bf238-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="bf238-171">Node.js</span><span class="sxs-lookup"><span data-stu-id="bf238-171">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a><span data-ttu-id="bf238-172">Ukázka výstupní fronty v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="bf238-172">Queue output sample in C#</span></span> #

```cs
// C# example of HTTP trigger binding tooa custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

<span data-ttu-id="bf238-173">toosend více zpráv používat `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="bf238-173">toosend multiple messages, use an `ICollector`:</span></span>

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a><span data-ttu-id="bf238-174">Ukázka výstupní fronty v Node.js</span><span class="sxs-lookup"><span data-stu-id="bf238-174">Queue output sample in Node.js</span></span>

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

<span data-ttu-id="bf238-175">Nebo toosend více zpráv</span><span class="sxs-lookup"><span data-stu-id="bf238-175">Or, toosend multiple messages,</span></span>

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="bf238-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf238-176">Next steps</span></span>

<span data-ttu-id="bf238-177">Příklad, funkci, která používá fronty úložiště triggerů a vazeb, naleznete v části [vytvořit tooan funkce Azure připojení služby Azure](functions-create-an-azure-connected-function.md).</span><span class="sxs-lookup"><span data-stu-id="bf238-177">For an example of a function that uses queue storage triggers and bindings, see [Create an Azure Function connected tooan Azure service](functions-create-an-azure-connected-function.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage.]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
