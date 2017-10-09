---
title: vazby funkce Cosmos DB aaaAzure | Microsoft Docs
description: Pochopit, jak vazeb Azure Cosmos DB toouse v Azure Functions.
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
ms.openlocfilehash: 76b89e8296db1dd28dff9528903b1f6a28f55232
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-cosmos-db-bindings"></a><span data-ttu-id="42af0-104">Azure DB Cosmos funkce vazby</span><span class="sxs-lookup"><span data-stu-id="42af0-104">Azure Functions Cosmos DB bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="42af0-105">Tento článek vysvětluje, jak Azure Cosmos DB vazeb tooconfigure a kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="42af0-105">This article explains how tooconfigure and code Azure Cosmos DB bindings in Azure Functions.</span></span> <span data-ttu-id="42af0-106">Azure Functions podporuje vstup a výstup vazby pro Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="42af0-106">Azure Functions supports input and output bindings for Cosmos DB.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="42af0-107">Další informace o Cosmos DB najdete v tématu [tooCosmos Úvod DB](../documentdb/documentdb-introduction.md) a [vytvoření konzolové aplikace Cosmos DB](../documentdb/documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="42af0-107">For more information on Cosmos DB, see [Introduction tooCosmos DB](../documentdb/documentdb-introduction.md) and [Build a Cosmos DB console application](../documentdb/documentdb-get-started.md).</span></span>

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a><span data-ttu-id="42af0-108">Rozhraní API DocumentDB vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="42af0-108">DocumentDB API input binding</span></span>
<span data-ttu-id="42af0-109">Hello Vstupní vazba rozhraní API DocumentDB Cosmos DB dokumentu načte a předává je toohello s názvem vstupní parametr funkce hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-109">hello DocumentDB API input binding retrieves a Cosmos DB document and passes it toohello named input parameter of hello function.</span></span> <span data-ttu-id="42af0-110">Hello dokument, který se dá určit ID podle hello aktivační událost, která volá funkce hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-110">hello document ID can be determined based on hello trigger that invokes hello function.</span></span> 

<span data-ttu-id="42af0-111">Hello Vstupní vazba rozhraní API DocumentDB má následující vlastnosti v hello *function.json*:</span><span class="sxs-lookup"><span data-stu-id="42af0-111">hello DocumentDB API input binding has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="42af0-112">`name`: Název identifikátor používaný v kódu funkce pro dokument hello</span><span class="sxs-lookup"><span data-stu-id="42af0-112">`name` : Identifier name used in function code for hello document</span></span>
- <span data-ttu-id="42af0-113">`type`: musí být nastaven příliš "documentdb"</span><span class="sxs-lookup"><span data-stu-id="42af0-113">`type` : must be set too"documentdb"</span></span>
- <span data-ttu-id="42af0-114">`databaseName`: hello databáze obsahující hello dokumentu</span><span class="sxs-lookup"><span data-stu-id="42af0-114">`databaseName` : hello database containing hello document</span></span>
- <span data-ttu-id="42af0-115">`collectionName`: hello kolekce obsahující hello dokumentu</span><span class="sxs-lookup"><span data-stu-id="42af0-115">`collectionName` : hello collection containing hello document</span></span>
- <span data-ttu-id="42af0-116">`id`: hello Id dokumentu tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-116">`id` : hello Id of hello document tooretrieve.</span></span> <span data-ttu-id="42af0-117">Tato vlastnost podporuje vazby parametrů; v tématu [vazbu vlastnosti vstupu toocustom ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) v článku hello [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="42af0-117">This property supports bindings parameters; see [Bind toocustom input properties in a binding expression](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) in hello article [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>
- <span data-ttu-id="42af0-118">`sqlQuery`: Dotaz Cosmos DB SQL použitý k načtení více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="42af0-118">`sqlQuery` : A Cosmos DB SQL query used for retrieving multiple documents.</span></span> <span data-ttu-id="42af0-119">Hello query podporuje runtime vazby.</span><span class="sxs-lookup"><span data-stu-id="42af0-119">hello query supports runtime bindings.</span></span> <span data-ttu-id="42af0-120">Příklad: `SELECT * FROM c where c.departmentId = {departmentId}`</span><span class="sxs-lookup"><span data-stu-id="42af0-120">For example: `SELECT * FROM c where c.departmentId = {departmentId}`</span></span>
- <span data-ttu-id="42af0-121">`connection`: hello název nastavení aplikace hello obsahující připojovací řetězec databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="42af0-121">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="42af0-122">`direction`: musí být nastaven příliš`"in"`.</span><span class="sxs-lookup"><span data-stu-id="42af0-122">`direction`  : must be set too`"in"`.</span></span>

<span data-ttu-id="42af0-123">Hello vlastnosti `id` a `sqlQuery` nelze zadat současně.</span><span class="sxs-lookup"><span data-stu-id="42af0-123">hello properties `id` and `sqlQuery` cannot both be specified.</span></span> <span data-ttu-id="42af0-124">Pokud ani `id` ani `sqlQuery` nastavena, hello celou kolekci se načítají.</span><span class="sxs-lookup"><span data-stu-id="42af0-124">If neither `id` nor `sqlQuery` is set, hello entire collection is retrieved.</span></span>

## <a name="using-a-documentdb-api-input-binding"></a><span data-ttu-id="42af0-125">Použití rozhraní API DocumentDB vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="42af0-125">Using a DocumentDB API input binding</span></span>

* <span data-ttu-id="42af0-126">V jazyce C# a F # funkce při ukončení hello funkce úspěšně, některé změny provedené v toohello vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="42af0-126">In C# and F# functions, when hello function exits successfully, any changes made toohello input document via named input parameters are automatically persisted.</span></span> 
* <span data-ttu-id="42af0-127">V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce.</span><span class="sxs-lookup"><span data-stu-id="42af0-127">In JavaScript functions, updates are not made automatically upon function exit.</span></span> <span data-ttu-id="42af0-128">Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` toomake aktualizace.</span><span class="sxs-lookup"><span data-stu-id="42af0-128">Instead, use `context.bindings.<documentName>In` and `context.bindings.<documentName>Out` toomake updates.</span></span> <span data-ttu-id="42af0-129">V tématu hello [JavaScript ukázka](#injavascript).</span><span class="sxs-lookup"><span data-stu-id="42af0-129">See hello [JavaScript sample](#injavascript).</span></span>

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a><span data-ttu-id="42af0-130">Vstupní vzorek pro jednotlivý dokument</span><span class="sxs-lookup"><span data-stu-id="42af0-130">Input sample for single document</span></span>
<span data-ttu-id="42af0-131">Předpokládejme, že máte následující hello DocumentDB API vstupní vazby v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="42af0-131">Suppose you have hello following DocumentDB API input binding in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="42af0-132">V tématu vzorku hello konkrétní jazyk, který používá vstupní vazby tooupdate hello tohoto dokumentu textovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="42af0-132">See hello language-specific sample that uses this input binding tooupdate hello document's text value.</span></span>

* [<span data-ttu-id="42af0-133">C#</span><span class="sxs-lookup"><span data-stu-id="42af0-133">C#</span></span>](#incsharp)
* [<span data-ttu-id="42af0-134">F#</span><span class="sxs-lookup"><span data-stu-id="42af0-134">F#</span></span>](#infsharp)
* [<span data-ttu-id="42af0-135">JavaScript</span><span class="sxs-lookup"><span data-stu-id="42af0-135">JavaScript</span></span>](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a><span data-ttu-id="42af0-136">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="42af0-136">Input sample in C#</span></span> #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a><span data-ttu-id="42af0-137">Vstupní ukázka v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="42af0-137">Input sample in F#</span></span> #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

<span data-ttu-id="42af0-138">Tato ukázka vyžaduje `project.json` soubor, který určuje hello `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:</span><span class="sxs-lookup"><span data-stu-id="42af0-138">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

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

<span data-ttu-id="42af0-139">tooadd `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="42af0-139">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a><span data-ttu-id="42af0-140">Vstupní ukázka v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="42af0-140">Input sample in JavaScript</span></span>

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a><span data-ttu-id="42af0-141">Vstupní ukázkové s více dokumenty</span><span class="sxs-lookup"><span data-stu-id="42af0-141">Input sample with multiple documents</span></span>

<span data-ttu-id="42af0-142">Předpokládejme, že si přejete tooretrieve více dokumentů, zadaný v dotazu SQL pomocí parametry dotazu toocustomize hello frontě aktivační události.</span><span class="sxs-lookup"><span data-stu-id="42af0-142">Suppose that you wish tooretrieve multiple documents specified by a SQL query, using a queue trigger toocustomize hello query parameters.</span></span> 

<span data-ttu-id="42af0-143">V tomto příkladu hello frontě aktivační události obsahuje parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro hello finančního oddělení.</span><span class="sxs-lookup"><span data-stu-id="42af0-143">In this example, hello queue trigger provides a parameter `departmentId`.A queue message of `{ "departmentId" : "Finance" }` would return all records for hello finance department.</span></span> <span data-ttu-id="42af0-144">Použijte hello v *function.json*:</span><span class="sxs-lookup"><span data-stu-id="42af0-144">Use hello following in *function.json*:</span></span>

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

### <a name="input-sample-with-multiple-documents-in-c"></a><span data-ttu-id="42af0-145">Vstupní ukázkové s více dokumenty v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="42af0-145">Input sample with multiple documents in C#</span></span>

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

### <a name="input-sample-with-multiple-documents-in-javascript"></a><span data-ttu-id="42af0-146">Vstupní ukázkové s více dokumenty v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="42af0-146">Input sample with multiple documents in JavaScript</span></span>

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

## <span data-ttu-id="42af0-147"><a id="docdboutput"></a>Rozhraní API DocumentDB výstup vazby</span><span class="sxs-lookup"><span data-stu-id="42af0-147"><a id="docdboutput"></a>DocumentDB API output binding</span></span>
<span data-ttu-id="42af0-148">Hello DocumentDB API výstupu vazby umožňuje zapisovat novou databázi Azure Cosmos DB tooan dokumentu.</span><span class="sxs-lookup"><span data-stu-id="42af0-148">hello DocumentDB API output binding lets you write a new document tooan Azure Cosmos DB database.</span></span> <span data-ttu-id="42af0-149">Má následující vlastnosti v hello *function.json*:</span><span class="sxs-lookup"><span data-stu-id="42af0-149">It has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="42af0-150">`name`: Identifikátor použitý v kódu funkce pro nový dokument hello</span><span class="sxs-lookup"><span data-stu-id="42af0-150">`name` : Identifier used in function code for hello new document</span></span>
- <span data-ttu-id="42af0-151">`type`: musí být nastaven příliš`"documentdb"`</span><span class="sxs-lookup"><span data-stu-id="42af0-151">`type` : must be set too`"documentdb"`</span></span>
- <span data-ttu-id="42af0-152">`databaseName`: hello databáze obsahující hello kolekce, kde bude vytvořen nový dokument hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-152">`databaseName` : hello database containing hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="42af0-153">`collectionName`: hello kolekce, kde bude vytvořen nový dokument hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-153">`collectionName` : hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="42af0-154">`createIfNotExists`: Logická hodnota. hodnota tooindicate, zda text hello kolekce se vytvoří, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="42af0-154">`createIfNotExists` : A boolean value tooindicate whether hello collection will be created if it does not exist.</span></span> <span data-ttu-id="42af0-155">Výchozí hodnota Hello je *false*.</span><span class="sxs-lookup"><span data-stu-id="42af0-155">hello default is *false*.</span></span> <span data-ttu-id="42af0-156">Hello důvod pro toto je nové kolekce se vytvoří s vyhrazenou propustností, kterou se hradí.</span><span class="sxs-lookup"><span data-stu-id="42af0-156">hello reason for this is new collections are created with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="42af0-157">Další podrobnosti naleznete na adrese hello [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="42af0-157">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/).</span></span>
- <span data-ttu-id="42af0-158">`connection`: hello název nastavení aplikace hello obsahující připojovací řetězec databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="42af0-158">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="42af0-159">`direction`: musí být nastaven příliš`"out"`</span><span class="sxs-lookup"><span data-stu-id="42af0-159">`direction` : must be set too`"out"`</span></span>

## <a name="using-a-documentdb-api-output-binding"></a><span data-ttu-id="42af0-160">Pomocí rozhraní API DocumentDB výstup vazby</span><span class="sxs-lookup"><span data-stu-id="42af0-160">Using a DocumentDB API output binding</span></span>
<span data-ttu-id="42af0-161">Tato část uvádí, jak toouse rozhraní API DocumentDB výstup vazby v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="42af0-161">This section shows you how toouse your DocumentDB API output binding in your function code.</span></span>

<span data-ttu-id="42af0-162">Při psaní toohello výstupní parametr do funkce, ve výchozím nastavení do nového dokumentu se generuje ve vaší databázi, se automaticky vytvářenému identifikátoru GUID jako hello dokumentu ID.</span><span class="sxs-lookup"><span data-stu-id="42af0-162">When you write toohello output parameter in your function, by default a new document is generated in your database, with an automatically generated GUID as hello document ID.</span></span> <span data-ttu-id="42af0-163">Můžete zadat ID dokumentu hello výstup dokumentu zadáním hello `id` vlastnost JSON v hello výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="42af0-163">You can specify hello document ID of output document by specifying hello `id` JSON property in hello output parameter.</span></span> 

>[!Note]  
><span data-ttu-id="42af0-164">Pokud zadáte ID hello stávající dokument, získá přepsány nový dokument výstup hello.</span><span class="sxs-lookup"><span data-stu-id="42af0-164">When you specify hello ID of an existing document, it gets overwritten by hello new output document.</span></span> 

<span data-ttu-id="42af0-165">toooutput více dokumentů, můžete také navázat příliš`ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jeden z typů hello podporována.</span><span class="sxs-lookup"><span data-stu-id="42af0-165">toooutput multiple documents, you can also bind too`ICollector<T>` or `IAsyncCollector<T>` where `T` is one of hello supported types.</span></span>

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a><span data-ttu-id="42af0-166">Ukázka výstupu vazby DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="42af0-166">DocumentDB API output binding sample</span></span>
<span data-ttu-id="42af0-167">Předpokládejme, že máte následující hello DocumentDB API výstup vazby v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="42af0-167">Suppose you have hello following DocumentDB API output binding in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="42af0-168">A máte vazbu vstupní fronty pro frontu, která přijímá JSON v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="42af0-168">And you have a queue input binding for a queue that receives JSON in hello following format:</span></span>

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="42af0-169">A chcete, aby toocreate Cosmos DB dokumenty ve formátu pro každý záznam hello:</span><span class="sxs-lookup"><span data-stu-id="42af0-169">And you want toocreate Cosmos DB documents in hello following format for each record:</span></span>

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="42af0-170">V tématu vzorku hello konkrétní jazyk, který používá tento výstup vazby tooadd dokumenty tooyour databázi.</span><span class="sxs-lookup"><span data-stu-id="42af0-170">See hello language-specific sample that uses this output binding tooadd documents tooyour database.</span></span>

* [<span data-ttu-id="42af0-171">C#</span><span class="sxs-lookup"><span data-stu-id="42af0-171">C#</span></span>](#outcsharp)
* [<span data-ttu-id="42af0-172">F#</span><span class="sxs-lookup"><span data-stu-id="42af0-172">F#</span></span>](#outfsharp)
* [<span data-ttu-id="42af0-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="42af0-173">JavaScript</span></span>](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="42af0-174">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="42af0-174">Output sample in C#</span></span> #

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

### <a name="output-sample-in-f"></a><span data-ttu-id="42af0-175">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="42af0-175">Output sample in F#</span></span> #

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

<span data-ttu-id="42af0-176">Tato ukázka vyžaduje `project.json` soubor, který určuje hello `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:</span><span class="sxs-lookup"><span data-stu-id="42af0-176">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

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

<span data-ttu-id="42af0-177">tooadd `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="42af0-177">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a><span data-ttu-id="42af0-178">Ukázka výstupu v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="42af0-178">Output sample in JavaScript</span></span>

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
