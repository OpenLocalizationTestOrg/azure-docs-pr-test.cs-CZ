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
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="cef1e-104">Azure Service Bus funkce vazby</span><span class="sxs-lookup"><span data-stu-id="cef1e-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="cef1e-105">Tento článek vysvětluje, jak tooconfigure a práce s vazeb Azure Service Bus v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="cef1e-105">This article explains how tooconfigure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="cef1e-106">Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.</span><span class="sxs-lookup"><span data-stu-id="cef1e-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="cef1e-107">Aktivace služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="cef1e-107">Service Bus trigger</span></span>
<span data-ttu-id="cef1e-108">Použijte hello Service Bus aktivační událost toorespond toomessages z fronty sběrnice nebo téma.</span><span class="sxs-lookup"><span data-stu-id="cef1e-108">Use hello Service Bus trigger toorespond toomessages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="cef1e-109">Hello Service Bus frontu a téma aktivační události jsou definovány hello následující objekty JSON v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="cef1e-109">hello Service Bus queue and topic triggers are defined by hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="cef1e-110">*fronty* aktivační události:</span><span class="sxs-lookup"><span data-stu-id="cef1e-110">*queue* trigger:</span></span>

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

* <span data-ttu-id="cef1e-111">*téma* aktivační události:</span><span class="sxs-lookup"><span data-stu-id="cef1e-111">*topic* trigger:</span></span>

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

<span data-ttu-id="cef1e-112">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="cef1e-112">Note hello following:</span></span>

* <span data-ttu-id="cef1e-113">Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující hello připojovací řetězec tooyour oboru názvů Service Bus, potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="cef1e-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your trigger.</span></span> <span data-ttu-id="cef1e-114">Získat hello připojovací řetězec pomocí následujících kroků hello zobrazuje se v [získání přihlašovacích údajů pro správu hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="cef1e-114">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="cef1e-115">Hello připojovací řetězec musí být pro obor názvů, mimo jiné tooa konkrétní fronty nebo téma sběrnice.</span><span class="sxs-lookup"><span data-stu-id="cef1e-115">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="cef1e-116">Pokud necháte `connection` prázdná, aktivační události hello předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-116">If you leave `connection` empty, hello trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="cef1e-117">Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="cef1e-118">Výchozí hodnota Hello je `manage`, což znamená, že hello `connection` má hello **spravovat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cef1e-118">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="cef1e-119">Pokud použijete připojovací řetězec, který nemá hello **spravovat** nastavit oprávnění, `accessRights` příliš`listen`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-119">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="cef1e-120">Funkce hello runtime může selhat snažíme toodo činnosti, které vyžadují, jinak hodnota spravovat práva.</span><span class="sxs-lookup"><span data-stu-id="cef1e-120">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="cef1e-121">Aktivační událost chování</span><span class="sxs-lookup"><span data-stu-id="cef1e-121">Trigger behavior</span></span>
* <span data-ttu-id="cef1e-122">**Dělení na vlákna jedním** – ve výchozím nastavení hello funkce runtime procesy více zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="cef1e-122">**Single-threading** - By default, hello Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="cef1e-123">nastavit toodirect hello runtime tooprocess pouze do jedné fronta nebo téma zprávy najednou, `serviceBus.maxConcurrentCalls` too1 v *host.json*.</span><span class="sxs-lookup"><span data-stu-id="cef1e-123">toodirect hello runtime tooprocess only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` too1 in *host.json*.</span></span> 
  <span data-ttu-id="cef1e-124">Informace o *host.json*, najdete v části [struktura složek](functions-reference.md#folder-structure) a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="cef1e-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="cef1e-125">**Zpracování škodlivých zpráv** -Service Bus nepodporuje svůj vlastní zpracování poškozených zpráv, které nelze řídí nebo nakonfigurované v Azure Functions konfigurace nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="cef1e-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="cef1e-126">**Chování PeekLock** -hello Functions runtime přijme nějakou zprávu v [ `PeekLock` režimu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) a volání `Complete` na uvítací zprávu, pokud funkce hello skončí úspěšně, nebo volání `Abandon` Pokud hello funkce se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="cef1e-126">**PeekLock behavior** - hello Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> 
  <span data-ttu-id="cef1e-127">Pokud poběží déle než hello hello funkce `PeekLock` automaticky obnovují vždy vypršel časový limit, hello zámku.</span><span class="sxs-lookup"><span data-stu-id="cef1e-127">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="cef1e-128">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="cef1e-128">Trigger usage</span></span>
<span data-ttu-id="cef1e-129">Tato část uvádí, jak toouse Service Bus spustit ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="cef1e-129">This section shows you how toouse your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="cef1e-130">V C# a F # může být uvítací zprávu aktivační události služby Service Bus deserializovat tooany hello následující vstupní typy:</span><span class="sxs-lookup"><span data-stu-id="cef1e-130">In C# and F#, hello Service Bus trigger message can be deserialized tooany of hello following input types:</span></span>

* <span data-ttu-id="cef1e-131">`string`-užitečné pro řetězec zprávy</span><span class="sxs-lookup"><span data-stu-id="cef1e-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="cef1e-132">`byte[]`-vhodné pro binární data</span><span class="sxs-lookup"><span data-stu-id="cef1e-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="cef1e-133">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data serializací JSON.</span><span class="sxs-lookup"><span data-stu-id="cef1e-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="cef1e-134">Pokud je deklarovat vlastní typ vstupu, například `CustomType`, Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="cef1e-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="cef1e-135">`BrokeredMessage`-poskytuje můžete hello deserializovat zprávu s hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="cef1e-135">`BrokeredMessage` - gives you hello deserialized message with hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="cef1e-136">V Node.js uvítací zprávu aktivační události služby Service Bus je předán do funkce hello jako řetězec nebo objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="cef1e-136">In Node.js, hello Service Bus trigger message is passed into hello function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="cef1e-137">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="cef1e-137">Trigger sample</span></span>
<span data-ttu-id="cef1e-138">Předpokládejme, že máte následující function.json hello:</span><span class="sxs-lookup"><span data-stu-id="cef1e-138">Suppose you have hello following function.json:</span></span>

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

<span data-ttu-id="cef1e-139">V tématu vzorku hello konkrétní jazyk, který zpracuje zprávu fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="cef1e-139">See hello language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="cef1e-140">C#</span><span class="sxs-lookup"><span data-stu-id="cef1e-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="cef1e-141">F#</span><span class="sxs-lookup"><span data-stu-id="cef1e-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="cef1e-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="cef1e-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="cef1e-143">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="cef1e-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="cef1e-144">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="cef1e-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="cef1e-145">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="cef1e-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="cef1e-146">Service Bus výstup vazby</span><span class="sxs-lookup"><span data-stu-id="cef1e-146">Service Bus output binding</span></span>
<span data-ttu-id="cef1e-147">Hello výstup frontu a téma sběrnice pro funkci použít následující objekty JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="cef1e-147">hello Service Bus queue and topic output for a function use hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="cef1e-148">*fronty* výstup:</span><span class="sxs-lookup"><span data-stu-id="cef1e-148">*queue* output:</span></span>

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
* <span data-ttu-id="cef1e-149">*téma* výstup:</span><span class="sxs-lookup"><span data-stu-id="cef1e-149">*topic* output:</span></span>

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

<span data-ttu-id="cef1e-150">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="cef1e-150">Note hello following:</span></span>

* <span data-ttu-id="cef1e-151">Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující hello připojovací řetězec tooyour oboru názvů Service Bus, potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost ve výstupu vazba.</span><span class="sxs-lookup"><span data-stu-id="cef1e-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your output binding.</span></span> <span data-ttu-id="cef1e-152">Získat hello připojovací řetězec pomocí následujících kroků hello zobrazuje se v [získání přihlašovacích údajů pro správu hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="cef1e-152">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="cef1e-153">Hello připojovací řetězec musí být pro obor názvů, mimo jiné tooa konkrétní fronty nebo téma sběrnice.</span><span class="sxs-lookup"><span data-stu-id="cef1e-153">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="cef1e-154">Pokud necháte `connection` prázdná, hello výstup vazby předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-154">If you leave `connection` empty, hello output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="cef1e-155">Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="cef1e-156">Výchozí hodnota Hello je `manage`, což znamená, že hello `connection` má hello **spravovat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cef1e-156">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="cef1e-157">Pokud použijete připojovací řetězec, který nemá hello **spravovat** nastavit oprávnění, `accessRights` příliš`listen`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-157">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="cef1e-158">Funkce hello runtime může selhat snažíme toodo činnosti, které vyžadují, jinak hodnota spravovat práva.</span><span class="sxs-lookup"><span data-stu-id="cef1e-158">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="cef1e-159">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="cef1e-159">Output usage</span></span>
<span data-ttu-id="cef1e-160">V jazyce C# a F # můžete Azure Functions vytvořit zprávu fronty Service Bus z jakéhokoli z hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="cef1e-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of hello following types:</span></span>

* <span data-ttu-id="cef1e-161">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) -definici parametru vypadá jako `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="cef1e-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="cef1e-162">Funkce deserializuje hello objekt do formátu JSON zprávy.</span><span class="sxs-lookup"><span data-stu-id="cef1e-162">Functions deserializes hello object into a JSON message.</span></span> <span data-ttu-id="cef1e-163">Pokud je hodnota výstup hello při ukončení hello funkce null, funkce vytvoří zprávu hello objekt s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="cef1e-163">If hello output value is null when hello function exits, Functions creates hello message with a null object.</span></span>
* <span data-ttu-id="cef1e-164">`string`-Vypadá parametr definice `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="cef1e-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="cef1e-165">Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="cef1e-165">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="cef1e-166">`byte[]`-Vypadá parametr definice `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="cef1e-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="cef1e-167">Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="cef1e-167">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="cef1e-168">`BrokeredMessage`Definici parametru vypadá jako `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="cef1e-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="cef1e-169">Pokud hodnota parametru hello jinou hodnotu než null při ukončení hello funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="cef1e-169">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>

<span data-ttu-id="cef1e-170">Pro vytvoření více zpráv ve funkci jazyka C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="cef1e-171">Zprávu se vytvoří při volání hello `Add` metoda.</span><span class="sxs-lookup"><span data-stu-id="cef1e-171">A message is created when you call hello `Add` method.</span></span>

<span data-ttu-id="cef1e-172">V Node.js, můžete přiřadit řetězec, bajtové pole nebo objekt jazyka Javascript (deserializovat do formátu JSON) příliš`context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="cef1e-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) too`context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="cef1e-173">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="cef1e-173">Output sample</span></span>
<span data-ttu-id="cef1e-174">Předpokládejme, že máte následující function.json, která definuje výstupní fronty Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="cef1e-174">Suppose you have hello following function.json, that defines a Service Bus queue output:</span></span>

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

<span data-ttu-id="cef1e-175">V tématu vzorku hello konkrétní jazyk, který odešle frontu sběrnice zpráv toohello.</span><span class="sxs-lookup"><span data-stu-id="cef1e-175">See hello language-specific sample that sends a message toohello service bus queue.</span></span>

* [<span data-ttu-id="cef1e-176">C#</span><span class="sxs-lookup"><span data-stu-id="cef1e-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="cef1e-177">F#</span><span class="sxs-lookup"><span data-stu-id="cef1e-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="cef1e-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="cef1e-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="cef1e-179">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="cef1e-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="cef1e-180">Nebo toocreate více zpráv:</span><span class="sxs-lookup"><span data-stu-id="cef1e-180">Or, toocreate multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="cef1e-181">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="cef1e-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="cef1e-182">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="cef1e-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="cef1e-183">Nebo toocreate více zpráv:</span><span class="sxs-lookup"><span data-stu-id="cef1e-183">Or, toocreate multiple messages:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="cef1e-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cef1e-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

