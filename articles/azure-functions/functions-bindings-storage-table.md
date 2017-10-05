---
title: "Azure tabulce úložiště funkcí vazby | Microsoft Docs"
description: "Pochopit, jak používat Azure Storage vazby v Azure Functions."
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
ms.openlocfilehash: bb01be3ee044f60376e0c9c2de7b3dd34f3b7aca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a><span data-ttu-id="595c3-104">Vazby tabulky Azure funkce úložiště</span><span class="sxs-lookup"><span data-stu-id="595c3-104">Azure Functions Storage table bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="595c3-105">Tento článek vysvětluje postup konfigurace a vazby tabulky Azure Storage kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="595c3-105">This article explains how to configure and code Azure Storage table bindings in Azure Functions.</span></span> <span data-ttu-id="595c3-106">Azure Functions podporuje vstup a výstup vazby pro tabulky služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="595c3-106">Azure Functions supports input and output bindings for Azure Storage tables.</span></span>

<span data-ttu-id="595c3-107">Vazbu tabulky úložiště podporuje následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="595c3-107">The Storage table binding supports the following scenarios:</span></span>

* <span data-ttu-id="595c3-108">**Přečtěte si jeden řádek v C# nebo Node.js funkce** – nastavte `partitionKey` a `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="595c3-108">**Read a single row in a C# or Node.js function** - Set `partitionKey` and `rowKey`.</span></span> <span data-ttu-id="595c3-109">`filter` a `take` vlastnosti nejsou v tomto scénáři používají.</span><span class="sxs-lookup"><span data-stu-id="595c3-109">The `filter` and `take` properties are not used in this scenario.</span></span>
* <span data-ttu-id="595c3-110">**Přečtěte si více řádků ve funkci jazyka C#** – modul runtime Functions poskytuje `IQueryable<T>` objekt vázán k tabulce.</span><span class="sxs-lookup"><span data-stu-id="595c3-110">**Read multiple rows in a C# function** - The Functions runtime provides an `IQueryable<T>` object bound to the table.</span></span> <span data-ttu-id="595c3-111">Typ `T` musí být odvozeny od `TableEntity` nebo implementovat `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="595c3-111">Type `T` must derive from `TableEntity` or implement `ITableEntity`.</span></span> <span data-ttu-id="595c3-112">`partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nejsou použity v tomto scénáři, můžete pomocí `IQueryable` objekt, který chcete provést všechny potřebné filtrování.</span><span class="sxs-lookup"><span data-stu-id="595c3-112">The `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario; you can use the `IQueryable` object to do any filtering required.</span></span> 
* <span data-ttu-id="595c3-113">**Přečtěte si více řádků v uzlu funkce** – nastavte `filter` a `take` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="595c3-113">**Read multiple rows in a Node function** - Set the `filter` and `take` properties.</span></span> <span data-ttu-id="595c3-114">Není nastavený `partitionKey` nebo `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="595c3-114">Don't set `partitionKey` or `rowKey`.</span></span>
* <span data-ttu-id="595c3-115">**Zápis jeden nebo více řádků ve funkci jazyka C#** – modul runtime Functions poskytuje `ICollector<T>` nebo `IAsyncCollector<T>` vázán k tabulce, kde `T` Určuje schéma entity, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="595c3-115">**Write one or more rows in a C# function** - The Functions runtime provides an `ICollector<T>` or `IAsyncCollector<T>` bound to the table, where `T` specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="595c3-116">Obvykle zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="595c3-116">Typically, type `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="595c3-117">`partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nejsou v tomto scénáři používají.</span><span class="sxs-lookup"><span data-stu-id="595c3-117">The `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a><span data-ttu-id="595c3-118">Vazba vstupní tabulky úložiště</span><span class="sxs-lookup"><span data-stu-id="595c3-118">Storage table input binding</span></span>
<span data-ttu-id="595c3-119">Vazba vstupní tabulky Azure Storage umožňuje používat úložiště tabulku ve vaší funkci.</span><span class="sxs-lookup"><span data-stu-id="595c3-119">The Azure Storage table input binding enables you to use a storage table in your function.</span></span> 

<span data-ttu-id="595c3-120">Vstupní tabulka úložiště funkci používá těchto objektů JSON `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="595c3-120">The Storage table input to a function uses the following JSON objects in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="595c3-121">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="595c3-121">Note the following:</span></span> 

* <span data-ttu-id="595c3-122">Použití `partitionKey` a `rowKey` společně ke čtení jedné entity.</span><span class="sxs-lookup"><span data-stu-id="595c3-122">Use `partitionKey` and `rowKey` together to read a single entity.</span></span> <span data-ttu-id="595c3-123">Tyto vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="595c3-123">These properties are optional.</span></span> 
* <span data-ttu-id="595c3-124">`connection`musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="595c3-124">`connection` must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="595c3-125">Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="595c3-125">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="595c3-126">Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="595c3-126">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>  

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="595c3-127">Vstupní využití</span><span class="sxs-lookup"><span data-stu-id="595c3-127">Input usage</span></span>
<span data-ttu-id="595c3-128">V jazyce C# funkce, vytvoření vazby vstupní tabulka entity (nebo entity) pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="595c3-128">In C# functions, you bind to the input table entity (or entities) by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="595c3-129">Kde `T` je datový typ chcete deserializaci dat do, a `paramName` je název, který jste zadali v [vstupní vazby](#input).</span><span class="sxs-lookup"><span data-stu-id="595c3-129">Where `T` is the data type that you want to deserialize the data into, and `paramName` is the name you specified in the [input binding](#input).</span></span> <span data-ttu-id="595c3-130">V Node.js funkce získáte přístup vstupní tabulka entity (nebo entity) pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="595c3-130">In Node.js functions, you access the input table entity (or entities) using `context.bindings.<name>`.</span></span>

<span data-ttu-id="595c3-131">Ve funkcích Node.js nebo C# lze deserializovat vstupní data.</span><span class="sxs-lookup"><span data-stu-id="595c3-131">The input data can be deserialized in Node.js or C# functions.</span></span> <span data-ttu-id="595c3-132">Deserializovaná objekty mají `RowKey` a `PartitionKey` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="595c3-132">The deserialized objects have `RowKey` and `PartitionKey` properties.</span></span>

<span data-ttu-id="595c3-133">V jazyce C# funkce můžete také vytvořit vazbu na některý z následujících typů a Functions runtime se pokusí rekonstruovat data tabulky pomocí typu:</span><span class="sxs-lookup"><span data-stu-id="595c3-133">In C# functions, you can also bind to any of the following types, and the Functions runtime will attempt to deserialize the table data using that type:</span></span>

* <span data-ttu-id="595c3-134">Žádný typ, který implementuje`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="595c3-134">Any type that implements `ITableEntity`</span></span>
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="595c3-135">Vstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="595c3-135">Input sample</span></span>
<span data-ttu-id="595c3-136">By měl mít následující function.json, který používá aktivační procedury fronty načíst řádek jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="595c3-136">Supposed you have the following function.json, which uses a queue trigger to read a single table row.</span></span> <span data-ttu-id="595c3-137">Určuje formát JSON `PartitionKey`  
 `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="595c3-137">The JSON specifies `PartitionKey` 
`RowKey`.</span></span> <span data-ttu-id="595c3-138">`"rowKey": "{queueTrigger}"`Označuje, že klíč řádku pochází z řetězec zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="595c3-138">`"rowKey": "{queueTrigger}"` indicates that the row key comes from the queue message string.</span></span>

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

<span data-ttu-id="595c3-139">Naleznete v ukázce pro specifický jazyk, který čte entity jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="595c3-139">See the language-specific sample that reads a single table entity.</span></span>

* [<span data-ttu-id="595c3-140">C#</span><span class="sxs-lookup"><span data-stu-id="595c3-140">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="595c3-141">F#</span><span class="sxs-lookup"><span data-stu-id="595c3-141">F#</span></span>](#inputfsharp)
* [<span data-ttu-id="595c3-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="595c3-142">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="595c3-143">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="595c3-143">Input sample in C#</span></span> #
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

### <a name="input-sample-in-f"></a><span data-ttu-id="595c3-144">Vstupní ukázka v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="595c3-144">Input sample in F#</span></span> #
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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="595c3-145">Vstupní ukázka v Node.js</span><span class="sxs-lookup"><span data-stu-id="595c3-145">Input sample in Node.js</span></span>
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a><span data-ttu-id="595c3-146">Úložiště table výstup vazby</span><span class="sxs-lookup"><span data-stu-id="595c3-146">Storage table output binding</span></span>
<span data-ttu-id="595c3-147">Výstupní tabulky Azure Storage, vazba umožňuje zápis entity do úložiště tabulky ve vaší funkci.</span><span class="sxs-lookup"><span data-stu-id="595c3-147">The Azure Storage table output binding enables you to write entities to a Storage table in your function.</span></span> 

<span data-ttu-id="595c3-148">Výstup funkce používá následující objekty JSON v tabulce úložiště `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="595c3-148">The Storage table output for a function uses the following JSON objects in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="595c3-149">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="595c3-149">Note the following:</span></span> 

* <span data-ttu-id="595c3-150">Použití `partitionKey` a `rowKey` společně k zápisu jedné entity.</span><span class="sxs-lookup"><span data-stu-id="595c3-150">Use `partitionKey` and `rowKey` together to write a single entity.</span></span> <span data-ttu-id="595c3-151">Tyto vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="595c3-151">These properties are optional.</span></span> <span data-ttu-id="595c3-152">Můžete také zadat `PartitionKey` a `RowKey` při vytváření objektů entity ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="595c3-152">You can also specify `PartitionKey` and `RowKey` when you create the entity objects in your function code.</span></span>
* <span data-ttu-id="595c3-153">`connection`musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="595c3-153">`connection` must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="595c3-154">Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="595c3-154">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="595c3-155">Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="595c3-155">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span> 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="595c3-156">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="595c3-156">Output usage</span></span>
<span data-ttu-id="595c3-157">V jazyce C# funkce, je vytvoření vazby na výstupní tabulky pomocí pojmenované `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je datový typ chcete serializovat data do, a `paramName` je název, který jste zadali v [výstup vazby](#output).</span><span class="sxs-lookup"><span data-stu-id="595c3-157">In C# functions, you bind to the table output by using the named `out` parameter in your function signature, like `out <T> <name>`, where `T` is the data type that you want to serialize the data into, and `paramName` is the name you specified in the [output binding](#output).</span></span> <span data-ttu-id="595c3-158">Ve funkcích Node.js, můžete přístup k tabulce výstup pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="595c3-158">In Node.js functions, you access the table output using `context.bindings.<name>`.</span></span>

<span data-ttu-id="595c3-159">Může serializovat objekty v Node.js nebo C#.</span><span class="sxs-lookup"><span data-stu-id="595c3-159">You can serialize objects in Node.js or C# functions.</span></span> <span data-ttu-id="595c3-160">V C# funkce můžete také vytvořit vazbu na následující typy:</span><span class="sxs-lookup"><span data-stu-id="595c3-160">In C# functions, you can also bind to the following types:</span></span>

* <span data-ttu-id="595c3-161">Žádný typ, který implementuje`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="595c3-161">Any type that implements `ITableEntity`</span></span>
* <span data-ttu-id="595c3-162">`ICollector<T>`(pro výstup více entit.</span><span class="sxs-lookup"><span data-stu-id="595c3-162">`ICollector<T>` (to output multiple entities.</span></span> <span data-ttu-id="595c3-163">V tématu [ukázka](#outcsharp).)</span><span class="sxs-lookup"><span data-stu-id="595c3-163">See [sample](#outcsharp).)</span></span>
* <span data-ttu-id="595c3-164">`IAsyncCollector<T>`(asynchronní verzi `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="595c3-164">`IAsyncCollector<T>` (async version of `ICollector<T>`)</span></span>
* <span data-ttu-id="595c3-165">`CloudTable`(s použitím sady SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="595c3-165">`CloudTable` (using the Azure Storage SDK.</span></span> <span data-ttu-id="595c3-166">V tématu [ukázka](#readmulti).)</span><span class="sxs-lookup"><span data-stu-id="595c3-166">See [sample](#readmulti).)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="595c3-167">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="595c3-167">Output sample</span></span>
<span data-ttu-id="595c3-168">Následující *function.json* a *run.csx* příklad ukazuje, jak napsat více entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="595c3-168">The following *function.json* and *run.csx* example shows how to write multiple table entities.</span></span>

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

<span data-ttu-id="595c3-169">V tématu vzorku pro specifický jazyk, který vytvoří několik entit tabulky.</span><span class="sxs-lookup"><span data-stu-id="595c3-169">See the language-specific sample that creates multiple table entities.</span></span>

* [<span data-ttu-id="595c3-170">C#</span><span class="sxs-lookup"><span data-stu-id="595c3-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="595c3-171">F#</span><span class="sxs-lookup"><span data-stu-id="595c3-171">F#</span></span>](#outfsharp)
* [<span data-ttu-id="595c3-172">Node.js</span><span class="sxs-lookup"><span data-stu-id="595c3-172">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="595c3-173">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="595c3-173">Output sample in C#</span></span> #
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

### <a name="output-sample-in-f"></a><span data-ttu-id="595c3-174">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="595c3-174">Output sample in F#</span></span> #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="595c3-175">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="595c3-175">Output sample in Node.js</span></span>
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

## <a name="sample-read-multiple-table-entities-in-c"></a><span data-ttu-id="595c3-176">Ukázka: Přečtěte si více entity tabulky v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="595c3-176">Sample: Read multiple table entities in C#</span></span>  #
<span data-ttu-id="595c3-177">Následující *function.json* a příklad kódu C# přečte entity pro klíč oddílu, který je uveden v zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="595c3-177">The following *function.json* and C# code example reads entities for a partition key that is specified in the queue message.</span></span>

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

<span data-ttu-id="595c3-178">Kód jazyka C# přidá odkaz na sady SDK úložiště Azure tak, aby typ entity můžete odvozena od `TableEntity`.</span><span class="sxs-lookup"><span data-stu-id="595c3-178">The C# code adds a reference to the Azure Storage SDK so that the entity type can derive from `TableEntity`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="595c3-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="595c3-179">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

