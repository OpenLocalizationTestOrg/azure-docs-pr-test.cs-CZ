---
title: "vazby funkce úložiště Table aaaAzure | Microsoft Docs"
description: Pochopit, jak toouse vazeb Azure Storage v Azure Functions.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 90c2a73329139d4ab3504bc0e2c90370133158bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Vazby tabulky Azure funkce úložiště
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak kód pro Azure Storage a tooconfigure tabulky vazeb v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro tabulky služby Azure Storage.

vazbu tabulky Hello úložiště podporuje hello následující scénáře:

* **Přečtěte si jeden řádek v C# nebo Node.js funkce** – nastavte `partitionKey` a `rowKey`. Hello `filter` a `take` vlastnosti nejsou v tomto scénáři používají.
* **Přečtěte si více řádků ve funkci jazyka C#** -poskytuje hello Functions runtime `IQueryable<T>` objekt vázán toohello tabulky. Typ `T` musí být odvozeny od `TableEntity` nebo implementovat `ITableEntity`. Hello `partitionKey`, `rowKey`, `filter`, a `take` vlastnosti nejsou použity v tomto scénáři, můžete pomocí hello `IQueryable` objektu toodo jakéhokoli filtrování vyžaduje. 
* **Přečtěte si více řádků v uzlu funkce** – nastavte hello `filter` a `take` vlastnosti. Není nastavený `partitionKey` nebo `rowKey`.
* **Zápis jeden nebo více řádků ve funkci jazyka C#** -poskytuje hello Functions runtime `ICollector<T>` nebo `IAsyncCollector<T>` vázané toohello tabulky, kde `T` Určuje schéma hello hello entit chcete tooadd. Obvykle zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to. Hello `partitionKey`, `rowKey`, `filter`, a `take` vlastnosti nejsou v tomto scénáři používají.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Vazba vstupní tabulky úložiště
Hello vazbu vstupní tabulky Azure Storage vám umožní toouse tabulky úložiště v funkce. 

Hello funkce tooa vstupní tabulka úložiště používá následující objekty JSON v hello hello `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity tooread - see below>",
    "rowKey": "<RowKey of table entity tooread - see below>",
    "take": "<Maximum number of entities tooread in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Vezměte na vědomí následující hello: 

* Použití `partitionKey` a `rowKey` společně tooread jedné entity. Tyto vlastnosti jsou volitelné. 
* `connection`musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště. V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících. Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
V jazyce C# funkce, vytvoření vazby toohello vstupní tabulka entity (nebo entity) pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v hello [vstupní vazby](#input). Ve funkcích Node.js přistupujete hello vstupní tabulka entity (nebo entity) pomocí `context.bindings.<name>`.

vstupní data Hello lze deserializovat v Node.js nebo C#. Hello deserializovat objekty mají `RowKey` a `PartitionKey` vlastnosti.

V jazyce C# funkce můžete také navázat tooany hello následující typy a funkce hello runtime pokusí příliš deserializaci dat v tabulce hello pomocí typu:

* Žádný typ, který implementuje`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Vstupní ukázka
By mělo mít hello následující function.json, který používá tooread aktivační události fronty řádek jednu tabulku. Určuje, Hello JSON `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Označuje, že tento klíč řádku hello pochází z řetězec zprávy fronty hello.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

V tématu hello konkrétní jazyk vzorku, který čte entity jednu tabulku.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Vstupní ukázka v jazyce F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Vstupní ukázka v Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Úložiště table výstup vazby
Vazba vám umožní toowrite entity tooa úložiště tabulky v funkce výstupu Hello tabulky Azure Storage. 

Hello výstupní tabulky úložiště pro funkci používá následující objekty JSON v hello hello `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity toowrite - see below>",
    "rowKey": "<RowKey of table entity toowrite - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Vezměte na vědomí následující hello: 

* Použití `partitionKey` a `rowKey` společně toowrite jedné entity. Tyto vlastnosti jsou volitelné. Můžete také zadat `PartitionKey` a `RowKey` při vytváření hello objekty entity ve vašem kódu funkce.
* `connection`musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště. V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících. Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V jazyce C# funkce, vazby toohello výstupní tabulky pomocí hello s názvem `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je hello datový typ, který má tooserialize hello data do, a `paramName` je hello název můžete zadaný v hello [výstup vazby](#output). V Node.js funkce, je přístup k tabulce hello výstup pomocí `context.bindings.<name>`.

Může serializovat objekty v Node.js nebo C#. V jazyce C# funkce můžete také navázat toohello následující typy:

* Žádný typ, který implementuje`ITableEntity`
* `ICollector<T>`(toooutput více entit. V tématu [ukázka](#outcsharp).)
* `IAsyncCollector<T>`(asynchronní verzi `ICollector<T>`)
* `CloudTable`(s použitím hello sada SDK úložiště Azure. V tématu [ukázka](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Následující Hello *function.json* a *run.csx* příklad ukazuje způsob toowrite více entity tabulky.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který vytvoří několik entit tabulky.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 too10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Ukázka: Přečtěte si více entity tabulky v jazyce C#  #
Následující Hello *function.json* a příklad kódu C# přečte entity pro klíč oddílu, který je uveden v uvítací zprávu fronty.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Hello C# – kód přidá reference toohello sada SDK úložiště Azure tak, aby typ entity hello můžete odvozen od `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

