---
title: "aaaWork s triggerů a vazeb v Azure Functions | Microsoft Docs"
description: "Zjistěte, jak toouse aktivuje a vazeb v Azure Functions tooconnect události tooonline spuštění kódu a cloudové služby."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: donnam
ms.openlocfilehash: eb2ebfca172fcc8c0f479adbcfec99e90fc33615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkce triggerů a vazeb koncepty
Azure Functions umožňuje toowrite kód odpovědi tooevents v Azure a dalším službám prostřednictvím *aktivační události* a *vazby*. Tento článek obsahuje přehled služby aktivačních událostí a vazby pro všechny podporované programovací jazyky. Funkce, které jsou společné tooall vazby jsou zde popsány.

## <a name="overview"></a>Přehled

Triggerů a vazeb jsou deklarativní způsob toodefine způsob volání funkce a jaká data funguje s. A *aktivační událost* definuje způsob volání funkce. Funkce musí mít přesně jeden aktivační události. Aktivační události mají související data, která je obvykle hello datové části, která aktivuje funkce hello. 

Vstup a výstup *vazby* zadejte toodata deklarativní způsob tooconnect z vašeho kódu. Podobně jako tootriggers zadáte připojovací řetězce a další vlastnosti v konfiguraci funkce. Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby. 

Pomocí triggerů a vazeb, můžete napsat kód, který je více obecné a nemá závislé hello podrobnosti hello služeb, se kterými komunikuje. Dat pocházejících z služby jednoduše stát vstupní hodnoty kódu funkce. Služba tooanother toooutput data (jako je vytvoření nového řádku v Azure Table Storage), použijte hello návratovou hodnotu metody hello. Nebo pokud potřebujete toooutput více hodnot, použijte pomocný objekt. Mít triggerů a vazeb **název** vlastnost, která je identifikátor použijete v vazbě hello tooaccess vašeho kódu.

Můžete nakonfigurovat triggerů a vazeb v hello **integrací** karta na portálu Azure Functions hello. V části hello zahrnuje hello uživatelského rozhraní upraví soubor s názvem *function.json* soubor v adresáři funkce hello. Tento soubor můžete upravit změnou toohello **pokročilé editor**.

Hello následující tabulka uvádí hello triggerů a vazeb, které jsou podporovány Azure Functions. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Příklad: Aktivace fronty a tabulky Výstupní vazby

Předpokládejme, že chcete toowrite nový řádek tooAzure Table Storage vždy, když v Azure Queue Storage se objeví nová zpráva. Tento scénář může být implementovaná pomocí Azure Queue aktivační události a tabulku výstupní vazby. 

Aktivační procedury fronty vyžaduje následující informace v hello hello **integrací** karty:

* Název nastavení aplikace hello, který obsahuje připojovací řetězec účet hello úložiště pro frontu hello Hello
* Název fronty Hello
* Hello identifikátor v váš kód tooread hello obsah uvítací zprávu fronty, jako například `order`.

toowrite tooAzure Table Storage, použít vazbu výstup hello následující podrobnosti:

* Název nastavení aplikace hello, který obsahuje připojovací řetězec účet hello úložiště pro tabulku hello Hello
* Název tabulky Hello
* identifikátor Hello v váš kód toocreate výstupní položky nebo hello návratovou hodnotou z funkce hello.

Vazby používat nastavení aplikace pro připojovací řetězce tooenforce hello nejlepší praxi, který *function.json* neobsahuje tajné klíče služby.

Poté použijte hello identifikátory, které jste zadali toointegrate s Azure Storage v kódu.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello second parameter toocontext.done is used as hello value for hello new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Tady je hello *function.json* odpovídající toohello předcházející kódu. Všimněte si, že hello, je možné použít stejnou konfiguraci, bez ohledu na jazyk hello implementace funkce hello.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
tooview a úpravy hello obsah *function.json* v hello portálu Azure, klikněte na tlačítko hello **pokročilé editor** možnost na hello **integrací** kartě funkce.

Další příklady kódu a informace o integraci s Azure Storage najdete v tématu [Azure Functions triggerů a vazeb pro Azure Storage](functions-bindings-storage.md).

### <a name="binding-direction"></a>Směr vazby

Mají všechny triggerů a vazeb `direction` vlastnost:

- Pro aktivační události je vždy směr hello`in`
- Vstupní a výstupní vazby používat `in` a`out`
- Některé vazby podporují speciální směr `inout`. Pokud používáte `inout`, pouze hello **pokročilé editor** je k dispozici v hello **integrací** kartě.

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a>Pomocí tooreturn návratový typ funkce hello jediného výstupu

Hello předchozí příklad ukazuje, jak toouse hello funkce návratovou hodnotu tooprovide výstup tooa vazby, což je dosaženo pomocí hello speciální název parametru `$return`. (Toto je jediná hodnota podporovaná v jazycích, které mají návratovou hodnotu, například C#, JavaScript a F #.) Pokud funkce má několik vazeb výstup, použijte `$return` pouze pro jeden z vazby výstup hello. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Příklady Hello níže ukazují jak návratové typy se používají s výstup vazeb v C#, JavaScript a F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```javascript
// JavaScript: return a value in hello second parameter toocontext.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Vlastnost dataType vazby

V rozhraní .NET použijte hello typy toodefine hello datový typ pro vstupní data. Například použijte `string` toobind toohello text aktivační procedury fronty a bajtové pole tooread jako binární.

Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte hello `dataType` vlastnost v definici vazby hello. Například tooread hello obsahu požadavku HTTP v binárním formátu, použijte typ hello `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Další možnosti pro `dataType` jsou `stream` a `string`.

## <a name="resolving-app-settings"></a>Řešení nastavení aplikace
Jako osvědčený postup tajné klíče a připojovací řetězce musí být řízen pomocí nastavení aplikace, nikoli konfigurační soubory. To omezuje přístup toothese tajných klíčů a umožňuje bezpečné toostore *function.json* v úložišti veřejné zdroj ovládacího prvku.

Nastavení aplikace jsou užitečné také vždy, když chcete toochange konfigurace na základě hello prostředí. Například v testovacím prostředí, můžete toomonitor jiný kontejner fronty nebo objekt blob úložiště.

Nastavení aplikace jsou vyřešeny vždy, když hodnota je uzavřena mezi znaky procenta, jako například `%MyAppSetting%`. Všimněte si, že hello `connection` vlastnost triggerů a vazeb je zvláštní případ a automaticky vyřeší hodnoty jako nastavení aplikace. 

Hello následující příklad je fronty aktivační událost, která používá nastavení aplikace `%input-queue-name%` toodefine hello fronty tootrigger na.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Metadata vlastnosti aktivační události

V přidání toohello datovou poskytované aktivační událost (například uvítací zprávu fronty, který aktivoval funkci) zadejte aktivační události mnoho dalších metadat hodnoty. Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti hello `context.bindings` objektu v jazyce JavaScript. 

Například aktivační procedury fronty podporuje hello následující vlastnosti:

* QueueTrigger - aktivován obsah zprávy, pokud platný řetězec
* DequeueCount
* ExpirationTime
* ID
* InsertionTime
* NextVisibleTime
* Vlastnosti PopReceipt

Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v hello odpovídající referenční téma. Dokumentace je také dostupná v hello **integrací** kartě hello portálu hello **dokumentace** části níže oblast konfigurace vazby hello.  

Například vzhledem k tomu, že aktivační události objektu blob mají některé zpoždění, můžete použít frontě aktivační události toorun funkce (viz [aktivační události objektu Blob úložiště](functions-bindings-storage-blob.md#storage-blob-trigger). uvítací zprávu fronty by obsahovat hello blob filename tootrigger na. Pomocí hello `queueTrigger` vlastnost metadat, toto chování můžete zadat všechny v konfiguraci, nikoli kódu.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Metadata vlastnosti aktivační události lze také v *vazby výraz* pro jiné vazbu jako následující části popisují hello.

## <a name="binding-expressions-and-patterns"></a>Výrazy vazba a vzory

Jedním z nejúčinnějších funkce hello triggerů a vazeb je *vazby výrazy*. V rámci vaší vazba, můžete definovat vzor výrazy, které lze poté použít v jiné vazby nebo kódu. Aktivační událost metadata mohou sloužit také v vazby výrazy, jak je vidět v ukázce hello v předcházející části hello.

Předpokládejme například, že chcete tooresize bitové kopie v kontejneru konkrétní objektu blob úložiště, podobně jako toohello **Úprava velikosti obrázku** šablony v hello **novou funkci** stránky. Přejděte příliš**novou funkci** -> jazyka **C#** -> scénář **ukázky** -> **ImageResizer CSharp**. 

Tady je hello *function.json* definice:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Všimněte si, že hello `filename` parametr se používá v definici objektu blob aktivace hello jak hello blob výstup vazby. Tento parametr můžete použít také v kódu funkce.

```csharp
// C# example of binding too{filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Náhodné identifikátory GUID
Azure Functions nabízí pohodlí syntaxe pro generování identifikátory GUID v vazby, prostřednictvím hello `{rand-guid}` vazby výraz. Hello následující příklad používá tento toogenerate název jedinečný objektů blob: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Aktuální čas

Můžete použít výraz vazby hello `DateTime`, který přeloží příliš`DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a>Vytvoření vazby vlastnosti vstupu toocustom ve výrazu vazby

Vazba výrazy můžete taky odkazovat vlastnosti, které jsou definovány v datové části hello aktivační událost, sám sebe. Například můžete soubor úložiště toodynamically vazby tooa objektů blob z uvedených v webhook, jehož název souboru.

Například hello následující *function.json* používá vlastnost s názvem `BlobName` z datové části hello aktivační události:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

tooaccomplish tento v C# a F #, je nutné definovat objektů POCO, definující hello pole, která bude deserializovat v datové části hello aktivační události.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

V jazyce JavaScript se provádí automaticky deserializaci JSON a hello vlastnosti můžete používat přímo.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Konfigurace vazba dat za běhu

V jazyce C# a jinými jazyky rozhraní .NET, můžete použít vzor imperativní vazby jako názvem na rozdíl od toohello deklarativní vazeb v *function.json*. Imperativní vazba je užitečné, když vázané parametry potřebovat toobe počítaný v době běhu spíše než návrhu. Další, najdete v části toolearn [vazby za běhu prostřednictvím imperativní vazby](functions-reference-csharp.md#imperative-bindings) v referenční informace pro vývojáře hello C#.

## <a name="next-steps"></a>Další kroky
Další informace o konkrétní vazbu najdete v části hello následující články:

- [HTTP a webhooky](functions-bindings-http-webhook.md)
- [Časovač](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum událostí](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Databáze Cosmos](functions-bindings-documentdb.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externí soubor](functions-bindings-external-file.md)
