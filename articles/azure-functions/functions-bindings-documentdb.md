---
title: Azure DB Cosmos funkce vazby | Microsoft Docs
description: "Pochopit, jak používat Azure Cosmos DB vazby v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/18/2016
ms.author: glenga
ms.openlocfilehash: de95b0591eb95e76dbb7ba2382e9e14e1f66cda1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-cosmos-db-bindings"></a><span data-ttu-id="62d77-104">Azure DB Cosmos funkce vazby</span><span class="sxs-lookup"><span data-stu-id="62d77-104">Azure Functions Cosmos DB bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="62d77-105">Tento článek vysvětluje, jak nakonfigurovat a vazeb Azure Cosmos DB kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="62d77-105">This article explains how to configure and code Azure Cosmos DB bindings in Azure Functions.</span></span> <span data-ttu-id="62d77-106">Azure Functions podporuje vstup a výstup vazby pro Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="62d77-106">Azure Functions supports input and output bindings for Cosmos DB.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="62d77-107">Další informace o Cosmos DB najdete v tématu [Úvod do Cosmos DB](../documentdb/documentdb-introduction.md) a [vytvoření konzolové aplikace Cosmos DB](../documentdb/documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="62d77-107">For more information on Cosmos DB, see [Introduction to Cosmos DB](../documentdb/documentdb-introduction.md) and [Build a Cosmos DB console application](../documentdb/documentdb-get-started.md).</span></span>

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a><span data-ttu-id="62d77-108">Rozhraní API DocumentDB vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="62d77-108">DocumentDB API input binding</span></span>
<span data-ttu-id="62d77-109">Vstupní vazba rozhraní API DocumentDB Cosmos DB dokumentu načte a předává je pro pojmenované vstupní parametr funkce.</span><span class="sxs-lookup"><span data-stu-id="62d77-109">The DocumentDB API input binding retrieves a Cosmos DB document and passes it to the named input parameter of the function.</span></span> <span data-ttu-id="62d77-110">Dokument, který se dá určit ID založené na aktivační událost, která volá funkci.</span><span class="sxs-lookup"><span data-stu-id="62d77-110">The document ID can be determined based on the trigger that invokes the function.</span></span> 

<span data-ttu-id="62d77-111">Vstupní vazba rozhraní API DocumentDB má následující vlastnosti *function.json*:</span><span class="sxs-lookup"><span data-stu-id="62d77-111">The DocumentDB API input binding has the following properties in *function.json*:</span></span>

- <span data-ttu-id="62d77-112">`name`: Název identifikátor použitý v kódu funkce pro dokument</span><span class="sxs-lookup"><span data-stu-id="62d77-112">`name` : Identifier name used in function code for the document</span></span>
- <span data-ttu-id="62d77-113">`type`: musí být nastavena na "documentdb"</span><span class="sxs-lookup"><span data-stu-id="62d77-113">`type` : must be set to "documentdb"</span></span>
- <span data-ttu-id="62d77-114">`databaseName`: Databáze obsahující dokumentu</span><span class="sxs-lookup"><span data-stu-id="62d77-114">`databaseName` : The database containing the document</span></span>
- <span data-ttu-id="62d77-115">`collectionName`: Kolekce obsahující dokumentu</span><span class="sxs-lookup"><span data-stu-id="62d77-115">`collectionName` : The collection containing the document</span></span>
- <span data-ttu-id="62d77-116">`id`: Id dokumentu pro načtení.</span><span class="sxs-lookup"><span data-stu-id="62d77-116">`id` : The Id of the document to retrieve.</span></span> <span data-ttu-id="62d77-117">Tato vlastnost podporuje vazby parametrů; v tématu [vazby na vlastní vstupní vlastnosti ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) v článku [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="62d77-117">This property supports bindings parameters; see [Bind to custom input properties in a binding expression](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) in the article [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>
- <span data-ttu-id="62d77-118">`sqlQuery`: Dotaz Cosmos DB SQL použitý k načtení více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="62d77-118">`sqlQuery` : A Cosmos DB SQL query used for retrieving multiple documents.</span></span> <span data-ttu-id="62d77-119">Dotaz podporuje runtime vazby.</span><span class="sxs-lookup"><span data-stu-id="62d77-119">The query supports runtime bindings.</span></span> <span data-ttu-id="62d77-120">Příklad: `SELECT * FROM c where c.departmentId = {departmentId}`</span><span class="sxs-lookup"><span data-stu-id="62d77-120">For example: `SELECT * FROM c where c.departmentId = {departmentId}`</span></span>
- <span data-ttu-id="62d77-121">`connection`: Název nastavení aplikace obsahující připojovací řetězec databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="62d77-121">`connection` : The name of the app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="62d77-122">`direction`: musí být nastavena na `"in"`.</span><span class="sxs-lookup"><span data-stu-id="62d77-122">`direction`  : must be set to `"in"`.</span></span>

<span data-ttu-id="62d77-123">Vlastnosti `id` a `sqlQuery` nelze zadat současně.</span><span class="sxs-lookup"><span data-stu-id="62d77-123">The properties `id` and `sqlQuery` cannot both be specified.</span></span> <span data-ttu-id="62d77-124">Pokud ani `id` ani `sqlQuery` není nastaven, je-li načíst celou kolekci.</span><span class="sxs-lookup"><span data-stu-id="62d77-124">If neither `id` nor `sqlQuery` is set, the entire collection is retrieved.</span></span>

## <a name="using-a-documentdb-api-input-binding"></a><span data-ttu-id="62d77-125">Použití rozhraní API DocumentDB vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="62d77-125">Using a DocumentDB API input binding</span></span>

* <span data-ttu-id="62d77-126">V jazyce C# a F # funkce při ukončení funkce úspěšně, některé změny provedené vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="62d77-126">In C# and F# functions, when the function exits successfully, any changes made to the input document via named input parameters are automatically persisted.</span></span> 
* <span data-ttu-id="62d77-127">V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce.</span><span class="sxs-lookup"><span data-stu-id="62d77-127">In JavaScript functions, updates are not made automatically upon function exit.</span></span> <span data-ttu-id="62d77-128">Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` pro nastavení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="62d77-128">Instead, use `context.bindings.<documentName>In` and `context.bindings.<documentName>Out` to make updates.</span></span> <span data-ttu-id="62d77-129">Najdete v článku [JavaScript ukázka](#injavascript).</span><span class="sxs-lookup"><span data-stu-id="62d77-129">See the [JavaScript sample](#injavascript).</span></span>

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a><span data-ttu-id="62d77-130">Vstupní vzorek pro jednotlivý dokument</span><span class="sxs-lookup"><span data-stu-id="62d77-130">Input sample for single document</span></span>
<span data-ttu-id="62d77-131">Předpokládejme, že máte následující rozhraní API DocumentDB vstupní vazby v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="62d77-131">Suppose you have the following DocumentDB API input binding in the `bindings` array of function.json:</span></span>

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

<span data-ttu-id="62d77-132">V tématu vzorku pro specifický jazyk, který používá tuto vstupní vazbu k aktualizaci dokumentu textovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="62d77-132">See the language-specific sample that uses this input binding to update the document's text value.</span></span>

* [<span data-ttu-id="62d77-133">C#</span><span class="sxs-lookup"><span data-stu-id="62d77-133">C#</span></span>](#incsharp)
* [<span data-ttu-id="62d77-134">F#</span><span class="sxs-lookup"><span data-stu-id="62d77-134">F#</span></span>](#infsharp)
* [<span data-ttu-id="62d77-135">JavaScript</span><span class="sxs-lookup"><span data-stu-id="62d77-135">JavaScript</span></span>](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a><span data-ttu-id="62d77-136">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="62d77-136">Input sample in C#</span></span> #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a><span data-ttu-id="62d77-137">Vstupní ukázka v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="62d77-137">Input sample in F#</span></span> #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

<span data-ttu-id="62d77-138">Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:</span><span class="sxs-lookup"><span data-stu-id="62d77-138">This sample requires a `project.json` file that specifies the `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="62d77-139">Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="62d77-139">To add a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a><span data-ttu-id="62d77-140">Vstupní ukázka v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="62d77-140">Input sample in JavaScript</span></span>

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a><span data-ttu-id="62d77-141">Vstupní ukázkové s více dokumenty</span><span class="sxs-lookup"><span data-stu-id="62d77-141">Input sample with multiple documents</span></span>

<span data-ttu-id="62d77-142">Předpokládejme, které chcete načíst více dokumentů určeného dotaz SQL, chcete-li přizpůsobit parametry dotazu pomocí aktivační procedury fronty.</span><span class="sxs-lookup"><span data-stu-id="62d77-142">Suppose that you wish to retrieve multiple documents specified by a SQL query, using a queue trigger to customize the query parameters.</span></span> 

<span data-ttu-id="62d77-143">V tomto příkladu obsahuje aktivační událost fronty parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro finančního oddělení.</span><span class="sxs-lookup"><span data-stu-id="62d77-143">In this example, the queue trigger provides a parameter `departmentId`.A queue message of `{ "departmentId" : "Finance" }` would return all records for the finance department.</span></span> <span data-ttu-id="62d77-144">Použijte následující *function.json*:</span><span class="sxs-lookup"><span data-stu-id="62d77-144">Use the following in *function.json*:</span></span>

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a><span data-ttu-id="62d77-145">Vstupní ukázkové s více dokumenty v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="62d77-145">Input sample with multiple documents in C#</span></span>

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a><span data-ttu-id="62d77-146">Vstupní ukázkové s více dokumenty v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="62d77-146">Input sample with multiple documents in JavaScript</span></span>

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <span data-ttu-id="62d77-147"><a id="docdboutput"></a>Rozhraní API DocumentDB výstup vazby</span><span class="sxs-lookup"><span data-stu-id="62d77-147"><a id="docdboutput"></a>DocumentDB API output binding</span></span>
<span data-ttu-id="62d77-148">Rozhraní API DocumentDB výstup vazby umožňuje zapsat nový dokument k databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="62d77-148">The DocumentDB API output binding lets you write a new document to an Azure Cosmos DB database.</span></span> <span data-ttu-id="62d77-149">Má následující vlastnosti v *function.json*:</span><span class="sxs-lookup"><span data-stu-id="62d77-149">It has the following properties in *function.json*:</span></span>

- <span data-ttu-id="62d77-150">`name`: Identifikátor použitý v kódu funkce pro nový dokument</span><span class="sxs-lookup"><span data-stu-id="62d77-150">`name` : Identifier used in function code for the new document</span></span>
- <span data-ttu-id="62d77-151">`type`: musí být nastavený na`"documentdb"`</span><span class="sxs-lookup"><span data-stu-id="62d77-151">`type` : must be set to `"documentdb"`</span></span>
- <span data-ttu-id="62d77-152">`databaseName`: Databáze obsahující kolekci, kde bude vytvořen nový dokument.</span><span class="sxs-lookup"><span data-stu-id="62d77-152">`databaseName` : The database containing the collection where the new document will be created.</span></span>
- <span data-ttu-id="62d77-153">`collectionName`: Kolekce, kde bude vytvořen nový dokument.</span><span class="sxs-lookup"><span data-stu-id="62d77-153">`collectionName` : The collection where the new document will be created.</span></span>
- <span data-ttu-id="62d77-154">`createIfNotExists`: Logická hodnota označující, zda kolekce bude vytvořen, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="62d77-154">`createIfNotExists` : A boolean value to indicate whether the collection will be created if it does not exist.</span></span> <span data-ttu-id="62d77-155">Výchozí hodnota je *false*.</span><span class="sxs-lookup"><span data-stu-id="62d77-155">The default is *false*.</span></span> <span data-ttu-id="62d77-156">Z důvodu pro toto je nové kolekce se vytvoří s vyhrazenou propustností, kterou se hradí.</span><span class="sxs-lookup"><span data-stu-id="62d77-156">The reason for this is new collections are created with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="62d77-157">Další podrobnosti naleznete [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="62d77-157">For more details, please visit the [pricing page](https://azure.microsoft.com/pricing/details/documentdb/).</span></span>
- <span data-ttu-id="62d77-158">`connection`: Název nastavení aplikace obsahující připojovací řetězec databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="62d77-158">`connection` : The name of the app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="62d77-159">`direction`: musí být nastavený na`"out"`</span><span class="sxs-lookup"><span data-stu-id="62d77-159">`direction` : must be set to `"out"`</span></span>

## <a name="using-a-documentdb-api-output-binding"></a><span data-ttu-id="62d77-160">Pomocí rozhraní API DocumentDB výstup vazby</span><span class="sxs-lookup"><span data-stu-id="62d77-160">Using a DocumentDB API output binding</span></span>
<span data-ttu-id="62d77-161">V této části se dozvíte, jak používat rozhraní API DocumentDB výstupu vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="62d77-161">This section shows you how to use your DocumentDB API output binding in your function code.</span></span>

<span data-ttu-id="62d77-162">Při zápisu do výstupního parametru ve vaší funkci ve výchozím nastavení je nový dokument vygenerované v databázi, se automaticky vytvářenému identifikátoru GUID jako ID dokumentu.</span><span class="sxs-lookup"><span data-stu-id="62d77-162">When you write to the output parameter in your function, by default a new document is generated in your database, with an automatically generated GUID as the document ID.</span></span> <span data-ttu-id="62d77-163">Můžete zadat ID dokumentu výstup dokumentu zadáním `id` vlastnost JSON v výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="62d77-163">You can specify the document ID of output document by specifying the `id` JSON property in the output parameter.</span></span> 

>[!Note]  
><span data-ttu-id="62d77-164">Pokud zadáte ID stávající dokument, získá přepsány nový dokument výstup.</span><span class="sxs-lookup"><span data-stu-id="62d77-164">When you specify the ID of an existing document, it gets overwritten by the new output document.</span></span> 

<span data-ttu-id="62d77-165">Výstup více dokumentů, můžete také vázat na `ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jedním z podporovaných typů.</span><span class="sxs-lookup"><span data-stu-id="62d77-165">To output multiple documents, you can also bind to `ICollector<T>` or `IAsyncCollector<T>` where `T` is one of the supported types.</span></span>

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a><span data-ttu-id="62d77-166">Ukázka výstupu vazby DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="62d77-166">DocumentDB API output binding sample</span></span>
<span data-ttu-id="62d77-167">Předpokládejme, že máte následující rozhraní API DocumentDB výstup vazby v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="62d77-167">Suppose you have the following DocumentDB API output binding in the `bindings` array of function.json:</span></span>

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

<span data-ttu-id="62d77-168">A máte vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="62d77-168">And you have a queue input binding for a queue that receives JSON in the following format:</span></span>

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="62d77-169">A chcete vytvořit Cosmos DB dokumenty ve formátu pro každý záznam:</span><span class="sxs-lookup"><span data-stu-id="62d77-169">And you want to create Cosmos DB documents in the following format for each record:</span></span>

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="62d77-170">V tématu vzorku pro specifický jazyk, který používá tuto vazbu výstup k přidání dokumenty k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="62d77-170">See the language-specific sample that uses this output binding to add documents to your database.</span></span>

* [<span data-ttu-id="62d77-171">C#</span><span class="sxs-lookup"><span data-stu-id="62d77-171">C#</span></span>](#outcsharp)
* [<span data-ttu-id="62d77-172">F#</span><span class="sxs-lookup"><span data-stu-id="62d77-172">F#</span></span>](#outfsharp)
* [<span data-ttu-id="62d77-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="62d77-173">JavaScript</span></span>](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="62d77-174">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="62d77-174">Output sample in C#</span></span> #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="62d77-175">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="62d77-175">Output sample in F#</span></span> #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

<span data-ttu-id="62d77-176">Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:</span><span class="sxs-lookup"><span data-stu-id="62d77-176">This sample requires a `project.json` file that specifies the `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="62d77-177">Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="62d77-177">To add a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a><span data-ttu-id="62d77-178">Ukázka výstupu v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="62d77-178">Output sample in JavaScript</span></span>

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```
