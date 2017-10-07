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
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="c09c2-104">Azure funkce triggerů a vazeb koncepty</span><span class="sxs-lookup"><span data-stu-id="c09c2-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="c09c2-105">Azure Functions umožňuje toowrite kód odpovědi tooevents v Azure a dalším službám prostřednictvím *aktivační události* a *vazby*.</span><span class="sxs-lookup"><span data-stu-id="c09c2-105">Azure Functions allows you toowrite code in response tooevents in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="c09c2-106">Tento článek obsahuje přehled služby aktivačních událostí a vazby pro všechny podporované programovací jazyky.</span><span class="sxs-lookup"><span data-stu-id="c09c2-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="c09c2-107">Funkce, které jsou společné tooall vazby jsou zde popsány.</span><span class="sxs-lookup"><span data-stu-id="c09c2-107">Features that are common tooall bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="c09c2-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="c09c2-108">Overview</span></span>

<span data-ttu-id="c09c2-109">Triggerů a vazeb jsou deklarativní způsob toodefine způsob volání funkce a jaká data funguje s.</span><span class="sxs-lookup"><span data-stu-id="c09c2-109">Triggers and bindings are a declarative way toodefine how a function is invoked and what data it works with.</span></span> <span data-ttu-id="c09c2-110">A *aktivační událost* definuje způsob volání funkce.</span><span class="sxs-lookup"><span data-stu-id="c09c2-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="c09c2-111">Funkce musí mít přesně jeden aktivační události.</span><span class="sxs-lookup"><span data-stu-id="c09c2-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="c09c2-112">Aktivační události mají související data, která je obvykle hello datové části, která aktivuje funkce hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-112">Triggers have associated data, which is usually hello payload that triggered hello function.</span></span> 

<span data-ttu-id="c09c2-113">Vstup a výstup *vazby* zadejte toodata deklarativní způsob tooconnect z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-113">Input and output *bindings* provide a declarative way tooconnect toodata from within your code.</span></span> <span data-ttu-id="c09c2-114">Podobně jako tootriggers zadáte připojovací řetězce a další vlastnosti v konfiguraci funkce.</span><span class="sxs-lookup"><span data-stu-id="c09c2-114">Similar tootriggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="c09c2-115">Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="c09c2-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="c09c2-116">Pomocí triggerů a vazeb, můžete napsat kód, který je více obecné a nemá závislé hello podrobnosti hello služeb, se kterými komunikuje.</span><span class="sxs-lookup"><span data-stu-id="c09c2-116">Using triggers and bindings, you can write code that is more generic and does not hardcode hello details of hello services with which it interacts.</span></span> <span data-ttu-id="c09c2-117">Dat pocházejících z služby jednoduše stát vstupní hodnoty kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="c09c2-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="c09c2-118">Služba tooanother toooutput data (jako je vytvoření nového řádku v Azure Table Storage), použijte hello návratovou hodnotu metody hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-118">toooutput data tooanother service (such as creating a new row in Azure Table Storage), use hello return value of hello method.</span></span> <span data-ttu-id="c09c2-119">Nebo pokud potřebujete toooutput více hodnot, použijte pomocný objekt.</span><span class="sxs-lookup"><span data-stu-id="c09c2-119">Or, if you need toooutput multiple values, use a helper object.</span></span> <span data-ttu-id="c09c2-120">Mít triggerů a vazeb **název** vlastnost, která je identifikátor použijete v vazbě hello tooaccess vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-120">Triggers and bindings have a **name** property, which is an identifier you use in your code tooaccess hello binding.</span></span>

<span data-ttu-id="c09c2-121">Můžete nakonfigurovat triggerů a vazeb v hello **integrací** karta na portálu Azure Functions hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-121">You can configure triggers and bindings in hello **Integrate** tab in hello Azure Functions portal.</span></span> <span data-ttu-id="c09c2-122">V části hello zahrnuje hello uživatelského rozhraní upraví soubor s názvem *function.json* soubor v adresáři funkce hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-122">Under hello covers, hello UI modifies a file called *function.json* file in hello function directory.</span></span> <span data-ttu-id="c09c2-123">Tento soubor můžete upravit změnou toohello **pokročilé editor**.</span><span class="sxs-lookup"><span data-stu-id="c09c2-123">You can edit this file by changing toohello **Advanced editor**.</span></span>

<span data-ttu-id="c09c2-124">Hello následující tabulka uvádí hello triggerů a vazeb, které jsou podporovány Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c09c2-124">hello following table shows hello triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="c09c2-125">Příklad: Aktivace fronty a tabulky Výstupní vazby</span><span class="sxs-lookup"><span data-stu-id="c09c2-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="c09c2-126">Předpokládejme, že chcete toowrite nový řádek tooAzure Table Storage vždy, když v Azure Queue Storage se objeví nová zpráva.</span><span class="sxs-lookup"><span data-stu-id="c09c2-126">Suppose you want toowrite a new row tooAzure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="c09c2-127">Tento scénář může být implementovaná pomocí Azure Queue aktivační události a tabulku výstupní vazby.</span><span class="sxs-lookup"><span data-stu-id="c09c2-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="c09c2-128">Aktivační procedury fronty vyžaduje následující informace v hello hello **integrací** karty:</span><span class="sxs-lookup"><span data-stu-id="c09c2-128">A queue trigger requires hello following information in hello **Integrate** tab:</span></span>

* <span data-ttu-id="c09c2-129">Název nastavení aplikace hello, který obsahuje připojovací řetězec účet hello úložiště pro frontu hello Hello</span><span class="sxs-lookup"><span data-stu-id="c09c2-129">hello name of hello app setting that contains hello storage account connection string for hello queue</span></span>
* <span data-ttu-id="c09c2-130">Název fronty Hello</span><span class="sxs-lookup"><span data-stu-id="c09c2-130">hello queue name</span></span>
* <span data-ttu-id="c09c2-131">Hello identifikátor v váš kód tooread hello obsah uvítací zprávu fronty, jako například `order`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-131">hello identifier in your code tooread hello contents of hello queue message, such as `order`.</span></span>

<span data-ttu-id="c09c2-132">toowrite tooAzure Table Storage, použít vazbu výstup hello následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="c09c2-132">toowrite tooAzure Table Storage, use an output binding with hello following details:</span></span>

* <span data-ttu-id="c09c2-133">Název nastavení aplikace hello, který obsahuje připojovací řetězec účet hello úložiště pro tabulku hello Hello</span><span class="sxs-lookup"><span data-stu-id="c09c2-133">hello name of hello app setting that contains hello storage account connection string for hello table</span></span>
* <span data-ttu-id="c09c2-134">Název tabulky Hello</span><span class="sxs-lookup"><span data-stu-id="c09c2-134">hello table name</span></span>
* <span data-ttu-id="c09c2-135">identifikátor Hello v váš kód toocreate výstupní položky nebo hello návratovou hodnotou z funkce hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-135">hello identifier in your code toocreate output items, or hello return value from hello function.</span></span>

<span data-ttu-id="c09c2-136">Vazby používat nastavení aplikace pro připojovací řetězce tooenforce hello nejlepší praxi, který *function.json* neobsahuje tajné klíče služby.</span><span class="sxs-lookup"><span data-stu-id="c09c2-136">Bindings use app settings for connection strings tooenforce hello best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="c09c2-137">Poté použijte hello identifikátory, které jste zadali toointegrate s Azure Storage v kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-137">Then, use hello identifiers you provided toointegrate with Azure Storage in your code.</span></span>

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

<span data-ttu-id="c09c2-138">Tady je hello *function.json* odpovídající toohello předcházející kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-138">Here is hello *function.json* that corresponds toohello preceding code.</span></span> <span data-ttu-id="c09c2-139">Všimněte si, že hello, je možné použít stejnou konfiguraci, bez ohledu na jazyk hello implementace funkce hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-139">Note that hello same configuration can be used, regardless of hello language of hello function implementation.</span></span>

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
<span data-ttu-id="c09c2-140">tooview a úpravy hello obsah *function.json* v hello portálu Azure, klikněte na tlačítko hello **pokročilé editor** možnost na hello **integrací** kartě funkce.</span><span class="sxs-lookup"><span data-stu-id="c09c2-140">tooview and edit hello contents of *function.json* in hello Azure portal, click hello **Advanced editor** option on hello **Integrate** tab of your function.</span></span>

<span data-ttu-id="c09c2-141">Další příklady kódu a informace o integraci s Azure Storage najdete v tématu [Azure Functions triggerů a vazeb pro Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c09c2-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="c09c2-142">Směr vazby</span><span class="sxs-lookup"><span data-stu-id="c09c2-142">Binding direction</span></span>

<span data-ttu-id="c09c2-143">Mají všechny triggerů a vazeb `direction` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c09c2-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="c09c2-144">Pro aktivační události je vždy směr hello`in`</span><span class="sxs-lookup"><span data-stu-id="c09c2-144">For triggers, hello direction is always `in`</span></span>
- <span data-ttu-id="c09c2-145">Vstupní a výstupní vazby používat `in` a`out`</span><span class="sxs-lookup"><span data-stu-id="c09c2-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="c09c2-146">Některé vazby podporují speciální směr `inout`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="c09c2-147">Pokud používáte `inout`, pouze hello **pokročilé editor** je k dispozici v hello **integrací** kartě.</span><span class="sxs-lookup"><span data-stu-id="c09c2-147">If you use `inout`, only hello **Advanced editor** is available in hello **Integrate** tab.</span></span>

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a><span data-ttu-id="c09c2-148">Pomocí tooreturn návratový typ funkce hello jediného výstupu</span><span class="sxs-lookup"><span data-stu-id="c09c2-148">Using hello function return type tooreturn a single output</span></span>

<span data-ttu-id="c09c2-149">Hello předchozí příklad ukazuje, jak toouse hello funkce návratovou hodnotu tooprovide výstup tooa vazby, což je dosaženo pomocí hello speciální název parametru `$return`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-149">hello preceding example shows how toouse hello function return value tooprovide output tooa binding, which is achieved by using hello special name parameter `$return`.</span></span> <span data-ttu-id="c09c2-150">(Toto je jediná hodnota podporovaná v jazycích, které mají návratovou hodnotu, například C#, JavaScript a F #.) Pokud funkce má několik vazeb výstup, použijte `$return` pouze pro jeden z vazby výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of hello output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="c09c2-151">Příklady Hello níže ukazují jak návratové typy se používají s výstup vazeb v C#, JavaScript a F #.</span><span class="sxs-lookup"><span data-stu-id="c09c2-151">hello examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

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

## <a name="binding-datatype-property"></a><span data-ttu-id="c09c2-152">Vlastnost dataType vazby</span><span class="sxs-lookup"><span data-stu-id="c09c2-152">Binding dataType property</span></span>

<span data-ttu-id="c09c2-153">V rozhraní .NET použijte hello typy toodefine hello datový typ pro vstupní data.</span><span class="sxs-lookup"><span data-stu-id="c09c2-153">In .NET, use hello types toodefine hello data type for input data.</span></span> <span data-ttu-id="c09c2-154">Například použijte `string` toobind toohello text aktivační procedury fronty a bajtové pole tooread jako binární.</span><span class="sxs-lookup"><span data-stu-id="c09c2-154">For instance, use `string` toobind toohello text of a queue trigger and a byte array tooread as binary.</span></span>

<span data-ttu-id="c09c2-155">Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte hello `dataType` vlastnost v definici vazby hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-155">For languages that are dynamically typed such as JavaScript, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="c09c2-156">Například tooread hello obsahu požadavku HTTP v binárním formátu, použijte typ hello `binary`:</span><span class="sxs-lookup"><span data-stu-id="c09c2-156">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="c09c2-157">Další možnosti pro `dataType` jsou `stream` a `string`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="c09c2-158">Řešení nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="c09c2-158">Resolving app settings</span></span>
<span data-ttu-id="c09c2-159">Jako osvědčený postup tajné klíče a připojovací řetězce musí být řízen pomocí nastavení aplikace, nikoli konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="c09c2-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="c09c2-160">To omezuje přístup toothese tajných klíčů a umožňuje bezpečné toostore *function.json* v úložišti veřejné zdroj ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c09c2-160">This limits access toothese secrets and makes it safe toostore *function.json* in a public source control repository.</span></span>

<span data-ttu-id="c09c2-161">Nastavení aplikace jsou užitečné také vždy, když chcete toochange konfigurace na základě hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="c09c2-161">App settings are also useful whenever you want toochange configuration based on hello environment.</span></span> <span data-ttu-id="c09c2-162">Například v testovacím prostředí, můžete toomonitor jiný kontejner fronty nebo objekt blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="c09c2-162">For example, in a test environment, you may want toomonitor a different queue or blob storage container.</span></span>

<span data-ttu-id="c09c2-163">Nastavení aplikace jsou vyřešeny vždy, když hodnota je uzavřena mezi znaky procenta, jako například `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="c09c2-164">Všimněte si, že hello `connection` vlastnost triggerů a vazeb je zvláštní případ a automaticky vyřeší hodnoty jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c09c2-164">Note that hello `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="c09c2-165">Hello následující příklad je fronty aktivační událost, která používá nastavení aplikace `%input-queue-name%` toodefine hello fronty tootrigger na.</span><span class="sxs-lookup"><span data-stu-id="c09c2-165">hello following example is a queue trigger that uses an app setting `%input-queue-name%` toodefine hello queue tootrigger on.</span></span>

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

## <a name="trigger-metadata-properties"></a><span data-ttu-id="c09c2-166">Metadata vlastnosti aktivační události</span><span class="sxs-lookup"><span data-stu-id="c09c2-166">Trigger metadata properties</span></span>

<span data-ttu-id="c09c2-167">V přidání toohello datovou poskytované aktivační událost (například uvítací zprávu fronty, který aktivoval funkci) zadejte aktivační události mnoho dalších metadat hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c09c2-167">In addition toohello data payload provided by a trigger (such as hello queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="c09c2-168">Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti hello `context.bindings` objektu v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c09c2-168">These values can be used as input parameters in C# and F# or properties on hello `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="c09c2-169">Například aktivační procedury fronty podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c09c2-169">For example, a queue trigger supports hello following properties:</span></span>

* <span data-ttu-id="c09c2-170">QueueTrigger - aktivován obsah zprávy, pokud platný řetězec</span><span class="sxs-lookup"><span data-stu-id="c09c2-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="c09c2-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="c09c2-171">DequeueCount</span></span>
* <span data-ttu-id="c09c2-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="c09c2-172">ExpirationTime</span></span>
* <span data-ttu-id="c09c2-173">ID</span><span class="sxs-lookup"><span data-stu-id="c09c2-173">Id</span></span>
* <span data-ttu-id="c09c2-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="c09c2-174">InsertionTime</span></span>
* <span data-ttu-id="c09c2-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="c09c2-175">NextVisibleTime</span></span>
* <span data-ttu-id="c09c2-176">Vlastnosti PopReceipt</span><span class="sxs-lookup"><span data-stu-id="c09c2-176">PopReceipt</span></span>

<span data-ttu-id="c09c2-177">Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v hello odpovídající referenční téma.</span><span class="sxs-lookup"><span data-stu-id="c09c2-177">Details of metadata properties for each trigger are described in hello corresponding reference topic.</span></span> <span data-ttu-id="c09c2-178">Dokumentace je také dostupná v hello **integrací** kartě hello portálu hello **dokumentace** části níže oblast konfigurace vazby hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-178">Documentation is also available in hello **Integrate** tab of hello portal, in hello **Documentation** section below hello binding configuration area.</span></span>  

<span data-ttu-id="c09c2-179">Například vzhledem k tomu, že aktivační události objektu blob mají některé zpoždění, můžete použít frontě aktivační události toorun funkce (viz [aktivační události objektu Blob úložiště](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="c09c2-179">For example, since blob triggers have some delays, you can use a queue trigger toorun your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="c09c2-180">uvítací zprávu fronty by obsahovat hello blob filename tootrigger na.</span><span class="sxs-lookup"><span data-stu-id="c09c2-180">hello queue message would contain hello blob filename tootrigger on.</span></span> <span data-ttu-id="c09c2-181">Pomocí hello `queueTrigger` vlastnost metadat, toto chování můžete zadat všechny v konfiguraci, nikoli kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-181">Using hello `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

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

<span data-ttu-id="c09c2-182">Metadata vlastnosti aktivační události lze také v *vazby výraz* pro jiné vazbu jako následující části popisují hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in hello following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="c09c2-183">Výrazy vazba a vzory</span><span class="sxs-lookup"><span data-stu-id="c09c2-183">Binding expressions and patterns</span></span>

<span data-ttu-id="c09c2-184">Jedním z nejúčinnějších funkce hello triggerů a vazeb je *vazby výrazy*.</span><span class="sxs-lookup"><span data-stu-id="c09c2-184">One of hello most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="c09c2-185">V rámci vaší vazba, můžete definovat vzor výrazy, které lze poté použít v jiné vazby nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="c09c2-186">Aktivační událost metadata mohou sloužit také v vazby výrazy, jak je vidět v ukázce hello v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="c09c2-186">Trigger metadata can also be used in binding expressions, as show in hello sample in hello preceding section.</span></span>

<span data-ttu-id="c09c2-187">Předpokládejme například, že chcete tooresize bitové kopie v kontejneru konkrétní objektu blob úložiště, podobně jako toohello **Úprava velikosti obrázku** šablony v hello **novou funkci** stránky.</span><span class="sxs-lookup"><span data-stu-id="c09c2-187">For example, suppose you want tooresize images in particular blob storage container, similar toohello **Image Resizer** template in hello **New Function** page.</span></span> <span data-ttu-id="c09c2-188">Přejděte příliš**novou funkci** -> jazyka **C#** -> scénář **ukázky** -> **ImageResizer CSharp**.</span><span class="sxs-lookup"><span data-stu-id="c09c2-188">Go too**New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="c09c2-189">Tady je hello *function.json* definice:</span><span class="sxs-lookup"><span data-stu-id="c09c2-189">Here is hello *function.json* definition:</span></span>

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

<span data-ttu-id="c09c2-190">Všimněte si, že hello `filename` parametr se používá v definici objektu blob aktivace hello jak hello blob výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="c09c2-190">Notice that hello `filename` parameter is used in both hello blob trigger definition as well as hello blob output binding.</span></span> <span data-ttu-id="c09c2-191">Tento parametr můžete použít také v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="c09c2-191">This parameter can also be used in function code.</span></span>

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


### <a name="random-guids"></a><span data-ttu-id="c09c2-192">Náhodné identifikátory GUID</span><span class="sxs-lookup"><span data-stu-id="c09c2-192">Random GUIDs</span></span>
<span data-ttu-id="c09c2-193">Azure Functions nabízí pohodlí syntaxe pro generování identifikátory GUID v vazby, prostřednictvím hello `{rand-guid}` vazby výraz.</span><span class="sxs-lookup"><span data-stu-id="c09c2-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through hello `{rand-guid}` binding expression.</span></span> <span data-ttu-id="c09c2-194">Hello následující příklad používá tento toogenerate název jedinečný objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c09c2-194">hello following example uses this toogenerate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="c09c2-195">Aktuální čas</span><span class="sxs-lookup"><span data-stu-id="c09c2-195">Current time</span></span>

<span data-ttu-id="c09c2-196">Můžete použít výraz vazby hello `DateTime`, který přeloží příliš`DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="c09c2-196">You can use hello binding expression `DateTime`, which resolves too`DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a><span data-ttu-id="c09c2-197">Vytvoření vazby vlastnosti vstupu toocustom ve výrazu vazby</span><span class="sxs-lookup"><span data-stu-id="c09c2-197">Bind toocustom input properties in a binding expression</span></span>

<span data-ttu-id="c09c2-198">Vazba výrazy můžete taky odkazovat vlastnosti, které jsou definovány v datové části hello aktivační událost, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c09c2-198">Binding expressions can also reference properties that are defined in hello trigger payload itself.</span></span> <span data-ttu-id="c09c2-199">Například můžete soubor úložiště toodynamically vazby tooa objektů blob z uvedených v webhook, jehož název souboru.</span><span class="sxs-lookup"><span data-stu-id="c09c2-199">For example, you may want toodynamically bind tooa blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="c09c2-200">Například hello následující *function.json* používá vlastnost s názvem `BlobName` z datové části hello aktivační události:</span><span class="sxs-lookup"><span data-stu-id="c09c2-200">For example, hello following *function.json* uses a property called `BlobName` from hello trigger payload:</span></span>

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

<span data-ttu-id="c09c2-201">tooaccomplish tento v C# a F #, je nutné definovat objektů POCO, definující hello pole, která bude deserializovat v datové části hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="c09c2-201">tooaccomplish this in C# and F#, you must define a POCO that defines hello fields that will be deserialized in hello trigger payload.</span></span>

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

<span data-ttu-id="c09c2-202">V jazyce JavaScript se provádí automaticky deserializaci JSON a hello vlastnosti můžete používat přímo.</span><span class="sxs-lookup"><span data-stu-id="c09c2-202">In JavaScript, JSON deserialization is automatically performed and you can use hello properties directly.</span></span>

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

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="c09c2-203">Konfigurace vazba dat za běhu</span><span class="sxs-lookup"><span data-stu-id="c09c2-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="c09c2-204">V jazyce C# a jinými jazyky rozhraní .NET, můžete použít vzor imperativní vazby jako názvem na rozdíl od toohello deklarativní vazeb v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="c09c2-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed toohello declarative bindings in *function.json*.</span></span> <span data-ttu-id="c09c2-205">Imperativní vazba je užitečné, když vázané parametry potřebovat toobe počítaný v době běhu spíše než návrhu.</span><span class="sxs-lookup"><span data-stu-id="c09c2-205">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="c09c2-206">Další, najdete v části toolearn [vazby za běhu prostřednictvím imperativní vazby](functions-reference-csharp.md#imperative-bindings) v referenční informace pro vývojáře hello C#.</span><span class="sxs-lookup"><span data-stu-id="c09c2-206">toolearn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in hello C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c09c2-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c09c2-207">Next steps</span></span>
<span data-ttu-id="c09c2-208">Další informace o konkrétní vazbu najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="c09c2-208">For more information on a specific binding, see hello following articles:</span></span>

- [<span data-ttu-id="c09c2-209">HTTP a webhooky</span><span class="sxs-lookup"><span data-stu-id="c09c2-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="c09c2-210">Časovač</span><span class="sxs-lookup"><span data-stu-id="c09c2-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="c09c2-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="c09c2-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="c09c2-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c09c2-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="c09c2-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="c09c2-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="c09c2-214">Centrum událostí</span><span class="sxs-lookup"><span data-stu-id="c09c2-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="c09c2-215">Service Bus</span><span class="sxs-lookup"><span data-stu-id="c09c2-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="c09c2-216">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="c09c2-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="c09c2-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="c09c2-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="c09c2-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="c09c2-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="c09c2-219">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="c09c2-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="c09c2-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="c09c2-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="c09c2-221">Externí soubor</span><span class="sxs-lookup"><span data-stu-id="c09c2-221">External file</span></span>](functions-bindings-external-file.md)
