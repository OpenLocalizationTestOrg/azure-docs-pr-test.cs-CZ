---
title: vazby funkce Event Hubs aaaAzure | Microsoft Docs
description: Pochopit, jak vazeb Azure Event Hubs toouse v Azure Functions.
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
ms.openlocfilehash: e864f032ad5ac58d318c9843c3844b5642733a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="91bac-104">Azure Event Hubs funkce vazby</span><span class="sxs-lookup"><span data-stu-id="91bac-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="91bac-105">Tento článek vysvětluje, jak tooconfigure a používat [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) vazby pro Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="91bac-105">This article explains how tooconfigure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="91bac-106">Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="91bac-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="91bac-107">Pokud jste nový tooAzure Event Hubs, najdete v části hello [Přehled služby Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="91bac-107">If you are new tooAzure Event Hubs, see hello [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="91bac-108">Aktivační událost rozbočovače</span><span class="sxs-lookup"><span data-stu-id="91bac-108">Event hub trigger</span></span>
<span data-ttu-id="91bac-109">Použití centra událostí hello aktivovat toorespond tooan událostí odeslaných tooan datového proudu událostí centra událostí.</span><span class="sxs-lookup"><span data-stu-id="91bac-109">Use hello Event Hubs trigger toorespond tooan event sent tooan event hub event stream.</span></span> <span data-ttu-id="91bac-110">Musíte mít přístup pro čtení toohello události rozbočovače tooset až hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="91bac-110">You must have read access toohello event hub tooset up hello trigger.</span></span>

<span data-ttu-id="91bac-111">Aktivace funkce služby Event Hubs Hello používá následující objekt JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91bac-111">hello Event Hubs function trigger uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of hello event hub>",
    "consumerGroup": "Consumer group toouse - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

<span data-ttu-id="91bac-112">`consumerGroup`je hello tooset volitelná vlastnost použít [skupiny příjemců](../event-hubs/event-hubs-features.md#event-consumers) používá toosubscribe tooevents v centru hello.</span><span class="sxs-lookup"><span data-stu-id="91bac-112">`consumerGroup` is an optional property used tooset hello [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used toosubscribe tooevents in hello hub.</span></span> <span data-ttu-id="91bac-113">Pokud tento parametr vynechán, hello `$Default` skupina uživatelů slouží.</span><span class="sxs-lookup"><span data-stu-id="91bac-113">If omitted, hello `$Default` consumer group is used.</span></span>  
<span data-ttu-id="91bac-114">`connection`musí být hello název nastavení aplikace obsahující hello připojovací řetězec toohello Centrum událostí je obor názvů.</span><span class="sxs-lookup"><span data-stu-id="91bac-114">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="91bac-115">Zkopírujte tento připojovací řetězec kliknutím hello **informace o připojení** tlačítko hello *obor názvů*, není centra událostí hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="91bac-115">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="91bac-116">Tento připojovací řetězec musí mít alespoň čtení oprávnění tooactivate hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="91bac-116">This connection string must have at least read permissions tooactivate hello trigger.</span></span>

<span data-ttu-id="91bac-117">[Další nastavení](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) může být zadaný v host.json souboru toofurther jemné vyladění aktivační události Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="91bac-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file toofurther fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="91bac-118">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="91bac-118">Trigger usage</span></span>
<span data-ttu-id="91bac-119">Při aktivaci Event Hubs aktivační funkce uvítací zprávu, která ji spouští je předán do funkce hello jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="91bac-119">When an Event Hubs trigger function is triggered, hello message that triggers it is passed into hello function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="91bac-120">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="91bac-120">Trigger sample</span></span>
<span data-ttu-id="91bac-121">Předpokládejme, že máte následující Event Hubs aktivovat v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91bac-121">Suppose you have hello following Event Hubs trigger in hello `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="91bac-122">V tématu vzorku hello konkrétní jazyk, který je protokoly tělo zprávy hello hello event hub aktivační události.</span><span class="sxs-lookup"><span data-stu-id="91bac-122">See hello language-specific sample that logs hello message body of hello event hub trigger.</span></span>

* [<span data-ttu-id="91bac-123">C#</span><span class="sxs-lookup"><span data-stu-id="91bac-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="91bac-124">F#</span><span class="sxs-lookup"><span data-stu-id="91bac-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="91bac-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="91bac-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="91bac-126">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="91bac-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="91bac-127">Můžete také získat hello událostí jako [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objekt, který umožňuje získat přístup k metadatům toohello událostí.</span><span class="sxs-lookup"><span data-stu-id="91bac-127">You can also receive hello event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access toohello event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="91bac-128">události tooreceive v dávce, změnit podpis metody hello příliš`string[]` nebo `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="91bac-128">tooreceive events in a batch, change hello method signature too`string[]` or `EventData[]`.</span></span>

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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="91bac-129">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="91bac-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="91bac-130">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="91bac-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="91bac-131">Služba Event Hubs výstup vazby</span><span class="sxs-lookup"><span data-stu-id="91bac-131">Event Hubs output binding</span></span>
<span data-ttu-id="91bac-132">Použití hello Event Hubs Výstupní vazba toowrite události tooan události rozbočovače datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="91bac-132">Use hello Event Hubs output binding toowrite events tooan event hub event stream.</span></span> <span data-ttu-id="91bac-133">Musíte mít oprávnění odesílání tooan události rozbočovače toowrite události tooit.</span><span class="sxs-lookup"><span data-stu-id="91bac-133">You must have send permission tooan event hub toowrite events tooit.</span></span>

<span data-ttu-id="91bac-134">Hello výstup vazba používá následující objekt JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91bac-134">hello output binding uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="91bac-135">`connection`musí být hello název nastavení aplikace obsahující hello připojovací řetězec toohello Centrum událostí je obor názvů.</span><span class="sxs-lookup"><span data-stu-id="91bac-135">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="91bac-136">Zkopírujte tento připojovací řetězec kliknutím hello **informace o připojení** tlačítko hello *obor názvů*, není centra událostí hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="91bac-136">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="91bac-137">Tento připojovací řetězec musí mít odesílání oprávnění toosend hello toohello události datového proudu zpráv.</span><span class="sxs-lookup"><span data-stu-id="91bac-137">This connection string must have send permissions toosend hello message toohello event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="91bac-138">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="91bac-138">Output usage</span></span>
<span data-ttu-id="91bac-139">Tato část uvádí, jak toouse Event Hubs výstup vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="91bac-139">This section shows you how toouse your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="91bac-140">Výstup centra událostí toohello nakonfigurované zprávy můžete s hello následující typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="91bac-140">You can output messages toohello configured event hub with hello following parameter types:</span></span>

* `out string`
* <span data-ttu-id="91bac-141">`ICollector<string>`(toooutput více zpráv)</span><span class="sxs-lookup"><span data-stu-id="91bac-141">`ICollector<string>` (toooutput multiple messages)</span></span>
* <span data-ttu-id="91bac-142">`IAsyncCollector<string>`(asynchronní verzi `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="91bac-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="91bac-143">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="91bac-143">Output sample</span></span>
<span data-ttu-id="91bac-144">Předpokládejme, že máte následující hello Event Hubs výstup vazby v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91bac-144">Suppose you have hello following Event Hubs output binding in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="91bac-145">V tématu vzorku hello konkrétní jazyk, který zapíše toohello i datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="91bac-145">See hello language-specific sample that writes an event toohello even stream.</span></span>

* [<span data-ttu-id="91bac-146">C#</span><span class="sxs-lookup"><span data-stu-id="91bac-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="91bac-147">F#</span><span class="sxs-lookup"><span data-stu-id="91bac-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="91bac-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="91bac-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="91bac-149">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="91bac-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="91bac-150">Nebo toocreate více zpráv:</span><span class="sxs-lookup"><span data-stu-id="91bac-150">Or, toocreate multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="91bac-151">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="91bac-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="91bac-152">Ukázka výstupu pro Node.js</span><span class="sxs-lookup"><span data-stu-id="91bac-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="91bac-153">Nebo toosend více zpráv</span><span class="sxs-lookup"><span data-stu-id="91bac-153">Or, toosend multiple messages,</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="91bac-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91bac-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
