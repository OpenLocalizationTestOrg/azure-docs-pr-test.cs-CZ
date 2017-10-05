---
title: "Azure Functions fronty úložiště vazby | Microsoft Docs"
description: "Pochopit, jak používat Azure Storage triggerů a vazeb v Azure Functions."
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
ms.openlocfilehash: e007acd75a2210d54f512e2c6698c90919f0fcd2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-queue-storage-bindings"></a><span data-ttu-id="7f1c0-104">Azure Queue Storage funkce vazby</span><span class="sxs-lookup"><span data-stu-id="7f1c0-104">Azure Functions Queue Storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="7f1c0-105">Tento článek popisuje, jak nakonfigurovat a vazby Azure Queue storage kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-105">This article describes how to configure and code Azure Queue storage bindings in Azure Functions.</span></span> <span data-ttu-id="7f1c0-106">Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-106">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="7f1c0-107">Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="7f1c0-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a><span data-ttu-id="7f1c0-108">Aktivace fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="7f1c0-108">Queue storage trigger</span></span>
<span data-ttu-id="7f1c0-109">Aktivační událost Azure Queue storage vám umožňuje monitorovat fronty úložiště pro nové zprávy a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-109">The Azure Queue storage trigger enables you to monitor a queue storage for new messages and react to them.</span></span> 

<span data-ttu-id="7f1c0-110">Zadejte aktivační události fronty pomocí **integrací** na portálu funkce.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-110">Define a queue trigger using the **Integrate** tab in the Functions portal.</span></span> <span data-ttu-id="7f1c0-111">Vytvoří následující definice v portálu **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-111">The portal creates the following definition in the  **bindings** section of *function.json*:</span></span>

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="7f1c0-112">`connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-112">The `connection` property must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="7f1c0-113">Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-113">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<span data-ttu-id="7f1c0-114">Další nastavení lze zadat do [host.json soubor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) a vyladit aktivační procedury fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-114">Additional settings can be provided in a [host.json file](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) to further fine-tune queue storage triggers.</span></span> <span data-ttu-id="7f1c0-115">Můžete například změnit fronty v host.json interval dotazování.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-115">For example, you can change the queue polling interval in host.json.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a><span data-ttu-id="7f1c0-116">Pomocí aktivační procedury fronty</span><span class="sxs-lookup"><span data-stu-id="7f1c0-116">Using a queue trigger</span></span>
<span data-ttu-id="7f1c0-117">Ve funkcích Node.js, přístup k frontě dat pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-117">In Node.js functions, access the queue data using `context.bindings.<name>`.</span></span>


<span data-ttu-id="7f1c0-118">V rozhraní .NET funkce, přístup k datové části fronty pomocí parametru metody, jako třeba `CloudQueueMessage paramName`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-118">In .NET functions, access the queue payload using a method parameter such as `CloudQueueMessage paramName`.</span></span> <span data-ttu-id="7f1c0-119">Zde `paramName` je hodnota zadaná v [konfigurace aktivační události](#trigger).</span><span class="sxs-lookup"><span data-stu-id="7f1c0-119">Here, `paramName` is the value you specified in the [trigger configuration](#trigger).</span></span> <span data-ttu-id="7f1c0-120">Zprávy ve frontě lze deserializovat na některý z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-120">The queue message can be deserialized to any of the following types:</span></span>

* <span data-ttu-id="7f1c0-121">Objektů POCO objekt.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-121">POCO object.</span></span> <span data-ttu-id="7f1c0-122">Použijte, pokud datová část fronty je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-122">Use if the queue payload is a JSON object.</span></span> <span data-ttu-id="7f1c0-123">Modul runtime funkce deserializuje datovou část do objektů POCO objektu.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-123">The Functions runtime deserializes the payload into the POCO object.</span></span> 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a><span data-ttu-id="7f1c0-124">Metadata frontě aktivační události</span><span class="sxs-lookup"><span data-stu-id="7f1c0-124">Queue trigger metadata</span></span>
<span data-ttu-id="7f1c0-125">Aktivační událost fronty nabízí několik vlastností metadat.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-125">The queue trigger provides several metadata properties.</span></span> <span data-ttu-id="7f1c0-126">Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-126">These properties can be used as part of binding expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="7f1c0-127">Hodnoty mají stejnou sémantiku jako [ `CloudQueueMessage` ].</span><span class="sxs-lookup"><span data-stu-id="7f1c0-127">The values have the same semantics as [`CloudQueueMessage`].</span></span>

* <span data-ttu-id="7f1c0-128">**QueueTrigger** -datové frontu (pokud platný řetězec)</span><span class="sxs-lookup"><span data-stu-id="7f1c0-128">**QueueTrigger** - queue payload (if a valid string)</span></span>
* <span data-ttu-id="7f1c0-129">**DequeueCount** – typ `int`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-129">**DequeueCount** - Type `int`.</span></span> <span data-ttu-id="7f1c0-130">Počet časy, kdy se tato zpráva má bylo vyřazeno z fronty.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-130">The number of times this message has been dequeued.</span></span>
* <span data-ttu-id="7f1c0-131">**ExpirationTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-131">**ExpirationTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="7f1c0-132">Čas, kdy vyprší platnost zprávy.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-132">The time that the message expires.</span></span>
* <span data-ttu-id="7f1c0-133">**ID** – typ `string`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-133">**Id** - Type `string`.</span></span> <span data-ttu-id="7f1c0-134">ID zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="7f1c0-134">Queue message ID.</span></span>
* <span data-ttu-id="7f1c0-135">**InsertionTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-135">**InsertionTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="7f1c0-136">Doba, která zpráva byla přidána do fronty.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-136">The time that the message was added to the queue.</span></span>
* <span data-ttu-id="7f1c0-137">**NextVisibleTime** – typ `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-137">**NextVisibleTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="7f1c0-138">Čas, zprávy budou vedle viditelné.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-138">The time that the message will next be visible.</span></span>
* <span data-ttu-id="7f1c0-139">**Vlastnosti PopReceipt** – typ `string`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-139">**PopReceipt** - Type `string`.</span></span> <span data-ttu-id="7f1c0-140">Pop přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-140">The message's pop receipt.</span></span>

<span data-ttu-id="7f1c0-141">V tématu Jak používat fronty metadat v [aktivační událost vzorku](#triggersample).</span><span class="sxs-lookup"><span data-stu-id="7f1c0-141">See how to use the queue metadata in [Trigger sample](#triggersample).</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="7f1c0-142">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="7f1c0-142">Trigger sample</span></span>
<span data-ttu-id="7f1c0-143">Předpokládejme, že máte následující function.json, která definuje aktivační procedury fronty:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-143">Suppose you have the following function.json that defines a queue trigger:</span></span>

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

<span data-ttu-id="7f1c0-144">Viz ukázka pro specifický jazyk, který načte a protokoly ve frontě metadat.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-144">See the language-specific sample that retrieves and logs queue metadata.</span></span>

* [<span data-ttu-id="7f1c0-145">C#</span><span class="sxs-lookup"><span data-stu-id="7f1c0-145">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="7f1c0-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="7f1c0-146">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="7f1c0-147">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="7f1c0-147">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="7f1c0-148">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="7f1c0-148">Trigger sample in Node.js</span></span>

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

### <a name="handling-poison-queue-messages"></a><span data-ttu-id="7f1c0-149">Zpracování poškozených fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="7f1c0-149">Handling poison queue messages</span></span>
<span data-ttu-id="7f1c0-150">Pokud se nezdaří aktivační funkce fronty, Azure Functions opakuje této funkce až pětkrát pro danou frontu zprávy, včetně první pokus.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-150">When a queue trigger function fails, Azure Functions retries that function up to five times for a given queue message, including the first try.</span></span> <span data-ttu-id="7f1c0-151">Pokud selžou všechny pět pokusy, functions runtime přidá zprávu do fronty úložiště s názvem  *&lt;originalqueuename >-poškozených*.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-151">If all five attempts fail, the functions runtime adds a message to a queue storage named *&lt;originalqueuename>-poison*.</span></span> <span data-ttu-id="7f1c0-152">Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-152">You can write a function to process messages from the poison queue by logging them or sending a  notification that manual attention is needed.</span></span> 

<span data-ttu-id="7f1c0-153">Pro zpracování poškozených zpráv ručně, zkontrolujte `dequeueCount` zprávy fronty (v tématu [frontě aktivační události metadata](#meta)).</span><span class="sxs-lookup"><span data-stu-id="7f1c0-153">To handle poison messages manually, check the `dequeueCount` of the queue message (see [Queue trigger metadata](#meta)).</span></span>

<a name="output"></a>

## <a name="queue-storage-output-binding"></a><span data-ttu-id="7f1c0-154">Vazba výstupní fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="7f1c0-154">Queue storage output binding</span></span>
<span data-ttu-id="7f1c0-155">Úložiště Azure queue výstup vazby umožňuje zápis zpráv do fronty.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-155">The Azure queue storage output binding enables you to write messages to a queue.</span></span> 

<span data-ttu-id="7f1c0-156">Definovat pomocí vazby fronty výstupu **integrací** na portálu funkce.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-156">Define a queue output binding using the **Integrate** tab in the Functions portal.</span></span> <span data-ttu-id="7f1c0-157">Vytvoří následující definice v portálu **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-157">The portal creates the following definition in the  **bindings** section of *function.json*:</span></span>

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="7f1c0-158">`connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-158">The `connection` property must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="7f1c0-159">Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-159">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a><span data-ttu-id="7f1c0-160">Pomocí fronty výstup vazby</span><span class="sxs-lookup"><span data-stu-id="7f1c0-160">Using a queue output binding</span></span>
<span data-ttu-id="7f1c0-161">V Node.js funkce, přístup k výstupu fronty pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-161">In Node.js functions, you access the output queue using `context.bindings.<name>`.</span></span>

<span data-ttu-id="7f1c0-162">V rozhraní .NET funkce výstup můžete na některý z následujících typů.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-162">In .NET functions, you can output to any of the following types.</span></span> <span data-ttu-id="7f1c0-163">Pokud je parametr typu `T`, `T` musí být jeden z typů podporovaných výstupu, jako například `string` nebo objektů POCO.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-163">When there is a type parameter `T`, `T` must be one of the supported output types, such as `string` or a POCO.</span></span>

* <span data-ttu-id="7f1c0-164">`out T`(serializovanou jako JSON)</span><span class="sxs-lookup"><span data-stu-id="7f1c0-164">`out T` (serialized as JSON)</span></span>
* `out string`
* `out byte[]`
* <span data-ttu-id="7f1c0-165">`out` [`CloudQueueMessage`]</span><span class="sxs-lookup"><span data-stu-id="7f1c0-165">`out` [`CloudQueueMessage`]</span></span> 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

<span data-ttu-id="7f1c0-166">Návratový typ metody můžete také použít jako výstup vazba.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-166">You can also use the method return type as the output binding.</span></span>

<a name="outputsample"></a>

## <a name="queue-output-sample"></a><span data-ttu-id="7f1c0-167">Ukázka výstupní fronty</span><span class="sxs-lookup"><span data-stu-id="7f1c0-167">Queue output sample</span></span>
<span data-ttu-id="7f1c0-168">Následující *function.json* definuje aktivační procedury HTTP s frontou výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-168">The following *function.json* defines an HTTP trigger with a queue output binding:</span></span>

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

<span data-ttu-id="7f1c0-169">Naleznete v ukázce pro specifický jazyk, který produkuje zprávu fronty s příchozí datové části protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f1c0-169">See the language-specific sample that outputs a queue message with the incoming HTTP payload.</span></span>

* [<span data-ttu-id="7f1c0-170">C#</span><span class="sxs-lookup"><span data-stu-id="7f1c0-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="7f1c0-171">Node.js</span><span class="sxs-lookup"><span data-stu-id="7f1c0-171">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a><span data-ttu-id="7f1c0-172">Ukázka výstupní fronty v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="7f1c0-172">Queue output sample in C#</span></span> #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
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

<span data-ttu-id="7f1c0-173">Chcete-li odeslat více zpráv, použijte `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="7f1c0-173">To send multiple messages, use an `ICollector`:</span></span>

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a><span data-ttu-id="7f1c0-174">Ukázka výstupní fronty v Node.js</span><span class="sxs-lookup"><span data-stu-id="7f1c0-174">Queue output sample in Node.js</span></span>

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

<span data-ttu-id="7f1c0-175">Nebo, pokud chcete odeslat více zpráv</span><span class="sxs-lookup"><span data-stu-id="7f1c0-175">Or, to send multiple messages,</span></span>

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="7f1c0-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f1c0-176">Next steps</span></span>

<span data-ttu-id="7f1c0-177">Příklad, funkci, která používá fronty úložiště triggerů a vazeb, naleznete v části [vytvořit funkci Azure připojené k služby Azure](functions-create-an-azure-connected-function.md).</span><span class="sxs-lookup"><span data-stu-id="7f1c0-177">For an example of a function that uses queue storage triggers and bindings, see [Create an Azure Function connected to an Azure service](functions-create-an-azure-connected-function.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage.]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
