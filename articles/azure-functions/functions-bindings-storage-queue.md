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
# <a name="azure-functions-queue-storage-bindings"></a>Azure Queue Storage funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek popisuje, jak tooconfigure a kód Azure Queue storage vazeb v Azure Functions. Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty. Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Aktivace fronty úložiště
Hello Azure Queue storage aktivační událost umožňuje toomonitor fronty úložiště pro nové zprávy a toothem reagovat. 

Zadejte aktivační události fronty pomocí hello **integrací** kartě hello funkce portálu. Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště. V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

Další nastavení lze zadat do [host.json soubor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther doladit aktivační procedury fronty úložiště. Například můžete změnit interval dotazování hello fronty v host.json.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Pomocí aktivační procedury fronty
Ve funkcích Node.js, přístup k hello fronty dat pomocí `context.bindings.<name>`.


V rozhraní .NET funkce, přístup k datové části hello fronty pomocí parametru metody, jako třeba `CloudQueueMessage paramName`. Zde `paramName` je hodnota hello jste zadali v hello [konfigurace aktivační události](#trigger). uvítací zprávu fronty lze deserializovat tooany hello následující typy:

* Objektů POCO objekt. Použijte, pokud datová část fronty hello je objekt JSON. Hello Functions runtime deserializuje datovou část hello do objektu objektů POCO hello. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Metadata frontě aktivační události
Aktivace fronty Hello nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu. Hello hodnoty mají hello stejnou sémantiku jako [ `CloudQueueMessage` ].

* **QueueTrigger** -datové frontu (pokud platný řetězec)
* **DequeueCount** – typ `int`. počet pokusů, které se tato zpráva byla vyjmutou. Hello.
* **ExpirationTime** – typ `DateTimeOffset?`. Hello čas vypršení platnosti zprávy hello.
* **ID** – typ `string`. ID zprávy fronty
* **InsertionTime** – typ `DateTimeOffset?`. čas Hello hello zpráva byla přidána toohello fronty.
* **NextVisibleTime** – typ `DateTimeOffset?`. čas Hello hello zprávy budou vedle viditelné.
* **Vlastnosti PopReceipt** – typ `string`. uvítací zprávu pop přijetí.

V tématu Jak toouse hello metadata fronty v [aktivační událost vzorku](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte hello následující function.json, který definuje aktivační procedury fronty:

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

V tématu vzorku hello konkrétní jazyk, který načte a protokoly metadata fronty.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #
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

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

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

### <a name="handling-poison-queue-messages"></a>Zpracování poškozených fronty zpráv
Pokud se nezdaří aktivační funkce fronty, Azure Functions opakuje této funkce si toofive, které časy dané frontě zprávy, včetně hello nejprve provést. Pokud selžou všechny pět pokusy, hello functions runtime přidá zprávu tooa fronty úložiště s názvem  *&lt;originalqueuename >-poškozených*. Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost. 

poškozených zpráv toohandle ručně, zkontrolujte hello `dequeueCount` hello zprávy fronty (v tématu [frontě aktivační události metadata](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Vazba výstupní fronty úložiště
Vazba vám umožní toowrite zprávy tooa fronty výstupu Hello fronty Azure storage. 

Definování fronty výstupu vazby pomocí hello **integrací** kartě hello funkce portálu. Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště. V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Pomocí fronty výstup vazby
Ve funkcích Node.js přistupujete hello výstupní fronty pomocí `context.bindings.<name>`.

V rozhraní .NET funkce můžete výstup tooany hello následující typy. Pokud je parametr typu `T`, `T` musí být jeden z typů výstup hello podporována, jako například `string` nebo objektů POCO.

* `out T`(serializovanou jako JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Návratový typ metody hello můžete také použít jako hello výstup vazby.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Ukázka výstupní fronty
Následující Hello *function.json* definuje aktivační procedury HTTP s frontou výstup vazby:

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

V tématu vzorku hello konkrétní jazyk, který výstupy zprávu fronty s hello příchozí datové části protokolu HTTP.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Ukázka výstupní fronty v jazyce C# #

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

toosend více zpráv používat `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Ukázka výstupní fronty v Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Nebo toosend více zpráv

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

Příklad, funkci, která používá fronty úložiště triggerů a vazeb, naleznete v části [vytvořit tooan funkce Azure připojení služby Azure](functions-create-an-azure-connected-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage.]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
