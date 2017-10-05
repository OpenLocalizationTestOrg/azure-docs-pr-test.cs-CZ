---
title: "Práce s triggerů a vazeb v Azure Functions | Microsoft Docs"
description: "Další informace o použití triggerů a vazeb v Azure Functions k připojení vašeho provádění kódu k online události a cloudové služby."
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
ms.openlocfilehash: cc41debb2523df77be4db05817a4c7ac55604439
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="a9258-104">Azure funkce triggerů a vazeb koncepty</span><span class="sxs-lookup"><span data-stu-id="a9258-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="a9258-105">Azure Functions umožňuje psaní kódu v reakci na události v Azure a dalším službám prostřednictvím *aktivační události* a *vazby*.</span><span class="sxs-lookup"><span data-stu-id="a9258-105">Azure Functions allows you to write code in response to events in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="a9258-106">Tento článek obsahuje přehled služby aktivačních událostí a vazby pro všechny podporované programovací jazyky.</span><span class="sxs-lookup"><span data-stu-id="a9258-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="a9258-107">Tady jsou popsané funkce, které jsou společné pro všechny vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-107">Features that are common to all bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="a9258-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="a9258-108">Overview</span></span>

<span data-ttu-id="a9258-109">Triggerů a vazeb jsou deklarativní způsob, jak definovat, jak je volána funkce a co data funguje s.</span><span class="sxs-lookup"><span data-stu-id="a9258-109">Triggers and bindings are a declarative way to define how a function is invoked and what data it works with.</span></span> <span data-ttu-id="a9258-110">A *aktivační událost* definuje způsob volání funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="a9258-111">Funkce musí mít přesně jeden aktivační události.</span><span class="sxs-lookup"><span data-stu-id="a9258-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="a9258-112">Aktivační události mají související data, která je obvykle datové části, která aktivuje funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-112">Triggers have associated data, which is usually the payload that triggered the function.</span></span> 

<span data-ttu-id="a9258-113">Vstup a výstup *vazby* poskytnout deklarativní způsob, jak se připojit k datům z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="a9258-113">Input and output *bindings* provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="a9258-114">Podobně jako u aktivačních událostí, je zadat připojovací řetězce a další vlastnosti v konfiguraci funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-114">Similar to triggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="a9258-115">Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="a9258-116">Pomocí triggerů a vazeb, můžete napsat kód, který je instalace a další obecná závislé podrobnosti o službách, pomocí které komunikuje.</span><span class="sxs-lookup"><span data-stu-id="a9258-116">Using triggers and bindings, you can write code that is more generic and does not hardcode the details of the services with which it interacts.</span></span> <span data-ttu-id="a9258-117">Dat pocházejících z služby jednoduše stát vstupní hodnoty kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="a9258-118">Chcete-li výstupní data do jiné služby (jako je vytvoření nového řádku v Azure Table Storage), použijte návratovou hodnotu metody.</span><span class="sxs-lookup"><span data-stu-id="a9258-118">To output data to another service (such as creating a new row in Azure Table Storage), use the return value of the method.</span></span> <span data-ttu-id="a9258-119">Nebo pokud potřebujete výstup více hodnot, použijte pomocný objekt.</span><span class="sxs-lookup"><span data-stu-id="a9258-119">Or, if you need to output multiple values, use a helper object.</span></span> <span data-ttu-id="a9258-120">Mít triggerů a vazeb **název** vlastnost, která je identifikátor používáte ve vašem kódu pro přístup k vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-120">Triggers and bindings have a **name** property, which is an identifier you use in your code to access the binding.</span></span>

<span data-ttu-id="a9258-121">Můžete nakonfigurovat triggerů a vazeb v **integrací** na portálu Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="a9258-121">You can configure triggers and bindings in the **Integrate** tab in the Azure Functions portal.</span></span> <span data-ttu-id="a9258-122">V pozadí, uživatelské rozhraní upraví soubor s názvem *function.json* soubor v adresáři funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-122">Under the covers, the UI modifies a file called *function.json* file in the function directory.</span></span> <span data-ttu-id="a9258-123">Tento soubor můžete upravit změnou na **pokročilé editor**.</span><span class="sxs-lookup"><span data-stu-id="a9258-123">You can edit this file by changing to the **Advanced editor**.</span></span>

<span data-ttu-id="a9258-124">V následující tabulce jsou uvedeny triggerů a vazeb, které jsou podporovány Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="a9258-124">The following table shows the triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="a9258-125">Příklad: Aktivace fronty a tabulky Výstupní vazby</span><span class="sxs-lookup"><span data-stu-id="a9258-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="a9258-126">Předpokládejme, že chcete k zápisu nového řádku do Azure Table Storage, vždy, když v Azure Queue Storage se objeví nová zpráva.</span><span class="sxs-lookup"><span data-stu-id="a9258-126">Suppose you want to write a new row to Azure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="a9258-127">Tento scénář může být implementovaná pomocí Azure Queue aktivační události a tabulku výstupní vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="a9258-128">Aktivační procedury fronty vyžaduje následující informace v **integrací** karty:</span><span class="sxs-lookup"><span data-stu-id="a9258-128">A queue trigger requires the following information in the **Integrate** tab:</span></span>

* <span data-ttu-id="a9258-129">Název nastavení aplikace, který obsahuje připojovací řetězec pro účet úložiště pro frontu</span><span class="sxs-lookup"><span data-stu-id="a9258-129">The name of the app setting that contains the storage account connection string for the queue</span></span>
* <span data-ttu-id="a9258-130">Název fronty</span><span class="sxs-lookup"><span data-stu-id="a9258-130">The queue name</span></span>
* <span data-ttu-id="a9258-131">Identifikátor ve vašem kódu číst obsah zprávy ve frontě, jako například `order`.</span><span class="sxs-lookup"><span data-stu-id="a9258-131">The identifier in your code to read the contents of the queue message, such as `order`.</span></span>

<span data-ttu-id="a9258-132">Zapsat do Azure Table Storage, použijte vazbu výstup s následujícími podrobnostmi:</span><span class="sxs-lookup"><span data-stu-id="a9258-132">To write to Azure Table Storage, use an output binding with the following details:</span></span>

* <span data-ttu-id="a9258-133">Název nastavení aplikace, který obsahuje připojovací řetězec pro účet úložiště pro tabulku</span><span class="sxs-lookup"><span data-stu-id="a9258-133">The name of the app setting that contains the storage account connection string for the table</span></span>
* <span data-ttu-id="a9258-134">Název tabulky</span><span class="sxs-lookup"><span data-stu-id="a9258-134">The table name</span></span>
* <span data-ttu-id="a9258-135">Identifikátor ve vašem kódu k vytvoření výstupní položky nebo návratovou hodnotou z funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-135">The identifier in your code to create output items, or the return value from the function.</span></span>

<span data-ttu-id="a9258-136">Vazby nastavení pomocí aplikace pro připojovací řetězce k vynucení nejlepší praxi, který *function.json* neobsahuje tajné klíče služby.</span><span class="sxs-lookup"><span data-stu-id="a9258-136">Bindings use app settings for connection strings to enforce the best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="a9258-137">Potom pomocí identifikátorů, které jste zadali pro integraci s Azure Storage v kódu.</span><span class="sxs-lookup"><span data-stu-id="a9258-137">Then, use the identifiers you provided to integrate with Azure Storage in your code.</span></span>

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
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
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
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

<span data-ttu-id="a9258-138">Tady je *function.json* odpovídající předchozí kód.</span><span class="sxs-lookup"><span data-stu-id="a9258-138">Here is the *function.json* that corresponds to the preceding code.</span></span> <span data-ttu-id="a9258-139">Všimněte si, že stejnou konfiguraci můžete použít, bez ohledu na jazyk implementace funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-139">Note that the same configuration can be used, regardless of the language of the function implementation.</span></span>

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
<span data-ttu-id="a9258-140">K zobrazení a úprava obsahu *function.json* na portálu Azure klikněte na tlačítko **pokročilé editor** možnost **integrací** kartě funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-140">To view and edit the contents of *function.json* in the Azure portal, click the **Advanced editor** option on the **Integrate** tab of your function.</span></span>

<span data-ttu-id="a9258-141">Další příklady kódu a informace o integraci s Azure Storage najdete v tématu [Azure Functions triggerů a vazeb pro Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="a9258-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="a9258-142">Směr vazby</span><span class="sxs-lookup"><span data-stu-id="a9258-142">Binding direction</span></span>

<span data-ttu-id="a9258-143">Mají všechny triggerů a vazeb `direction` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="a9258-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="a9258-144">Pro aktivační události směr je vždy`in`</span><span class="sxs-lookup"><span data-stu-id="a9258-144">For triggers, the direction is always `in`</span></span>
- <span data-ttu-id="a9258-145">Vstupní a výstupní vazby používat `in` a`out`</span><span class="sxs-lookup"><span data-stu-id="a9258-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="a9258-146">Některé vazby podporují speciální směr `inout`.</span><span class="sxs-lookup"><span data-stu-id="a9258-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="a9258-147">Pokud používáte `inout`, jenom **pokročilé editor** je k dispozici v **integrací** kartě.</span><span class="sxs-lookup"><span data-stu-id="a9258-147">If you use `inout`, only the **Advanced editor** is available in the **Integrate** tab.</span></span>

## <a name="using-the-function-return-type-to-return-a-single-output"></a><span data-ttu-id="a9258-148">Použití návratový typ funkce pro vrácení jediného výstupu</span><span class="sxs-lookup"><span data-stu-id="a9258-148">Using the function return type to return a single output</span></span>

<span data-ttu-id="a9258-149">Předchozí příklad ukazuje, jak používat funkce návratovou hodnotu k poskytování výstup vazbu, což je dosaženo pomocí parametru speciální název `$return`.</span><span class="sxs-lookup"><span data-stu-id="a9258-149">The preceding example shows how to use the function return value to provide output to a binding, which is achieved by using the special name parameter `$return`.</span></span> <span data-ttu-id="a9258-150">(Toto je jediná hodnota podporovaná v jazycích, které mají návratovou hodnotu, například C#, JavaScript a F #.) Pokud funkce má několik vazeb výstup, použijte `$return` pouze pro jeden z výstupu vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of the output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="a9258-151">Příklady níže ukazují jak návratové typy se používají s výstup vazeb v C#, JavaScript a F #.</span><span class="sxs-lookup"><span data-stu-id="a9258-151">The examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

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
// JavaScript: return a value in the second parameter to context.done
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

## <a name="binding-datatype-property"></a><span data-ttu-id="a9258-152">Vlastnost dataType vazby</span><span class="sxs-lookup"><span data-stu-id="a9258-152">Binding dataType property</span></span>

<span data-ttu-id="a9258-153">V rozhraní .NET použijte k definování datový typ pro vstupní data typy.</span><span class="sxs-lookup"><span data-stu-id="a9258-153">In .NET, use the types to define the data type for input data.</span></span> <span data-ttu-id="a9258-154">Například použijte `string` k vytvoření vazby na text aktivační procedury fronty a bajtové pole přečíst jako binární.</span><span class="sxs-lookup"><span data-stu-id="a9258-154">For instance, use `string` to bind to the text of a queue trigger and a byte array to read as binary.</span></span>

<span data-ttu-id="a9258-155">Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte `dataType` vlastnost v definici vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-155">For languages that are dynamically typed such as JavaScript, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="a9258-156">Například pokud chcete číst obsah požadavku HTTP v binárním formátu, použijte typ `binary`:</span><span class="sxs-lookup"><span data-stu-id="a9258-156">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="a9258-157">Další možnosti pro `dataType` jsou `stream` a `string`.</span><span class="sxs-lookup"><span data-stu-id="a9258-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="a9258-158">Řešení nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="a9258-158">Resolving app settings</span></span>
<span data-ttu-id="a9258-159">Jako osvědčený postup tajné klíče a připojovací řetězce musí být řízen pomocí nastavení aplikace, nikoli konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="a9258-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="a9258-160">To omezuje přístup na těchto tajných klíčů a umožňuje bezpečné uložení *function.json* v úložišti veřejné zdroj ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a9258-160">This limits access to these secrets and makes it safe to store *function.json* in a public source control repository.</span></span>

<span data-ttu-id="a9258-161">Nastavení aplikace jsou užitečné také vždy, když chcete změnit konfiguraci na základě prostředí.</span><span class="sxs-lookup"><span data-stu-id="a9258-161">App settings are also useful whenever you want to change configuration based on the environment.</span></span> <span data-ttu-id="a9258-162">Například v testovacím prostředí, můžete monitorovat jiný kontejner fronty nebo objekt blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="a9258-162">For example, in a test environment, you may want to monitor a different queue or blob storage container.</span></span>

<span data-ttu-id="a9258-163">Nastavení aplikace jsou vyřešeny vždy, když hodnota je uzavřena mezi znaky procenta, jako například `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="a9258-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="a9258-164">Všimněte si, že `connection` vlastnost triggerů a vazeb je zvláštní případ a automaticky vyřeší hodnoty jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9258-164">Note that the `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="a9258-165">Následující příklad je fronty aktivační událost, která používá nastavení aplikace `%input-queue-name%` k definování fronty k aktivaci na.</span><span class="sxs-lookup"><span data-stu-id="a9258-165">The following example is a queue trigger that uses an app setting `%input-queue-name%` to define the queue to trigger on.</span></span>

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

## <a name="trigger-metadata-properties"></a><span data-ttu-id="a9258-166">Metadata vlastnosti aktivační události</span><span class="sxs-lookup"><span data-stu-id="a9258-166">Trigger metadata properties</span></span>

<span data-ttu-id="a9258-167">Kromě datová poskytované aktivační událost (například zprávy ve frontě, která aktivuje funkci) zadejte mnoho aktivační události hodnoty dalších metadat.</span><span class="sxs-lookup"><span data-stu-id="a9258-167">In addition to the data payload provided by a trigger (such as the queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="a9258-168">Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti na `context.bindings` objektu v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a9258-168">These values can be used as input parameters in C# and F# or properties on the `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="a9258-169">Například aktivační procedury fronty podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a9258-169">For example, a queue trigger supports the following properties:</span></span>

* <span data-ttu-id="a9258-170">QueueTrigger - aktivován obsah zprávy, pokud platný řetězec</span><span class="sxs-lookup"><span data-stu-id="a9258-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="a9258-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="a9258-171">DequeueCount</span></span>
* <span data-ttu-id="a9258-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="a9258-172">ExpirationTime</span></span>
* <span data-ttu-id="a9258-173">ID</span><span class="sxs-lookup"><span data-stu-id="a9258-173">Id</span></span>
* <span data-ttu-id="a9258-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="a9258-174">InsertionTime</span></span>
* <span data-ttu-id="a9258-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="a9258-175">NextVisibleTime</span></span>
* <span data-ttu-id="a9258-176">Vlastnosti PopReceipt</span><span class="sxs-lookup"><span data-stu-id="a9258-176">PopReceipt</span></span>

<span data-ttu-id="a9258-177">Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v odpovídajícího tématu.</span><span class="sxs-lookup"><span data-stu-id="a9258-177">Details of metadata properties for each trigger are described in the corresponding reference topic.</span></span> <span data-ttu-id="a9258-178">Je také dostupná v dokumentaci **integrací** karta portálu v **dokumentace** části níže oblast konfigurace vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-178">Documentation is also available in the **Integrate** tab of the portal, in the **Documentation** section below the binding configuration area.</span></span>  

<span data-ttu-id="a9258-179">Například vzhledem k tomu, že aktivační události objektu blob mají některé zpoždění, můžete použít aktivační procedury fronty ke spuštění funkce (viz [aktivační událost úložiště objektů Blob](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="a9258-179">For example, since blob triggers have some delays, you can use a queue trigger to run your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="a9258-180">Zprávy ve frontě by obsahovat název souboru objektů blob k aktivaci na.</span><span class="sxs-lookup"><span data-stu-id="a9258-180">The queue message would contain the blob filename to trigger on.</span></span> <span data-ttu-id="a9258-181">Pomocí `queueTrigger` vlastnost metadat, toto chování můžete zadat všechny v konfiguraci, nikoli kódu.</span><span class="sxs-lookup"><span data-stu-id="a9258-181">Using the `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

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

<span data-ttu-id="a9258-182">Metadata vlastnosti aktivační události lze také v *vazby výraz* pro jiné vazbu, jak je popsáno v následující části.</span><span class="sxs-lookup"><span data-stu-id="a9258-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in the following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="a9258-183">Výrazy vazba a vzory</span><span class="sxs-lookup"><span data-stu-id="a9258-183">Binding expressions and patterns</span></span>

<span data-ttu-id="a9258-184">Jedním z nejúčinnějších funkce triggerů a vazeb je *vazby výrazy*.</span><span class="sxs-lookup"><span data-stu-id="a9258-184">One of the most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="a9258-185">V rámci vaší vazba, můžete definovat vzor výrazy, které lze poté použít v jiné vazby nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="a9258-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="a9258-186">Aktivační událost metadata mohou sloužit také v vazby výrazy, jak je vidět v ukázce v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="a9258-186">Trigger metadata can also be used in binding expressions, as show in the sample in the preceding section.</span></span>

<span data-ttu-id="a9258-187">Předpokládejme například, kterou chcete změnit velikost bitové kopie v kontejneru konkrétní objektu blob úložiště, podobně jako **Úprava velikosti obrázku** šablony v **novou funkci** stránky.</span><span class="sxs-lookup"><span data-stu-id="a9258-187">For example, suppose you want to resize images in particular blob storage container, similar to the **Image Resizer** template in the **New Function** page.</span></span> <span data-ttu-id="a9258-188">Přejděte na **novou funkci** -> jazyka **C#** -> scénář **ukázky** -> **ImageResizer CSharp**.</span><span class="sxs-lookup"><span data-stu-id="a9258-188">Go to **New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="a9258-189">Tady je *function.json* definice:</span><span class="sxs-lookup"><span data-stu-id="a9258-189">Here is the *function.json* definition:</span></span>

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

<span data-ttu-id="a9258-190">Všimněte si, že `filename` parametr se používá v definici aktivační události objektu blob jak objektu blob výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="a9258-190">Notice that the `filename` parameter is used in both the blob trigger definition as well as the blob output binding.</span></span> <span data-ttu-id="a9258-191">Tento parametr můžete použít také v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="a9258-191">This parameter can also be used in function code.</span></span>

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a><span data-ttu-id="a9258-192">Náhodné identifikátory GUID</span><span class="sxs-lookup"><span data-stu-id="a9258-192">Random GUIDs</span></span>
<span data-ttu-id="a9258-193">Azure Functions nabízí pohodlí syntaxe pro generování identifikátory GUID v vazby, prostřednictvím `{rand-guid}` vazby výraz.</span><span class="sxs-lookup"><span data-stu-id="a9258-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through the `{rand-guid}` binding expression.</span></span> <span data-ttu-id="a9258-194">Následující příklad používá ke generování objektů blob jedinečný název toto:</span><span class="sxs-lookup"><span data-stu-id="a9258-194">The following example uses this to generate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="a9258-195">Aktuální čas</span><span class="sxs-lookup"><span data-stu-id="a9258-195">Current time</span></span>

<span data-ttu-id="a9258-196">Můžete použít výraz vazby `DateTime`, který přeloží na `DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="a9258-196">You can use the binding expression `DateTime`, which resolves to `DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a><span data-ttu-id="a9258-197">Vytvoření vazby na vlastní vstupní vlastnosti ve výrazu vazby</span><span class="sxs-lookup"><span data-stu-id="a9258-197">Bind to custom input properties in a binding expression</span></span>

<span data-ttu-id="a9258-198">Vazba výrazy můžete taky odkazovat vlastnosti, které jsou definovány v datové části aktivační událost sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a9258-198">Binding expressions can also reference properties that are defined in the trigger payload itself.</span></span> <span data-ttu-id="a9258-199">Například můžete dynamicky vázat na soubor úložiště objektů blob ze součástí webhook, jehož název souboru.</span><span class="sxs-lookup"><span data-stu-id="a9258-199">For example, you may want to dynamically bind to a blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="a9258-200">Například následující *function.json* používá vlastnost s názvem `BlobName` z datové části aktivační události:</span><span class="sxs-lookup"><span data-stu-id="a9258-200">For example, the following *function.json* uses a property called `BlobName` from the trigger payload:</span></span>

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

<span data-ttu-id="a9258-201">K tomu v C# a F #, je nutné zadat objektů POCO, která definuje pole, která bude deserializovat v datové části aktivační události.</span><span class="sxs-lookup"><span data-stu-id="a9258-201">To accomplish this in C# and F#, you must define a POCO that defines the fields that will be deserialized in the trigger payload.</span></span>

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

<span data-ttu-id="a9258-202">V jazyce JavaScript se provádí automaticky deserializaci JSON a vlastností lze používat přímo.</span><span class="sxs-lookup"><span data-stu-id="a9258-202">In JavaScript, JSON deserialization is automatically performed and you can use the properties directly.</span></span>

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

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="a9258-203">Konfigurace vazba dat za běhu</span><span class="sxs-lookup"><span data-stu-id="a9258-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="a9258-204">V jazyce C# a jinými jazyky rozhraní .NET, můžete použít imperativní vazby vzoru oproti deklarativní vazeb v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="a9258-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed to the declarative bindings in *function.json*.</span></span> <span data-ttu-id="a9258-205">Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu.</span><span class="sxs-lookup"><span data-stu-id="a9258-205">Imperative binding is useful when binding parameters need to be computed at runtime rather than design time.</span></span> <span data-ttu-id="a9258-206">Další informace najdete v tématu [vazby za běhu prostřednictvím imperativní vazby](functions-reference-csharp.md#imperative-bindings) v referenci vývojáře jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="a9258-206">To learn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in the C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9258-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9258-207">Next steps</span></span>
<span data-ttu-id="a9258-208">Další informace o konkrétní vazbu najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="a9258-208">For more information on a specific binding, see the following articles:</span></span>

- [<span data-ttu-id="a9258-209">HTTP a webhooky</span><span class="sxs-lookup"><span data-stu-id="a9258-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="a9258-210">Časovač</span><span class="sxs-lookup"><span data-stu-id="a9258-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="a9258-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="a9258-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="a9258-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="a9258-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="a9258-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="a9258-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="a9258-214">Centrum událostí</span><span class="sxs-lookup"><span data-stu-id="a9258-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="a9258-215">Service Bus</span><span class="sxs-lookup"><span data-stu-id="a9258-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="a9258-216">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="a9258-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="a9258-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="a9258-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="a9258-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="a9258-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="a9258-219">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="a9258-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="a9258-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="a9258-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="a9258-221">Externí soubor</span><span class="sxs-lookup"><span data-stu-id="a9258-221">External file</span></span>](functions-bindings-external-file.md)
