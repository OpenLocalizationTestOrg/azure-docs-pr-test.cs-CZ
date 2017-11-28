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
# <a name="azure-functions-storage-table-bindings"></a><span data-ttu-id="0b9fd-104">Vazby tabulky Azure funkce úložiště</span><span class="sxs-lookup"><span data-stu-id="0b9fd-104">Azure Functions Storage table bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="0b9fd-105">Tento článek vysvětluje, jak kód pro Azure Storage a tooconfigure tabulky vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-105">This article explains how tooconfigure and code Azure Storage table bindings in Azure Functions.</span></span> <span data-ttu-id="0b9fd-106">Azure Functions podporuje vstup a výstup vazby pro tabulky služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-106">Azure Functions supports input and output bindings for Azure Storage tables.</span></span>

<span data-ttu-id="0b9fd-107">vazbu tabulky Hello úložiště podporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-107">hello Storage table binding supports hello following scenarios:</span></span>

* <span data-ttu-id="0b9fd-108">**Přečtěte si jeden řádek v C# nebo Node.js funkce** – nastavte `partitionKey` a `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-108">**Read a single row in a C# or Node.js function** - Set `partitionKey` and `rowKey`.</span></span> <span data-ttu-id="0b9fd-109">Hello `filter` a `take` vlastnosti nejsou v tomto scénáři používají.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-109">hello `filter` and `take` properties are not used in this scenario.</span></span>
* <span data-ttu-id="0b9fd-110">**Přečtěte si více řádků ve funkci jazyka C#** -poskytuje hello Functions runtime `IQueryable<T>` objekt vázán toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-110">**Read multiple rows in a C# function** - hello Functions runtime provides an `IQueryable<T>` object bound toohello table.</span></span> <span data-ttu-id="0b9fd-111">Typ `T` musí být odvozeny od `TableEntity` nebo implementovat `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-111">Type `T` must derive from `TableEntity` or implement `ITableEntity`.</span></span> <span data-ttu-id="0b9fd-112">Hello `partitionKey`, `rowKey`, `filter`, a `take` vlastnosti nejsou použity v tomto scénáři, můžete pomocí hello `IQueryable` objektu toodo jakéhokoli filtrování vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-112">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario; you can use hello `IQueryable` object toodo any filtering required.</span></span> 
* <span data-ttu-id="0b9fd-113">**Přečtěte si více řádků v uzlu funkce** – nastavte hello `filter` a `take` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-113">**Read multiple rows in a Node function** - Set hello `filter` and `take` properties.</span></span> <span data-ttu-id="0b9fd-114">Není nastavený `partitionKey` nebo `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-114">Don't set `partitionKey` or `rowKey`.</span></span>
* <span data-ttu-id="0b9fd-115">**Zápis jeden nebo více řádků ve funkci jazyka C#** -poskytuje hello Functions runtime `ICollector<T>` nebo `IAsyncCollector<T>` vázané toohello tabulky, kde `T` Určuje schéma hello hello entit chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-115">**Write one or more rows in a C# function** - hello Functions runtime provides an `ICollector<T>` or `IAsyncCollector<T>` bound toohello table, where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="0b9fd-116">Obvykle zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-116">Typically, type `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="0b9fd-117">Hello `partitionKey`, `rowKey`, `filter`, a `take` vlastnosti nejsou v tomto scénáři používají.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-117">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a><span data-ttu-id="0b9fd-118">Vazba vstupní tabulky úložiště</span><span class="sxs-lookup"><span data-stu-id="0b9fd-118">Storage table input binding</span></span>
<span data-ttu-id="0b9fd-119">Hello vazbu vstupní tabulky Azure Storage vám umožní toouse tabulky úložiště v funkce.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-119">hello Azure Storage table input binding enables you toouse a storage table in your function.</span></span> 

<span data-ttu-id="0b9fd-120">Hello funkce tooa vstupní tabulka úložiště používá následující objekty JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-120">hello Storage table input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="0b9fd-121">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-121">Note hello following:</span></span> 

* <span data-ttu-id="0b9fd-122">Použití `partitionKey` a `rowKey` společně tooread jedné entity.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-122">Use `partitionKey` and `rowKey` together tooread a single entity.</span></span> <span data-ttu-id="0b9fd-123">Tyto vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-123">These properties are optional.</span></span> 
* <span data-ttu-id="0b9fd-124">`connection`musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-124">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="0b9fd-125">V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-125">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="0b9fd-126">Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="0b9fd-126">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>  

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="0b9fd-127">Vstupní využití</span><span class="sxs-lookup"><span data-stu-id="0b9fd-127">Input usage</span></span>
<span data-ttu-id="0b9fd-128">V jazyce C# funkce, vytvoření vazby toohello vstupní tabulka entity (nebo entity) pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-128">In C# functions, you bind toohello input table entity (or entities) by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="0b9fd-129">Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v hello [vstupní vazby](#input).</span><span class="sxs-lookup"><span data-stu-id="0b9fd-129">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in hello [input binding](#input).</span></span> <span data-ttu-id="0b9fd-130">Ve funkcích Node.js přistupujete hello vstupní tabulka entity (nebo entity) pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-130">In Node.js functions, you access hello input table entity (or entities) using `context.bindings.<name>`.</span></span>

<span data-ttu-id="0b9fd-131">vstupní data Hello lze deserializovat v Node.js nebo C#.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-131">hello input data can be deserialized in Node.js or C# functions.</span></span> <span data-ttu-id="0b9fd-132">Hello deserializovat objekty mají `RowKey` a `PartitionKey` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-132">hello deserialized objects have `RowKey` and `PartitionKey` properties.</span></span>

<span data-ttu-id="0b9fd-133">V jazyce C# funkce můžete také navázat tooany hello následující typy a funkce hello runtime pokusí příliš deserializaci dat v tabulce hello pomocí typu:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-133">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime will attempt too deserialize hello table data using that type:</span></span>

* <span data-ttu-id="0b9fd-134">Žádný typ, který implementuje`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="0b9fd-134">Any type that implements `ITableEntity`</span></span>
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="0b9fd-135">Vstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="0b9fd-135">Input sample</span></span>
<span data-ttu-id="0b9fd-136">By mělo mít hello následující function.json, který používá tooread aktivační události fronty řádek jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-136">Supposed you have hello following function.json, which uses a queue trigger tooread a single table row.</span></span> <span data-ttu-id="0b9fd-137">Určuje, Hello JSON `PartitionKey`  
 `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-137">hello JSON specifies `PartitionKey` 
`RowKey`.</span></span> <span data-ttu-id="0b9fd-138">`"rowKey": "{queueTrigger}"`Označuje, že tento klíč řádku hello pochází z řetězec zprávy fronty hello.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-138">`"rowKey": "{queueTrigger}"` indicates that hello row key comes from hello queue message string.</span></span>

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

<span data-ttu-id="0b9fd-139">V tématu hello konkrétní jazyk vzorku, který čte entity jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-139">See hello language-specific sample that reads a single table entity.</span></span>

* [<span data-ttu-id="0b9fd-140">C#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-140">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="0b9fd-141">F#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-141">F#</span></span>](#inputfsharp)
* [<span data-ttu-id="0b9fd-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="0b9fd-142">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="0b9fd-143">Vstupní ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-143">Input sample in C#</span></span> #
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

### <a name="input-sample-in-f"></a><span data-ttu-id="0b9fd-144">Vstupní ukázka v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="0b9fd-144">Input sample in F#</span></span> #
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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="0b9fd-145">Vstupní ukázka v Node.js</span><span class="sxs-lookup"><span data-stu-id="0b9fd-145">Input sample in Node.js</span></span>
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a><span data-ttu-id="0b9fd-146">Úložiště table výstup vazby</span><span class="sxs-lookup"><span data-stu-id="0b9fd-146">Storage table output binding</span></span>
<span data-ttu-id="0b9fd-147">Vazba vám umožní toowrite entity tooa úložiště tabulky v funkce výstupu Hello tabulky Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-147">hello Azure Storage table output binding enables you toowrite entities tooa Storage table in your function.</span></span> 

<span data-ttu-id="0b9fd-148">Hello výstupní tabulky úložiště pro funkci používá následující objekty JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-148">hello Storage table output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="0b9fd-149">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-149">Note hello following:</span></span> 

* <span data-ttu-id="0b9fd-150">Použití `partitionKey` a `rowKey` společně toowrite jedné entity.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-150">Use `partitionKey` and `rowKey` together toowrite a single entity.</span></span> <span data-ttu-id="0b9fd-151">Tyto vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-151">These properties are optional.</span></span> <span data-ttu-id="0b9fd-152">Můžete také zadat `PartitionKey` a `RowKey` při vytváření hello objekty entity ve vašem kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-152">You can also specify `PartitionKey` and `RowKey` when you create hello entity objects in your function code.</span></span>
* <span data-ttu-id="0b9fd-153">`connection`musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-153">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="0b9fd-154">V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace, můžete při vytváření úložiště účet nebo vybere některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-154">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="0b9fd-155">Můžete také [konfigurace této aplikace, nastavení ručně](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="0b9fd-155">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span> 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="0b9fd-156">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="0b9fd-156">Output usage</span></span>
<span data-ttu-id="0b9fd-157">V jazyce C# funkce, vazby toohello výstupní tabulky pomocí hello s názvem `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je hello datový typ, který má tooserialize hello data do, a `paramName` je hello název můžete zadaný v hello [výstup vazby](#output).</span><span class="sxs-lookup"><span data-stu-id="0b9fd-157">In C# functions, you bind toohello table output by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in hello [output binding](#output).</span></span> <span data-ttu-id="0b9fd-158">V Node.js funkce, je přístup k tabulce hello výstup pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-158">In Node.js functions, you access hello table output using `context.bindings.<name>`.</span></span>

<span data-ttu-id="0b9fd-159">Může serializovat objekty v Node.js nebo C#.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-159">You can serialize objects in Node.js or C# functions.</span></span> <span data-ttu-id="0b9fd-160">V jazyce C# funkce můžete také navázat toohello následující typy:</span><span class="sxs-lookup"><span data-stu-id="0b9fd-160">In C# functions, you can also bind toohello following types:</span></span>

* <span data-ttu-id="0b9fd-161">Žádný typ, který implementuje`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="0b9fd-161">Any type that implements `ITableEntity`</span></span>
* <span data-ttu-id="0b9fd-162">`ICollector<T>`(toooutput více entit.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-162">`ICollector<T>` (toooutput multiple entities.</span></span> <span data-ttu-id="0b9fd-163">V tématu [ukázka](#outcsharp).)</span><span class="sxs-lookup"><span data-stu-id="0b9fd-163">See [sample](#outcsharp).)</span></span>
* <span data-ttu-id="0b9fd-164">`IAsyncCollector<T>`(asynchronní verzi `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="0b9fd-164">`IAsyncCollector<T>` (async version of `ICollector<T>`)</span></span>
* <span data-ttu-id="0b9fd-165">`CloudTable`(s použitím hello sada SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-165">`CloudTable` (using hello Azure Storage SDK.</span></span> <span data-ttu-id="0b9fd-166">V tématu [ukázka](#readmulti).)</span><span class="sxs-lookup"><span data-stu-id="0b9fd-166">See [sample](#readmulti).)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="0b9fd-167">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="0b9fd-167">Output sample</span></span>
<span data-ttu-id="0b9fd-168">Následující Hello *function.json* a *run.csx* příklad ukazuje způsob toowrite více entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-168">hello following *function.json* and *run.csx* example shows how toowrite multiple table entities.</span></span>

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

<span data-ttu-id="0b9fd-169">V tématu vzorku hello konkrétní jazyk, který vytvoří několik entit tabulky.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-169">See hello language-specific sample that creates multiple table entities.</span></span>

* [<span data-ttu-id="0b9fd-170">C#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="0b9fd-171">F#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-171">F#</span></span>](#outfsharp)
* [<span data-ttu-id="0b9fd-172">Node.js</span><span class="sxs-lookup"><span data-stu-id="0b9fd-172">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="0b9fd-173">Ukázka výstupu v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-173">Output sample in C#</span></span> #
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

### <a name="output-sample-in-f"></a><span data-ttu-id="0b9fd-174">Ukázka výstupu v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="0b9fd-174">Output sample in F#</span></span> #
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

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="0b9fd-175">Ukázka výstupu v Node.js</span><span class="sxs-lookup"><span data-stu-id="0b9fd-175">Output sample in Node.js</span></span>
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

## <a name="sample-read-multiple-table-entities-in-c"></a><span data-ttu-id="0b9fd-176">Ukázka: Přečtěte si více entity tabulky v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="0b9fd-176">Sample: Read multiple table entities in C#</span></span>  #
<span data-ttu-id="0b9fd-177">Následující Hello *function.json* a příklad kódu C# přečte entity pro klíč oddílu, který je uveden v uvítací zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-177">hello following *function.json* and C# code example reads entities for a partition key that is specified in hello queue message.</span></span>

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

<span data-ttu-id="0b9fd-178">Hello C# – kód přidá reference toohello sada SDK úložiště Azure tak, aby typ entity hello můžete odvozen od `TableEntity`.</span><span class="sxs-lookup"><span data-stu-id="0b9fd-178">hello C# code adds a reference toohello Azure Storage SDK so that hello entity type can derive from `TableEntity`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0b9fd-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b9fd-179">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

