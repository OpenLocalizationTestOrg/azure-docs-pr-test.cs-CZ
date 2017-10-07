---
title: "aaaAzure funkce služby Service Bus triggerů a vazeb | Microsoft Docs"
description: Pochopit, jak se aktivuje toouse Azure Service Bus a vazeb v Azure Functions.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: glenga
ms.openlocfilehash: dff9e89bd3840b8c11f91cae41e13502afc7aa60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure Service Bus funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak tooconfigure a práce s vazeb Azure Service Bus v Azure Functions. 

Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Aktivace služby Service Bus
Použijte hello Service Bus aktivační událost toorespond toomessages z fronty sběrnice nebo téma. 

Hello Service Bus frontu a téma aktivační události jsou definovány hello následující objekty JSON v hello `bindings` pole function.json:

* *fronty* aktivační události:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *téma* aktivační události:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Vezměte na vědomí následující hello:

* Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující hello připojovací řetězec tooyour oboru názvů Service Bus, potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost aktivační událost. Získat hello připojovací řetězec pomocí následujících kroků hello zobrazuje se v [získání přihlašovacích údajů pro správu hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Hello připojovací řetězec musí být pro obor názvů, mimo jiné tooa konkrétní fronty nebo téma sběrnice.
  Pokud necháte `connection` prázdná, aktivační události hello předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.
* Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`. Výchozí hodnota Hello je `manage`, což znamená, že hello `connection` má hello **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá hello **spravovat** nastavit oprávnění, `accessRights` příliš`listen`. Funkce hello runtime může selhat snažíme toodo činnosti, které vyžadují, jinak hodnota spravovat práva.

## <a name="trigger-behavior"></a>Aktivační událost chování
* **Dělení na vlákna jedním** – ve výchozím nastavení hello funkce runtime procesy více zpráv současně. nastavit toodirect hello runtime tooprocess pouze do jedné fronta nebo téma zprávy najednou, `serviceBus.maxConcurrentCalls` too1 v *host.json*. 
  Informace o *host.json*, najdete v části [struktura složek](functions-reference.md#folder-structure) a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Zpracování škodlivých zpráv** -Service Bus nepodporuje svůj vlastní zpracování poškozených zpráv, které nelze řídí nebo nakonfigurované v Azure Functions konfigurace nebo kódu. 
* **Chování PeekLock** -hello Functions runtime přijme nějakou zprávu v [ `PeekLock` režimu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) a volání `Complete` na uvítací zprávu, pokud funkce hello skončí úspěšně, nebo volání `Abandon` Pokud hello funkce se nezdaří. 
  Pokud poběží déle než hello hello funkce `PeekLock` automaticky obnovují vždy vypršel časový limit, hello zámku.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
Tato část uvádí, jak toouse Service Bus spustit ve vašem kódu funkce. 

V C# a F # může být uvítací zprávu aktivační události služby Service Bus deserializovat tooany hello následující vstupní typy:

* `string`-užitečné pro řetězec zprávy
* `byte[]`-vhodné pro binární data
* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data serializací JSON.
  Pokud je deklarovat vlastní typ vstupu, například `CustomType`, Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.
* `BrokeredMessage`-poskytuje můžete hello deserializovat zprávu s hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metoda.

V Node.js uvítací zprávu aktivační události služby Service Bus je předán do funkce hello jako řetězec nebo objekt JSON.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující function.json hello:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který zpracuje zprávu fronty Service Bus.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Service Bus výstup vazby
Hello výstup frontu a téma sběrnice pro funkci použít následující objekty JSON v hello hello `bindings` pole function.json:

* *fronty* výstup:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *téma* výstup:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Vezměte na vědomí následující hello:

* Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující hello připojovací řetězec tooyour oboru názvů Service Bus, potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost ve výstupu vazba. Získat hello připojovací řetězec pomocí následujících kroků hello zobrazuje se v [získání přihlašovacích údajů pro správu hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Hello připojovací řetězec musí být pro obor názvů, mimo jiné tooa konkrétní fronty nebo téma sběrnice.
  Pokud necháte `connection` prázdná, hello výstup vazby předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.
* Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`. Výchozí hodnota Hello je `manage`, což znamená, že hello `connection` má hello **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá hello **spravovat** nastavit oprávnění, `accessRights` příliš`listen`. Funkce hello runtime může selhat snažíme toodo činnosti, které vyžadují, jinak hodnota spravovat práva.

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V jazyce C# a F # můžete Azure Functions vytvořit zprávu fronty Service Bus z jakéhokoli z hello následující typy:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) -definici parametru vypadá jako `out T paramName` (C#).
  Funkce deserializuje hello objekt do formátu JSON zprávy. Pokud je hodnota výstup hello při ukončení hello funkce null, funkce vytvoří zprávu hello objekt s hodnotou null.
* `string`-Vypadá parametr definice `out string paraName` (C#). Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.
* `byte[]`-Vypadá parametr definice `out byte[] paraName` (C#). Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.
* `BrokeredMessage`Definici parametru vypadá jako `out BrokeredMessage paraName` (C#). Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.

Pro vytvoření více zpráv ve funkci jazyka C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`. Zprávu se vytvoří při volání hello `Add` metoda.

V Node.js, můžete přiřadit řetězec, bajtové pole nebo objekt jazyka Javascript (deserializovat do formátu JSON) příliš`context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující function.json, která definuje výstupní fronty Service Bus hello:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který odešle frontu sběrnice zpráv toohello.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Nebo toocreate více zpráv:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Nebo toocreate více zpráv:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

