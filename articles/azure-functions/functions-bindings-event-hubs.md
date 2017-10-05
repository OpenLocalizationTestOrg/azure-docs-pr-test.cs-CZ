---
title: Azure Event Hubs funkce vazby | Microsoft Docs
description: "Pochopit, jak používat Azure Event Hubs vazby v Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: 19021bef8b7156b3049f43b0275c0ed0c6b22514
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="97a90-104">Azure Event Hubs funkce vazby</span><span class="sxs-lookup"><span data-stu-id="97a90-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="97a90-105">Tento článek vysvětluje, jak konfigurovat a používat [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) vazby pro Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="97a90-105">This article explains how to configure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="97a90-106">Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="97a90-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="97a90-107">Pokud nový Azure Event Hubs, podívejte se [Přehled služby Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="97a90-107">If you are new to Azure Event Hubs, see the [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="97a90-108">Aktivační událost rozbočovače</span><span class="sxs-lookup"><span data-stu-id="97a90-108">Event hub trigger</span></span>
<span data-ttu-id="97a90-109">Použijte aktivační událost Event Hubs reagovat na událost odeslaná datového proudu událostí centra událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-109">Use the Event Hubs trigger to respond to an event sent to an event hub event stream.</span></span> <span data-ttu-id="97a90-110">Musíte mít přístup pro čtení do centra událostí vytvořit aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="97a90-110">You must have read access to the event hub to set up the trigger.</span></span>

<span data-ttu-id="97a90-111">Aktivační událost Event Hubs funkce používá následující objekt JSON v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="97a90-111">The Event Hubs function trigger uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

<span data-ttu-id="97a90-112">`consumerGroup`je volitelná vlastnost lze nastavit [skupiny příjemců](../event-hubs/event-hubs-features.md#event-consumers) používá přihlásit k odběru událostí v centru.</span><span class="sxs-lookup"><span data-stu-id="97a90-112">`consumerGroup` is an optional property used to set the [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used to subscribe to events in the hub.</span></span> <span data-ttu-id="97a90-113">Pokud tento parametr vynechán, `$Default` skupina uživatelů slouží.</span><span class="sxs-lookup"><span data-stu-id="97a90-113">If omitted, the `$Default` consumer group is used.</span></span>  
<span data-ttu-id="97a90-114">`connection`musí být název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="97a90-114">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="97a90-115">Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-115">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="97a90-116">Tento připojovací řetězec musí mít alespoň oprávnění ke čtení pro aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="97a90-116">This connection string must have at least read permissions to activate the trigger.</span></span>

<span data-ttu-id="97a90-117">[Další nastavení](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) lze zadat do souboru host.json další vyladění aktivační události Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="97a90-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file to further fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="97a90-118">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="97a90-118">Trigger usage</span></span>
<span data-ttu-id="97a90-119">Když se aktivuje funkce aktivační událost Event Hubs, zprávu, která ji spouští je předán do funkce jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="97a90-119">When an Event Hubs trigger function is triggered, the message that triggers it is passed into the function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="97a90-120">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="97a90-120">Trigger sample</span></span>
<span data-ttu-id="97a90-121">Předpokládejme, že máte následující služby Event Hubs aktivovat v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="97a90-121">Suppose you have the following Event Hubs trigger in the `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="97a90-122">Naleznete v ukázce pro specifický jazyk, který zaznamenává tělo zprávy aktivační události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="97a90-122">See the language-specific sample that logs the message body of the event hub trigger.</span></span>

* [<span data-ttu-id="97a90-123">C#</span><span class="sxs-lookup"><span data-stu-id="97a90-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="97a90-124">F#</span><span class="sxs-lookup"><span data-stu-id="97a90-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="97a90-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="97a90-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="97a90-126">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="97a90-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="97a90-127">Můžete také získat událost jako [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objekt, který umožňuje přístup k metadatům událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-127">You can also receive the event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access to the event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="97a90-128">Chcete-li přijímat události v dávce, změňte podpis metody k `string[]` nebo `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="97a90-128">To receive events in a batch, change the method signature to `string[]` or `EventData[]`.</span></span>

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="97a90-129">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="97a90-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="97a90-130">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="97a90-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="97a90-131">Služba Event Hubs výstup vazby</span><span class="sxs-lookup"><span data-stu-id="97a90-131">Event Hubs output binding</span></span>
<span data-ttu-id="97a90-132">Použijte službu Event Hubs výstup vytvoření vazby na zapsat události do datového proudu událostí centra událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-132">Use the Event Hubs output binding to write events to an event hub event stream.</span></span> <span data-ttu-id="97a90-133">Musíte mít oprávnění odesílat do centra událostí zapsat události do ní.</span><span class="sxs-lookup"><span data-stu-id="97a90-133">You must have send permission to an event hub to write events to it.</span></span>

<span data-ttu-id="97a90-134">Vazba výstup používá následující objekt JSON v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="97a90-134">The output binding uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="97a90-135">`connection`musí být název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="97a90-135">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="97a90-136">Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-136">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="97a90-137">Tento připojovací řetězec musí mít oprávnění pro odesílání k odeslání zprávy do datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="97a90-137">This connection string must have send permissions to send the message to the event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="97a90-138">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="97a90-138">Output usage</span></span>
<span data-ttu-id="97a90-139">V této části se dozvíte, jak používat službu Event Hubs výstupu vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="97a90-139">This section shows you how to use your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="97a90-140">Výstup můžete zprávy do centra událostí nakonfigurované s následujícími typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="97a90-140">You can output messages to the configured event hub with the following parameter types:</span></span>

* `out string`
* <span data-ttu-id="97a90-141">`ICollector<string>`(pro výstup více zpráv)</span><span class="sxs-lookup"><span data-stu-id="97a90-141">`ICollector<string>` (to output multiple messages)</span></span>
* <span data-ttu-id="97a90-142">`IAsyncCollector<string>`(asynchronní verzi `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="97a90-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="97a90-143">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="97a90-143">Output sample</span></span>
<span data-ttu-id="97a90-144">Předpokládejme, že máte následující služby Event Hubs výstup vazby v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="97a90-144">Suppose you have the following Event Hubs output binding in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="97a90-145">Naleznete v ukázce konkrétní jazyk, který zapíše se událost do i datový proud.</span><span class="sxs-lookup"><span data-stu-id="97a90-145">See the language-specific sample that writes an event to the even stream.</span></span>

* [<span data-ttu-id="97a90-146">C#</span><span class="sxs-lookup"><span data-stu-id="97a90-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="97a90-147">F#</span><span class="sxs-lookup"><span data-stu-id="97a90-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="97a90-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="97a90-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="97a90-149">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="97a90-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="97a90-150">Nebo, chcete-li vytvořit více zpráv:</span><span class="sxs-lookup"><span data-stu-id="97a90-150">Or, to create multiple messages:</span></span>

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="97a90-151">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="97a90-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="97a90-152">Ukázka výstupu pro Node.js</span><span class="sxs-lookup"><span data-stu-id="97a90-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="97a90-153">Nebo, pokud chcete odeslat více zpráv</span><span class="sxs-lookup"><span data-stu-id="97a90-153">Or, to send multiple messages,</span></span>

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="97a90-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97a90-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
