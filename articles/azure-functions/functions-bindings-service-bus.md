---
title: "Azure Service Bus funkce triggerů a vazeb | Microsoft Docs"
description: "Pochopit, jak používat Azure Service Bus triggerů a vazeb v Azure Functions."
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
ms.openlocfilehash: b3ee306cd37ebf88dc9369ccc2dc6c670557fd5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="c4723-104">Azure Service Bus funkce vazby</span><span class="sxs-lookup"><span data-stu-id="c4723-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="c4723-105">Tento článek vysvětluje postup konfigurace a práce s vazeb Azure Service Bus v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c4723-105">This article explains how to configure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="c4723-106">Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.</span><span class="sxs-lookup"><span data-stu-id="c4723-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="c4723-107">Aktivace služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="c4723-107">Service Bus trigger</span></span>
<span data-ttu-id="c4723-108">Použijte aktivační událost Service Bus reagovat na zprávy z fronty sběrnice nebo téma.</span><span class="sxs-lookup"><span data-stu-id="c4723-108">Use the Service Bus trigger to respond to messages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="c4723-109">Následující objekty JSON v jsou definovány aktivační události pro frontu a téma sběrnice `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="c4723-109">The Service Bus queue and topic triggers are defined by the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="c4723-110">*fronty* aktivační události:</span><span class="sxs-lookup"><span data-stu-id="c4723-110">*queue* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* <span data-ttu-id="c4723-111">*téma* aktivační události:</span><span class="sxs-lookup"><span data-stu-id="c4723-111">*topic* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

<span data-ttu-id="c4723-112">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="c4723-112">Note the following:</span></span>

* <span data-ttu-id="c4723-113">Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující připojovací řetězec k oboru názvů Service Bus, pak zadejte název nastavení aplikace v `connection` vlastnost aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="c4723-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your trigger.</span></span> <span data-ttu-id="c4723-114">Získat připojovací řetězec pomocí následujících kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="c4723-114">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="c4723-115">Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma.</span><span class="sxs-lookup"><span data-stu-id="c4723-115">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="c4723-116">Pokud necháte `connection` prázdná, aktivační událost se předpokládá, že je zadán připojovací řetězec sběrnice výchozí v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="c4723-116">If you leave `connection` empty, the trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="c4723-117">Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`.</span><span class="sxs-lookup"><span data-stu-id="c4723-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="c4723-118">Výchozí hodnota je `manage`, což znamená, že `connection` má **spravovat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c4723-118">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="c4723-119">Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` k `listen`.</span><span class="sxs-lookup"><span data-stu-id="c4723-119">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="c4723-120">Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.</span><span class="sxs-lookup"><span data-stu-id="c4723-120">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="c4723-121">Aktivační událost chování</span><span class="sxs-lookup"><span data-stu-id="c4723-121">Trigger behavior</span></span>
* <span data-ttu-id="c4723-122">**Dělení na vlákna jedním** – ve výchozím nastavení funkce procesy runtime více zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="c4723-122">**Single-threading** - By default, the Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="c4723-123">Přímé modulu runtime zpracovat jenom jeden fronta nebo téma zprávy najednou, nastavte `serviceBus.maxConcurrentCalls` 1 v *host.json*.</span><span class="sxs-lookup"><span data-stu-id="c4723-123">To direct the runtime to process only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` to 1 in *host.json*.</span></span> 
  <span data-ttu-id="c4723-124">Informace o *host.json*, najdete v části [struktura složek](functions-reference.md#folder-structure) a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="c4723-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="c4723-125">**Zpracování škodlivých zpráv** -Service Bus nepodporuje svůj vlastní zpracování poškozených zpráv, které nelze řídí nebo nakonfigurované v Azure Functions konfigurace nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="c4723-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="c4723-126">**Chování PeekLock** – modul runtime Functions přijme nějakou zprávu v [ `PeekLock` režimu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) a volání `Complete` na zprávu, pokud funkci skončí úspěšně, nebo volání `Abandon` Pokud funkce se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c4723-126">**PeekLock behavior** - The Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> 
  <span data-ttu-id="c4723-127">Pokud je funkce spuštěná déle, než `PeekLock` automaticky obnovují vždy vypršel časový limit, zámek.</span><span class="sxs-lookup"><span data-stu-id="c4723-127">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="c4723-128">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="c4723-128">Trigger usage</span></span>
<span data-ttu-id="c4723-129">V této části se dozvíte, jak používat vaše aktivační události služby Service Bus ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="c4723-129">This section shows you how to use your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="c4723-130">V jazyce C# a F # lze deserializovat zpráva aktivační události služby Service Bus ke kterékoli z následujících typů vstupu:</span><span class="sxs-lookup"><span data-stu-id="c4723-130">In C# and F#, the Service Bus trigger message can be deserialized to any of the following input types:</span></span>

* <span data-ttu-id="c4723-131">`string`-užitečné pro řetězec zprávy</span><span class="sxs-lookup"><span data-stu-id="c4723-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="c4723-132">`byte[]`-vhodné pro binární data</span><span class="sxs-lookup"><span data-stu-id="c4723-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="c4723-133">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data serializací JSON.</span><span class="sxs-lookup"><span data-stu-id="c4723-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="c4723-134">Pokud je deklarovat vlastní typ vstupu, například `CustomType`, Azure Functions se pokusí rekonstruovat JSON data do zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="c4723-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries to deserialize the JSON data into your specified type.</span></span>
* <span data-ttu-id="c4723-135">`BrokeredMessage`-vám deserializovat zprávu s [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="c4723-135">`BrokeredMessage` - gives you the deserialized message with the [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="c4723-136">V Node.js aktivační událost zprávy služby Service Bus je předán do funkce jako řetězec nebo objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="c4723-136">In Node.js, the Service Bus trigger message is passed into the function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="c4723-137">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="c4723-137">Trigger sample</span></span>
<span data-ttu-id="c4723-138">Předpokládejme, že máte následující function.json:</span><span class="sxs-lookup"><span data-stu-id="c4723-138">Suppose you have the following function.json:</span></span>

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

<span data-ttu-id="c4723-139">Naleznete v ukázce pro specifický jazyk, který zpracuje zprávu fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c4723-139">See the language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="c4723-140">C#</span><span class="sxs-lookup"><span data-stu-id="c4723-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="c4723-141">F#</span><span class="sxs-lookup"><span data-stu-id="c4723-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="c4723-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="c4723-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="c4723-143">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="c4723-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="c4723-144">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="c4723-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="c4723-145">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="c4723-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="c4723-146">Service Bus výstup vazby</span><span class="sxs-lookup"><span data-stu-id="c4723-146">Service Bus output binding</span></span>
<span data-ttu-id="c4723-147">Výstup frontu a téma sběrnice pro funkci použití následujících objektů JSON `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="c4723-147">The Service Bus queue and topic output for a function use the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="c4723-148">*fronty* výstup:</span><span class="sxs-lookup"><span data-stu-id="c4723-148">*queue* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* <span data-ttu-id="c4723-149">*téma* výstup:</span><span class="sxs-lookup"><span data-stu-id="c4723-149">*topic* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

<span data-ttu-id="c4723-150">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="c4723-150">Note the following:</span></span>

* <span data-ttu-id="c4723-151">Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující připojovací řetězec k oboru názvů Service Bus, pak zadejte název nastavení aplikace v `connection` vlastnost Výstupní vazba.</span><span class="sxs-lookup"><span data-stu-id="c4723-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your output binding.</span></span> <span data-ttu-id="c4723-152">Získat připojovací řetězec pomocí následujících kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="c4723-152">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="c4723-153">Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma.</span><span class="sxs-lookup"><span data-stu-id="c4723-153">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="c4723-154">Pokud necháte `connection` prázdná, vazba výstup předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="c4723-154">If you leave `connection` empty, the output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="c4723-155">Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`.</span><span class="sxs-lookup"><span data-stu-id="c4723-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="c4723-156">Výchozí hodnota je `manage`, což znamená, že `connection` má **spravovat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c4723-156">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="c4723-157">Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` k `listen`.</span><span class="sxs-lookup"><span data-stu-id="c4723-157">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="c4723-158">Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.</span><span class="sxs-lookup"><span data-stu-id="c4723-158">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="c4723-159">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="c4723-159">Output usage</span></span>
<span data-ttu-id="c4723-160">V jazyce C# a F # můžete Azure Functions vytvořit zprávu fronty Service Bus z libovolného z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="c4723-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of the following types:</span></span>

* <span data-ttu-id="c4723-161">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) -definici parametru vypadá jako `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="c4723-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="c4723-162">Funkce deserializuje objekt do formátu JSON zprávy.</span><span class="sxs-lookup"><span data-stu-id="c4723-162">Functions deserializes the object into a JSON message.</span></span> <span data-ttu-id="c4723-163">Pokud jsou výstupní hodnotu null při ukončení funkce, funkce vytvoří zprávu s objektem hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c4723-163">If the output value is null when the function exits, Functions creates the message with a null object.</span></span>
* <span data-ttu-id="c4723-164">`string`-Vypadá parametr definice `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="c4723-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="c4723-165">Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="c4723-165">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="c4723-166">`byte[]`-Vypadá parametr definice `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="c4723-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="c4723-167">Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="c4723-167">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="c4723-168">`BrokeredMessage`Definici parametru vypadá jako `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="c4723-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="c4723-169">Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="c4723-169">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>

<span data-ttu-id="c4723-170">Pro vytvoření více zpráv ve funkci jazyka C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="c4723-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="c4723-171">Zprávu se vytvoří při volání `Add` metoda.</span><span class="sxs-lookup"><span data-stu-id="c4723-171">A message is created when you call the `Add` method.</span></span>

<span data-ttu-id="c4723-172">V Node.js, můžete přiřadit řetězec, bajtové pole nebo objekt jazyka Javascript (deserializovat do formátu JSON) na `context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="c4723-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) to `context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="c4723-173">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="c4723-173">Output sample</span></span>
<span data-ttu-id="c4723-174">Předpokládejme, že máte následující function.json, která definuje výstupní fronty Service Bus:</span><span class="sxs-lookup"><span data-stu-id="c4723-174">Suppose you have the following function.json, that defines a Service Bus queue output:</span></span>

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

<span data-ttu-id="c4723-175">Naleznete v ukázce pro specifický jazyk, který odešle zprávu do fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="c4723-175">See the language-specific sample that sends a message to the service bus queue.</span></span>

* [<span data-ttu-id="c4723-176">C#</span><span class="sxs-lookup"><span data-stu-id="c4723-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="c4723-177">F#</span><span class="sxs-lookup"><span data-stu-id="c4723-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="c4723-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="c4723-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="c4723-179">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="c4723-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="c4723-180">Nebo, chcete-li vytvořit více zpráv:</span><span class="sxs-lookup"><span data-stu-id="c4723-180">Or, to create multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="c4723-181">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="c4723-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="c4723-182">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="c4723-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="c4723-183">Nebo, chcete-li vytvořit více zpráv:</span><span class="sxs-lookup"><span data-stu-id="c4723-183">Or, to create multiple messages:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c4723-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4723-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

