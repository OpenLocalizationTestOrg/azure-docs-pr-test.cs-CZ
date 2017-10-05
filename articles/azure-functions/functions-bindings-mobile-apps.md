---
title: Azure vazby funkce Mobile Apps | Microsoft Docs
description: "Pochopit, jak používat Azure Mobile Apps vazby v Azure Functions."
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
ms.openlocfilehash: c5e1c02984f9773b263c0bee7685c7d5ff62e658
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="e88b4-104">Azure Mobile Apps funkce vazby</span><span class="sxs-lookup"><span data-stu-id="e88b4-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="e88b4-105">Tento článek vysvětluje, jak nakonfigurovat a kódu [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e88b4-105">This article explains how to configure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="e88b4-106">Azure Functions podporuje vstup a výstup vazby pro Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="e88b4-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="e88b4-107">Mobile Apps vstup a výstup vazby umožňují [číst a zapisovat do datových tabulek](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) v mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e88b4-107">The Mobile Apps input and output bindings let you [read from and write to data tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="e88b4-108">Vstupní vazba Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="e88b4-108">Mobile Apps input binding</span></span>
<span data-ttu-id="e88b4-109">Vstupní vazba Mobile Apps načte záznam z koncového bodu mobilní tabulky a předá ji do funkce.</span><span class="sxs-lookup"><span data-stu-id="e88b4-109">The Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="e88b4-110">C# a F # funkce všechny změny provedené v záznamu se automaticky odešlou zpět k tabulce při ukončení funkce úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e88b4-110">In a C# and F# functions, any changes made to the record are automatically sent back to the table when the function exits successfully.</span></span>

<span data-ttu-id="e88b4-111">Mobile Apps vstup do funkce používá následující objekt JSON v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="e88b4-111">The Mobile Apps input to a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="e88b4-112">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="e88b4-112">Note the following:</span></span>

* <span data-ttu-id="e88b4-113">`id`může být statické nebo může být založen na aktivační událost, která volá funkci.</span><span class="sxs-lookup"><span data-stu-id="e88b4-113">`id` can be static, or it can be based on the trigger that invokes the function.</span></span> <span data-ttu-id="e88b4-114">Například, pokud používáte [aktivační událost fronty]() pro funkce, pak `"id": "{queueTrigger}"` používá s řetězcovou hodnotou obsahující zprávy ve frontě jako ID záznamu pro načtení.</span><span class="sxs-lookup"><span data-stu-id="e88b4-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses the string value of the queue message as the record ID to retrieve.</span></span>
* <span data-ttu-id="e88b4-115">`connection`by mělo obsahovat název nastavení aplikace v aplikaci funkce, která obsahuje adresu URL této mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-115">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="e88b4-116">Funkce používá tuto adresu URL k vytvoření požadované operace REST pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-116">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="e88b4-117">Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), pak zadejte název nastavení aplikace v `connection` vlastnost Vstupní vazba.</span><span class="sxs-lookup"><span data-stu-id="e88b4-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="e88b4-118">Je třeba zadat `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="e88b4-118">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="e88b4-119">K tomu, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč rozhraní API, přidejte `apiKey` vlastnost vaše Vstupní vazba s názvem nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-119">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="e88b4-120">Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="e88b4-121">Je měli jenom zajistit jeho distribuci bezpečně klientům straně služby, jako je Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e88b4-121">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="e88b4-122">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="e88b4-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="e88b4-123">To chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="e88b4-124">Vstupní využití</span><span class="sxs-lookup"><span data-stu-id="e88b4-124">Input usage</span></span>
<span data-ttu-id="e88b4-125">V této části se dozvíte, jak používat vaše mobilní aplikace vstupní vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="e88b4-125">This section shows you how to use your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="e88b4-126">Když se najde záznam se zadané tabulky a ID záznamu, je předaná do pojmenované [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametr (nebo v Node.js, je předaná do `context.bindings.<name>` objekt).</span><span class="sxs-lookup"><span data-stu-id="e88b4-126">When the record with the specified table and record ID is found, it is passed into the named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into the `context.bindings.<name>` object).</span></span> <span data-ttu-id="e88b4-127">Při záznamu není nalezen, je parametr `null`.</span><span class="sxs-lookup"><span data-stu-id="e88b4-127">When the record is not found, the parameter is `null`.</span></span> 

<span data-ttu-id="e88b4-128">Funkcí jazyka C# a F #, provedené změny do vstupní záznamu (vstupní parametr) je automaticky odeslán zpět k tabulce mobilní aplikace při ukončení funkce úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e88b4-128">In C# and F# functions, any changes you make to the input record (input parameter) is automatically sent back to the Mobile Apps table when the function exits successfully.</span></span> <span data-ttu-id="e88b4-129">Ve funkcích Node.js, použijte `context.bindings.<name>` pro přístup k vstupního záznamu.</span><span class="sxs-lookup"><span data-stu-id="e88b4-129">In Node.js functions, use `context.bindings.<name>` to access the input record.</span></span> <span data-ttu-id="e88b4-130">Nelze upravit záznam v Node.js.</span><span class="sxs-lookup"><span data-stu-id="e88b4-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="e88b4-131">Vstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="e88b4-131">Input sample</span></span>
<span data-ttu-id="e88b4-132">Předpokládejme, že máte následující function.json, který načte záznam tabulky mobilní aplikace s id zprávy ve frontě aktivační události:</span><span class="sxs-lookup"><span data-stu-id="e88b4-132">Suppose you have the following function.json, that retrieves a Mobile App table record with the id of the queue trigger message:</span></span>

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

<span data-ttu-id="e88b4-133">V tématu vzorku pro specifický jazyk, který používá vstupního záznamu z vazby.</span><span class="sxs-lookup"><span data-stu-id="e88b4-133">See the language-specific sample that uses the input record from the binding.</span></span> <span data-ttu-id="e88b4-134">Ukázky jazyka C# a F # také upravit na záznam `text` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e88b4-134">The C# and F# samples also modify the record's `text` property.</span></span>

* [<span data-ttu-id="e88b4-135">C#</span><span class="sxs-lookup"><span data-stu-id="e88b4-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="e88b4-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="e88b4-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="e88b4-137">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="e88b4-137">Input sample in C#</span></span> #

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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="e88b4-138">Vstupní ukázka v Node.js</span><span class="sxs-lookup"><span data-stu-id="e88b4-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="e88b4-139">Mobile Apps výstup vazby</span><span class="sxs-lookup"><span data-stu-id="e88b4-139">Mobile Apps output binding</span></span>
<span data-ttu-id="e88b4-140">Použijte výstup Mobile Apps vazby k zápisu nového záznamu do tabulky koncový bod mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-140">Use the Mobile Apps output binding to write a new record to a Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="e88b4-141">Mobile Apps výstup funkce používá následující objekt JSON v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="e88b4-141">The Mobile Apps output for a function uses the following JSON object in the `bindings` array of function.json:</span></span>

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

<span data-ttu-id="e88b4-142">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="e88b4-142">Note the following:</span></span>

* <span data-ttu-id="e88b4-143">`connection`by mělo obsahovat název nastavení aplikace v aplikaci funkce, která obsahuje adresu URL této mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-143">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="e88b4-144">Funkce používá tuto adresu URL k vytvoření požadované operace REST pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-144">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="e88b4-145">Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), pak zadejte název nastavení aplikace v `connection` vlastnost Vstupní vazba.</span><span class="sxs-lookup"><span data-stu-id="e88b4-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="e88b4-146">Je třeba zadat `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="e88b4-146">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="e88b4-147">K tomu, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč rozhraní API, přidejte `apiKey` vlastnost vaše Vstupní vazba s názvem nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-147">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="e88b4-148">Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="e88b4-149">Je měli jenom zajistit jeho distribuci bezpečně klientům straně služby, jako je Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e88b4-149">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="e88b4-150">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="e88b4-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="e88b4-151">To chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="e88b4-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="e88b4-152">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="e88b4-152">Output usage</span></span>
<span data-ttu-id="e88b4-153">V této části se dozvíte, jak používat Mobile Apps výstupu vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="e88b4-153">This section shows you how to use your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="e88b4-154">V C# funkce, použijte parametr s názvem výstup typu `out object` pro přístup k výstupu záznamu.</span><span class="sxs-lookup"><span data-stu-id="e88b4-154">In C# functions, use a named output parameter of type `out object` to access the output record.</span></span> <span data-ttu-id="e88b4-155">Ve funkcích Node.js, použijte `context.bindings.<name>` pro přístup k výstupu záznamu.</span><span class="sxs-lookup"><span data-stu-id="e88b4-155">In Node.js functions, use `context.bindings.<name>` to access the output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="e88b4-156">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="e88b4-156">Output sample</span></span>
<span data-ttu-id="e88b4-157">Předpokládejme, že máte následující function.json, která definuje aktivační procedury fronty a výstup mobilní aplikace:</span><span class="sxs-lookup"><span data-stu-id="e88b4-157">Suppose you have the following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

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

<span data-ttu-id="e88b4-158">Naleznete v ukázce pro specifický jazyk, který vytvoří záznam v koncovém bodě Mobile Apps tabulku s obsahem zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="e88b4-158">See the language-specific sample that creates a record in the Mobile Apps table endpoint with the content of the queue message.</span></span>

* [<span data-ttu-id="e88b4-159">C#</span><span class="sxs-lookup"><span data-stu-id="e88b4-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="e88b4-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="e88b4-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="e88b4-161">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="e88b4-161">Output sample in C#</span></span> #

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

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="e88b4-162">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="e88b4-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="e88b4-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e88b4-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

