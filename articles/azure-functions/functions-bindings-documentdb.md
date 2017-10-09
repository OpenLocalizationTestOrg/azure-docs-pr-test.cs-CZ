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
# <a name="azure-functions-cosmos-db-bindings"></a>Azure DB Cosmos funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak Azure Cosmos DB vazeb tooconfigure a kódu v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Další informace o Cosmos DB najdete v tématu [tooCosmos Úvod DB](../documentdb/documentdb-introduction.md) a [vytvoření konzolové aplikace Cosmos DB](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>Rozhraní API DocumentDB vstupní vazby
Hello Vstupní vazba rozhraní API DocumentDB Cosmos DB dokumentu načte a předává je toohello s názvem vstupní parametr funkce hello. Hello dokument, který se dá určit ID podle hello aktivační událost, která volá funkce hello. 

Hello Vstupní vazba rozhraní API DocumentDB má následující vlastnosti v hello *function.json*:

- `name`: Název identifikátor používaný v kódu funkce pro dokument hello
- `type`: musí být nastaven příliš "documentdb"
- `databaseName`: hello databáze obsahující hello dokumentu
- `collectionName`: hello kolekce obsahující hello dokumentu
- `id`: hello Id dokumentu tooretrieve hello. Tato vlastnost podporuje vazby parametrů; v tématu [vazbu vlastnosti vstupu toocustom ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) v článku hello [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).
- `sqlQuery`: Dotaz Cosmos DB SQL použitý k načtení více dokumentů. Hello query podporuje runtime vazby. Příklad: `SELECT * FROM c where c.departmentId = {departmentId}`
- `connection`: hello název nastavení aplikace hello obsahující připojovací řetězec databáze Cosmos
- `direction`: musí být nastaven příliš`"in"`.

Hello vlastnosti `id` a `sqlQuery` nelze zadat současně. Pokud ani `id` ani `sqlQuery` nastavena, hello celou kolekci se načítají.

## <a name="using-a-documentdb-api-input-binding"></a>Použití rozhraní API DocumentDB vstupní vazby

* V jazyce C# a F # funkce při ukončení hello funkce úspěšně, některé změny provedené v toohello vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé. 
* V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce. Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` toomake aktualizace. V tématu hello [JavaScript ukázka](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Vstupní vzorek pro jednotlivý dokument
Předpokládejme, že máte následující hello DocumentDB API vstupní vazby v hello `bindings` pole function.json:

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

V tématu vzorku hello konkrétní jazyk, který používá vstupní vazby tooupdate hello tohoto dokumentu textovou hodnotu.

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

Tato ukázka vyžaduje `project.json` soubor, který určuje hello `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

tooadd `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

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

Předpokládejme, že si přejete tooretrieve více dokumentů, zadaný v dotazu SQL pomocí parametry dotazu toocustomize hello frontě aktivační události. 

V tomto příkladu hello frontě aktivační události obsahuje parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro hello finančního oddělení. Použijte hello v *function.json*:

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
Hello DocumentDB API výstupu vazby umožňuje zapisovat novou databázi Azure Cosmos DB tooan dokumentu. Má následující vlastnosti v hello *function.json*:

- `name`: Identifikátor použitý v kódu funkce pro nový dokument hello
- `type`: musí být nastaven příliš`"documentdb"`
- `databaseName`: hello databáze obsahující hello kolekce, kde bude vytvořen nový dokument hello.
- `collectionName`: hello kolekce, kde bude vytvořen nový dokument hello.
- `createIfNotExists`: Logická hodnota. hodnota tooindicate, zda text hello kolekce se vytvoří, pokud neexistuje. Výchozí hodnota Hello je *false*. Hello důvod pro toto je nové kolekce se vytvoří s vyhrazenou propustností, kterou se hradí. Další podrobnosti naleznete na adrese hello [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: hello název nastavení aplikace hello obsahující připojovací řetězec databáze Cosmos
- `direction`: musí být nastaven příliš`"out"`

## <a name="using-a-documentdb-api-output-binding"></a>Pomocí rozhraní API DocumentDB výstup vazby
Tato část uvádí, jak toouse rozhraní API DocumentDB výstup vazby v kódu funkce.

Při psaní toohello výstupní parametr do funkce, ve výchozím nastavení do nového dokumentu se generuje ve vaší databázi, se automaticky vytvářenému identifikátoru GUID jako hello dokumentu ID. Můžete zadat ID dokumentu hello výstup dokumentu zadáním hello `id` vlastnost JSON v hello výstupní parametr. 

>[!Note]  
>Pokud zadáte ID hello stávající dokument, získá přepsány nový dokument výstup hello. 

toooutput více dokumentů, můžete také navázat příliš`ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jeden z typů hello podporována.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Ukázka výstupu vazby DocumentDB rozhraní API
Předpokládejme, že máte následující hello DocumentDB API výstup vazby v hello `bindings` pole function.json:

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

A máte vazbu vstupní fronty pro frontu, která přijímá JSON v hello následující formát:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

A chcete, aby toocreate Cosmos DB dokumenty ve formátu pro každý záznam hello:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

V tématu vzorku hello konkrétní jazyk, který používá tento výstup vazby tooadd dokumenty tooyour databázi.

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

Tato ukázka vyžaduje `project.json` soubor, který určuje hello `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

tooadd `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

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
