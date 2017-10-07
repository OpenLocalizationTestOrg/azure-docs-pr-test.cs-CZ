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
# <a name="azure-functions-event-hubs-bindings"></a>Azure Event Hubs funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak tooconfigure a používat [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) vazby pro Azure Functions.
Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Pokud jste nový tooAzure Event Hubs, najdete v části hello [Přehled služby Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Aktivační událost rozbočovače
Použití centra událostí hello aktivovat toorespond tooan událostí odeslaných tooan datového proudu událostí centra událostí. Musíte mít přístup pro čtení toohello události rozbočovače tooset až hello aktivační události.

Aktivace funkce služby Event Hubs Hello používá následující objekt JSON v hello hello `bindings` pole function.json:

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

`consumerGroup`je hello tooset volitelná vlastnost použít [skupiny příjemců](../event-hubs/event-hubs-features.md#event-consumers) používá toosubscribe tooevents v centru hello. Pokud tento parametr vynechán, hello `$Default` skupina uživatelů slouží.  
`connection`musí být hello název nastavení aplikace obsahující hello připojovací řetězec toohello Centrum událostí je obor názvů.
Zkopírujte tento připojovací řetězec kliknutím hello **informace o připojení** tlačítko hello *obor názvů*, není centra událostí hello, sám sebe. Tento připojovací řetězec musí mít alespoň čtení oprávnění tooactivate hello aktivační události.

[Další nastavení](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) může být zadaný v host.json souboru toofurther jemné vyladění aktivační události Event Hubs.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
Při aktivaci Event Hubs aktivační funkce uvítací zprávu, která ji spouští je předán do funkce hello jako řetězec.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující Event Hubs aktivovat v hello hello `bindings` pole function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

V tématu vzorku hello konkrétní jazyk, který je protokoly tělo zprávy hello hello event hub aktivační události.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Můžete také získat hello událostí jako [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objekt, který umožňuje získat přístup k metadatům toohello událostí.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

události tooreceive v dávce, změnit podpis metody hello příliš`string[]` nebo `EventData[]`.

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

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Služba Event Hubs výstup vazby
Použití hello Event Hubs Výstupní vazba toowrite události tooan události rozbočovače datového proudu událostí. Musíte mít oprávnění odesílání tooan události rozbočovače toowrite události tooit.

Hello výstup vazba používá následující objekt JSON v hello hello `bindings` pole function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`musí být hello název nastavení aplikace obsahující hello připojovací řetězec toohello Centrum událostí je obor názvů.
Zkopírujte tento připojovací řetězec kliknutím hello **informace o připojení** tlačítko hello *obor názvů*, není centra událostí hello, sám sebe. Tento připojovací řetězec musí mít odesílání oprávnění toosend hello toohello události datového proudu zpráv.

## <a name="output-usage"></a>Využití výstupní
Tato část uvádí, jak toouse Event Hubs výstup vazby v kódu funkce.

Výstup centra událostí toohello nakonfigurované zprávy můžete s hello následující typy parametrů:

* `out string`
* `ICollector<string>`(toooutput více zpráv)
* `IAsyncCollector<string>`(asynchronní verzi `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující hello Event Hubs výstup vazby v hello `bindings` pole function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

V tématu vzorku hello konkrétní jazyk, který zapíše toohello i datového proudu událostí.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Nebo toocreate více zpráv:

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

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Ukázka výstupu pro Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Nebo toosend více zpráv

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

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
