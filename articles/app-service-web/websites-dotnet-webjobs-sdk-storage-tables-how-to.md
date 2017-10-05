---
title: "Používání Azure Table Storage se sadou WebJobs SDK"
description: "Další informace o použití úložiště tabulek Azure pomocí WebJobs SDK. Vytváření tabulek, přidání entity do tabulky a čtení existující tabulky."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 451432cc-c780-4310-85d3-84f44fe48afe
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 13cfc788c14d714df7022ce003d34691cf73d121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-with-the-webjobs-sdk"></a><span data-ttu-id="f2fd8-104">Používání Azure Table Storage se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="f2fd8-104">How to use Azure table storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="f2fd8-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="f2fd8-105">Overview</span></span>
<span data-ttu-id="f2fd8-106">Tato příručka obsahuje C# ukázek kódu, které ukazují, jak číst a zapisovat tabulky úložiště Azure pomocí [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-106">This guide provides C# code samples that show how to read and write Azure storage tables by using [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="f2fd8-107">V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="f2fd8-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="f2fd8-108">Některé zobrazit fragmenty kódu `Table` atribut používaných ve funkcích, které jsou [názvem ručně](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), která je není pomocí jeden z atributů aktivační události.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-108">Some of the code snippets show the `Table` attribute used in functions that are [called manually](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), that is, not by using one of the trigger attributes.</span></span> 

## <span data-ttu-id="f2fd8-109"><a id="ingress"></a>Postup přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="f2fd8-109"><a id="ingress"></a> How to add entities to a table</span></span>
<span data-ttu-id="f2fd8-110">K přidání entity do tabulky, použijte `Table` atribut s `ICollector<T>` nebo `IAsyncCollector<T>` parametr kde `T` Určuje schéma entity, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-110">To add entities to a table, use the `Table` attribute with an `ICollector<T>` or `IAsyncCollector<T>` parameter where `T` specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="f2fd8-111">Konstruktoru atributu přijímá řetězcový parametr, který určuje název tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-111">The attribute constructor takes a string parameter that specifies the name of the table.</span></span> 

<span data-ttu-id="f2fd8-112">Následující ukázka kódu přidá `Person` entity do tabulky s názvem *příjem příchozích dat*.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-112">The following code sample adds `Person` entities to a table named *Ingress*.</span></span>

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

<span data-ttu-id="f2fd8-113">Obvykle typ můžete používat s `ICollector` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-113">Typically the type you use with `ICollector` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="f2fd8-114">Z následujících `Person` třídy práce s kódem zobrazeným v předchozím `Ingress` metoda.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-114">Either of the following `Person` classes work with the code shown in the preceding `Ingress` method.</span></span>

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

<span data-ttu-id="f2fd8-115">Pokud chcete pracovat přímo s úložištěm Azure API, můžete přidat `CloudStorageAccount` parametr podpis metody.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-115">If you want to work directly with the Azure storage API, you can add a `CloudStorageAccount` parameter to the method signature.</span></span>

## <span data-ttu-id="f2fd8-116"><a id="monitor"></a>Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="f2fd8-116"><a id="monitor"></a> Real-time monitoring</span></span>
<span data-ttu-id="f2fd8-117">Protože funkce příchozí přenos dat často zpracování velkých objemů dat, řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-117">Because data ingress functions often process large volumes of data, the WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="f2fd8-118">**Volání protokolu** části oznamuje, zda je stále spuštěna funkce.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-118">The **Invocation Log** section tells you if the function is still running.</span></span>

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

<span data-ttu-id="f2fd8-120">**Volání podrobnosti** stránky sestavy průběhu funkce (počet entit, které jsou zapsány) je spuštěn a vám dává příležitost k přerušení ho.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-120">The **Invocation Details** page reports the function's progress (number of entities written) while it's running and gives you an opportunity to abort it.</span></span> 

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

<span data-ttu-id="f2fd8-122">Po dokončení funkce **volání podrobnosti** stránky sestavy počet řádků, které jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-122">When the function finishes, the **Invocation Details** page reports the number of rows written.</span></span>

![Funkce pro příjem příchozích dat bylo dokončeno](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <span data-ttu-id="f2fd8-124"><a id="multiple"></a>Jak si více entit z tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-124"><a id="multiple"></a> How to read multiple entities from a table</span></span>
<span data-ttu-id="f2fd8-125">Čtení tabulky, použijte `Table` atribut s `IQueryable<T>` parametr kde zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-125">To read a table, use the `Table` attribute with an `IQueryable<T>` parameter where type `T` derives from `TableEntity` or implements `ITableEntity`.</span></span>

<span data-ttu-id="f2fd8-126">Následující ukázka kódu čte a zaznamená všechny řádky z `Ingress` tabulky:</span><span class="sxs-lookup"><span data-stu-id="f2fd8-126">The following code sample reads and logs all rows from the `Ingress` table:</span></span>

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <span data-ttu-id="f2fd8-127"><a id="readone"></a>Informace o načtení jedné entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-127"><a id="readone"></a> How to read a single entity from a table</span></span>
<span data-ttu-id="f2fd8-128">Došlo `Table` konstruktoru atributu s dva další parametry, které umožňují zadat klíč oddílu a klíč řádku, pokud chcete vytvořit vazbu k jedné tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-128">There is a `Table` attribute constructor with two additional parameters that let you specify the partition key and row key when you want to bind to a single table entity.</span></span>

<span data-ttu-id="f2fd8-129">Následující ukázka kódu čte pro řádek tabulky `Person` entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:</span><span class="sxs-lookup"><span data-stu-id="f2fd8-129">The following code sample reads a table row for a `Person` entity based on partition key and row key values received in a queue message:</span></span>  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


<span data-ttu-id="f2fd8-130">`Person` Třídy v tomto příkladu nemá k implementaci `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-130">The `Person` class in this example does not have to implement `ITableEntity`.</span></span>

## <span data-ttu-id="f2fd8-131"><a id="storageapi"></a>Jak používat rozhraní API .NET úložiště přímo do tabulky</span><span class="sxs-lookup"><span data-stu-id="f2fd8-131"><a id="storageapi"></a> How to use the .NET Storage API directly to work with a table</span></span>
<span data-ttu-id="f2fd8-132">Můžete také `Table` atribut s `CloudTable` objekt pro větší flexibilitu při práci s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-132">You can also use the `Table` attribute with a `CloudTable` object for more flexibility in working with a table.</span></span>

<span data-ttu-id="f2fd8-133">Následující kód používá ukázka `CloudTable` objekt, který chcete přidat do jedné entity *příjem příchozích dat* tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-133">The following code sample uses a `CloudTable` object to add a single entity to the *Ingress* table.</span></span> 

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

<span data-ttu-id="f2fd8-134">Další informace o tom, jak používat `CloudTable` objektu, najdete v části [postup používání úložiště Table z .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f2fd8-134">For more information about how to use the `CloudTable` object, see [How to use Table Storage from .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span> 

## <span data-ttu-id="f2fd8-135"><a id="queues"></a>Související témata předmětem článek s postupy fronty</span><span class="sxs-lookup"><span data-stu-id="f2fd8-135"><a id="queues"></a>Related topics covered by the queues how-to article</span></span>
<span data-ttu-id="f2fd8-136">Informace o způsobu zpracování zpracování tabulky aktivovány zprávu fronty, nebo pro scénáře WebJobs SDK, které nejsou specifické pro zpracování tabulky, najdete v části [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="f2fd8-136">For information about how to handle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific to table processing, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="f2fd8-137">Následující témata popsaná v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="f2fd8-137">Topics covered in that article include the following:</span></span>

* <span data-ttu-id="f2fd8-138">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="f2fd8-138">Async functions</span></span>
* <span data-ttu-id="f2fd8-139">Více instancí</span><span class="sxs-lookup"><span data-stu-id="f2fd8-139">Multiple instances</span></span>
* <span data-ttu-id="f2fd8-140">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="f2fd8-140">Graceful shutdown</span></span>
* <span data-ttu-id="f2fd8-141">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="f2fd8-141">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="f2fd8-142">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="f2fd8-142">Set the SDK connection strings in code</span></span>
* <span data-ttu-id="f2fd8-143">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="f2fd8-143">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="f2fd8-144">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="f2fd8-144">Trigger a function manually</span></span>
* <span data-ttu-id="f2fd8-145">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="f2fd8-145">Write logs</span></span>

## <span data-ttu-id="f2fd8-146"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2fd8-146"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="f2fd8-147">Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="f2fd8-147">This guide has provided code samples that show how to handle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="f2fd8-148">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="f2fd8-148">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

