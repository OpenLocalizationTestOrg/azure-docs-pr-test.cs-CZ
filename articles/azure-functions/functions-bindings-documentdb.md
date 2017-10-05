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
# <a name="azure-functions-cosmos-db-bindings"></a>Azure DB Cosmos funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a vazeb Azure Cosmos DB kódu v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Další informace o Cosmos DB najdete v tématu [Úvod do Cosmos DB](../documentdb/documentdb-introduction.md) a [vytvoření konzolové aplikace Cosmos DB](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>Rozhraní API DocumentDB vstupní vazby
Vstupní vazba rozhraní API DocumentDB Cosmos DB dokumentu načte a předává je pro pojmenované vstupní parametr funkce. Dokument, který se dá určit ID založené na aktivační událost, která volá funkci. 

Vstupní vazba rozhraní API DocumentDB má následující vlastnosti *function.json*:

- `name`: Název identifikátor použitý v kódu funkce pro dokument
- `type`: musí být nastavena na "documentdb"
- `databaseName`: Databáze obsahující dokumentu
- `collectionName`: Kolekce obsahující dokumentu
- `id`: Id dokumentu pro načtení. Tato vlastnost podporuje vazby parametrů; v tématu [vazby na vlastní vstupní vlastnosti ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) v článku [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).
- `sqlQuery`: Dotaz Cosmos DB SQL použitý k načtení více dokumentů. Dotaz podporuje runtime vazby. Příklad: `SELECT * FROM c where c.departmentId = {departmentId}`
- `connection`: Název nastavení aplikace obsahující připojovací řetězec databáze Cosmos
- `direction`: musí být nastavena na `"in"`.

Vlastnosti `id` a `sqlQuery` nelze zadat současně. Pokud ani `id` ani `sqlQuery` není nastaven, je-li načíst celou kolekci.

## <a name="using-a-documentdb-api-input-binding"></a>Použití rozhraní API DocumentDB vstupní vazby

* V jazyce C# a F # funkce při ukončení funkce úspěšně, některé změny provedené vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé. 
* V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce. Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` pro nastavení aktualizací. Najdete v článku [JavaScript ukázka](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Vstupní vzorek pro jednotlivý dokument
Předpokládejme, že máte následující rozhraní API DocumentDB vstupní vazby v `bindings` pole function.json:

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

V tématu vzorku pro specifický jazyk, který používá tuto vstupní vazbu k aktualizaci dokumentu textovou hodnotu.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Vstupní ukázka v jazyce F # #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Vstupní ukázka v jazyce JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Vstupní ukázkové s více dokumenty

Předpokládejme, které chcete načíst více dokumentů určeného dotaz SQL, chcete-li přizpůsobit parametry dotazu pomocí aktivační procedury fronty. 

V tomto příkladu obsahuje aktivační událost fronty parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro finančního oddělení. Použijte následující *function.json*:

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

### <a name="input-sample-with-multiple-documents-in-c"></a>Vstupní ukázkové s více dokumenty v jazyce C#

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

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Vstupní ukázkové s více dokumenty v jazyce JavaScript

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

## <a id="docdboutput"></a>Rozhraní API DocumentDB výstup vazby
Rozhraní API DocumentDB výstup vazby umožňuje zapsat nový dokument k databázi Azure Cosmos DB. Má následující vlastnosti v *function.json*:

- `name`: Identifikátor použitý v kódu funkce pro nový dokument
- `type`: musí být nastavený na`"documentdb"`
- `databaseName`: Databáze obsahující kolekci, kde bude vytvořen nový dokument.
- `collectionName`: Kolekce, kde bude vytvořen nový dokument.
- `createIfNotExists`: Logická hodnota označující, zda kolekce bude vytvořen, pokud neexistuje. Výchozí hodnota je *false*. Z důvodu pro toto je nové kolekce se vytvoří s vyhrazenou propustností, kterou se hradí. Další podrobnosti naleznete [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Název nastavení aplikace obsahující připojovací řetězec databáze Cosmos
- `direction`: musí být nastavený na`"out"`

## <a name="using-a-documentdb-api-output-binding"></a>Pomocí rozhraní API DocumentDB výstup vazby
V této části se dozvíte, jak používat rozhraní API DocumentDB výstupu vazby v kódu funkce.

Při zápisu do výstupního parametru ve vaší funkci ve výchozím nastavení je nový dokument vygenerované v databázi, se automaticky vytvářenému identifikátoru GUID jako ID dokumentu. Můžete zadat ID dokumentu výstup dokumentu zadáním `id` vlastnost JSON v výstupní parametr. 

>[!Note]  
>Pokud zadáte ID stávající dokument, získá přepsány nový dokument výstup. 

Výstup více dokumentů, můžete také vázat na `ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jedním z podporovaných typů.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Ukázka výstupu vazby DocumentDB rozhraní API
Předpokládejme, že máte následující rozhraní API DocumentDB výstup vazby v `bindings` pole function.json:

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

A máte vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

A chcete vytvořit Cosmos DB dokumenty ve formátu pro každý záznam:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

V tématu vzorku pro specifický jazyk, který používá tuto vazbu výstup k přidání dokumenty k vaší databázi.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

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

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

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

Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Ukázka výstupu v jazyce JavaScript

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
