---
title: vazby funkce Mobile Apps aaaAzure | Microsoft Docs
description: Pochopit, jak toouse vazeb Azure Mobile Apps v Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d3679a5d5c66705b32e422ec17e3a1e6d6ac063c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="8fe0a-104">Azure Mobile Apps funkce vazby</span><span class="sxs-lookup"><span data-stu-id="8fe0a-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8fe0a-105">Tento článek vysvětluje, jak tooconfigure a kód [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-105">This article explains how tooconfigure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="8fe0a-106">Azure Functions podporuje vstup a výstup vazby pro Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="8fe0a-107">vstupní technologie Hello Mobile Apps a výstup vazby umožňují [číst a zapisovat toodata tabulky](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) v mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-107">hello Mobile Apps input and output bindings let you [read from and write toodata tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="8fe0a-108">Vstupní vazba Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="8fe0a-108">Mobile Apps input binding</span></span>
<span data-ttu-id="8fe0a-109">Hello Vstupní vazba Mobile Apps načte záznam z koncového bodu mobilní tabulky a předá ji do funkce.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-109">hello Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="8fe0a-110">V jazyce C# a F # funkce všechny změny provedené toohello záznam automaticky odešlou back toohello tabulky při ukončení hello funkce úspěšně.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-110">In a C# and F# functions, any changes made toohello record are automatically sent back toohello table when hello function exits successfully.</span></span>

<span data-ttu-id="8fe0a-111">Hello Mobile Apps vstupní tooa funkce používá následující objekt JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-111">hello Mobile Apps input tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of hello record tooretrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="8fe0a-112">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-112">Note hello following:</span></span>

* <span data-ttu-id="8fe0a-113">`id`může být statické nebo může být založen na hello aktivační událost, která volá funkce hello.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-113">`id` can be static, or it can be based on hello trigger that invokes hello function.</span></span> <span data-ttu-id="8fe0a-114">Například, pokud používáte [aktivační událost fronty]() pro funkce, pak `"id": "{queueTrigger}"` používá hello řetězcovou hodnotou obsahující zprávu fronty hello jako hello záznamů tooretrieve ID.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses hello string value of hello queue message as hello record ID tooretrieve.</span></span>
* <span data-ttu-id="8fe0a-115">`connection`by mělo obsahovat název hello nastavení aplikace v aplikaci funkce, která obsahuje adresu URL hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-115">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="8fe0a-116">Funkce Hello používá tuto adresu URL tooconstruct hello požadované operace REST proti mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-116">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="8fe0a-117">Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost Vstupní vazba.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="8fe0a-118">Je třeba toospecify `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="8fe0a-118">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="8fe0a-119">toodo, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč hello rozhraní API, pak přidejte hello `apiKey` vlastnost vaše Vstupní vazba s hello název nastavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-119">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="8fe0a-120">Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="8fe0a-121">By mělo být distribuované bezpečně tooservice straně klienty, jako jsou Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-121">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="8fe0a-122">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="8fe0a-123">To chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="8fe0a-124">Vstupní využití</span><span class="sxs-lookup"><span data-stu-id="8fe0a-124">Input usage</span></span>
<span data-ttu-id="8fe0a-125">Tato část uvádí, jak toouse Mobile Apps vstupní vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-125">This section shows you how toouse your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="8fe0a-126">Pokud záznam hello se hello zadána tabulka a záznam ID nenajde, je předána do hello s názvem [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametr (nebo v Node.js, je předaná do hello `context.bindings.<name>` objekt).</span><span class="sxs-lookup"><span data-stu-id="8fe0a-126">When hello record with hello specified table and record ID is found, it is passed into hello named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into hello `context.bindings.<name>` object).</span></span> <span data-ttu-id="8fe0a-127">Když se nenašel záznam hello, parametr hello je `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-127">When hello record is not found, hello parameter is `null`.</span></span> 

<span data-ttu-id="8fe0a-128">Funkcí jazyka C# a F # je všechny změny toohello vstupní záznamu (vstupní parametr) automaticky odeslán zpět toohello Mobile Apps tabulce, při ukončení hello funkce úspěšně.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-128">In C# and F# functions, any changes you make toohello input record (input parameter) is automatically sent back toohello Mobile Apps table when hello function exits successfully.</span></span> <span data-ttu-id="8fe0a-129">Ve funkcích Node.js, použijte `context.bindings.<name>` tooaccess hello vstupního záznamu.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-129">In Node.js functions, use `context.bindings.<name>` tooaccess hello input record.</span></span> <span data-ttu-id="8fe0a-130">Nelze upravit záznam v Node.js.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="8fe0a-131">Vstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="8fe0a-131">Input sample</span></span>
<span data-ttu-id="8fe0a-132">Předpokládejme, že máte hello následující function.json, který načte záznam tabulky mobilní aplikace s id hello hello fronty aktivační událost zprávy:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-132">Suppose you have hello following function.json, that retrieves a Mobile App table record with hello id of hello queue trigger message:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

<span data-ttu-id="8fe0a-133">V tématu vzorku hello konkrétní jazyk, který používá hello vstupního záznamu z vazby hello.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-133">See hello language-specific sample that uses hello input record from hello binding.</span></span> <span data-ttu-id="8fe0a-134">Ukázky jazyka C# a F # Hello také upravit záznam hello `text` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-134">hello C# and F# samples also modify hello record's `text` property.</span></span>

* [<span data-ttu-id="8fe0a-135">C#</span><span class="sxs-lookup"><span data-stu-id="8fe0a-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="8fe0a-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fe0a-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="8fe0a-137">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="8fe0a-137">Input sample in C#</span></span> #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="8fe0a-138">Vstupní ukázka v Node.js</span><span class="sxs-lookup"><span data-stu-id="8fe0a-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="8fe0a-139">Mobile Apps výstup vazby</span><span class="sxs-lookup"><span data-stu-id="8fe0a-139">Mobile Apps output binding</span></span>
<span data-ttu-id="8fe0a-140">Použijte hello Mobile Apps výstup vazby toowrite nový koncový bod záznamů tooa tabulky mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-140">Use hello Mobile Apps output binding toowrite a new record tooa Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="8fe0a-141">Hello Mobile Apps výstup hello následující objekt JSON v hello používá funkci `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-141">hello Mobile Apps output for a function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

<span data-ttu-id="8fe0a-142">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-142">Note hello following:</span></span>

* <span data-ttu-id="8fe0a-143">`connection`by mělo obsahovat název hello nastavení aplikace v aplikaci funkce, která obsahuje adresu URL hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-143">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="8fe0a-144">Funkce Hello používá tuto adresu URL tooconstruct hello požadované operace REST proti mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-144">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="8fe0a-145">Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost Vstupní vazba.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="8fe0a-146">Je třeba toospecify `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="8fe0a-146">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="8fe0a-147">toodo, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč hello rozhraní API, pak přidejte hello `apiKey` vlastnost vaše Vstupní vazba s hello název nastavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-147">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="8fe0a-148">Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="8fe0a-149">By mělo být distribuované bezpečně tooservice straně klienty, jako jsou Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-149">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="8fe0a-150">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="8fe0a-151">To chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="8fe0a-152">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="8fe0a-152">Output usage</span></span>
<span data-ttu-id="8fe0a-153">Tato část uvádí, jak toouse Mobile Apps výstup vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-153">This section shows you how toouse your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="8fe0a-154">V C# funkce, použijte parametr s názvem výstup typu `out object` tooaccess hello výstup záznamu.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-154">In C# functions, use a named output parameter of type `out object` tooaccess hello output record.</span></span> <span data-ttu-id="8fe0a-155">Ve funkcích Node.js, použijte `context.bindings.<name>` tooaccess hello výstup záznamu.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-155">In Node.js functions, use `context.bindings.<name>` tooaccess hello output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="8fe0a-156">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="8fe0a-156">Output sample</span></span>
<span data-ttu-id="8fe0a-157">Předpokládejme, že máte následující function.json, který definuje aktivační procedury fronty a Mobile Apps výstup hello:</span><span class="sxs-lookup"><span data-stu-id="8fe0a-157">Suppose you have hello following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

<span data-ttu-id="8fe0a-158">V tématu vzorku hello konkrétní jazyk, který vytvoří záznam v koncový bod hello Mobile Apps tabulek s obsahem hello uvítací zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="8fe0a-158">See hello language-specific sample that creates a record in hello Mobile Apps table endpoint with hello content of hello queue message.</span></span>

* [<span data-ttu-id="8fe0a-159">C#</span><span class="sxs-lookup"><span data-stu-id="8fe0a-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="8fe0a-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fe0a-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="8fe0a-161">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="8fe0a-161">Output sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="8fe0a-162">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="8fe0a-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="8fe0a-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8fe0a-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

