---
title: "Začínáme s Azure storage a Visual Studio připojené služeb (webové úlohy projekty)"
description: "Jak začít používat Azure Table storage v Azure WebJobs projektu v sadě Visual Studio po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 79fba102377cdc6b681f6798699767961040a7e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a><span data-ttu-id="e7539-103">Začínáme s Azure Storage (webová úloha Azure projekty)</span><span class="sxs-lookup"><span data-stu-id="e7539-103">Getting Started with Azure Storage (Azure WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="e7539-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e7539-104">Overview</span></span>
<span data-ttu-id="e7539-105">Tento článek obsahuje C# ukázek kódu, které ukazují ukazují, jak používat Azure WebJobs SDK verze 1.x s služby úložiště Azure table.</span><span class="sxs-lookup"><span data-stu-id="e7539-105">This article provides C# code samples that show show how to use the Azure WebJobs SDK version 1.x with the Azure table storage service.</span></span> <span data-ttu-id="e7539-106">Kód – ukázky použití [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="e7539-106">The code samples use the [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="e7539-107">Služba úložiště Azure Table umožňuje ukládat velké množství strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="e7539-107">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="e7539-108">Služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z uvnitř i vně cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="e7539-108">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="e7539-109">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="e7539-109">Azure tables are ideal for storing structured, non-relational data.</span></span>  <span data-ttu-id="e7539-110">V tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md#create-a-table) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7539-110">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md#create-a-table) for more information.</span></span>

<span data-ttu-id="e7539-111">Některé zobrazit fragmenty kódu **tabulky** atribut používaných ve funkcích, které se nazývají ručně, tedy ne prostřednictvím jeden z atributů aktivační události.</span><span class="sxs-lookup"><span data-stu-id="e7539-111">Some of the code snippets show the **Table** attribute used in functions that are called manually, that is, not by using one of the trigger attributes.</span></span>

## <a name="how-to-add-entities-to-a-table"></a><span data-ttu-id="e7539-112">Postup přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="e7539-112">How to add entities to a table</span></span>
<span data-ttu-id="e7539-113">K přidání entity do tabulky, použijte **tabulky** atribut s **ICollector<T>**  nebo **IAsyncCollector<T>**  parametr kde **T** Určuje schéma entity, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="e7539-113">To add entities to a table, use the **Table** attribute with an **ICollector<T>** or **IAsyncCollector<T>** parameter where **T** specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="e7539-114">Konstruktoru atributu přijímá řetězcový parametr, který určuje název tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7539-114">The attribute constructor takes a string parameter that specifies the name of the table.</span></span>

<span data-ttu-id="e7539-115">Následující ukázka kódu přidá **osoba** entity do tabulky s názvem *příjem příchozích dat*.</span><span class="sxs-lookup"><span data-stu-id="e7539-115">The following code sample adds **Person** entities to a table named *Ingress*.</span></span>

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

<span data-ttu-id="e7539-116">Obvykle typ můžete používat s **ICollector** je odvozena z **TableEntity** nebo implementuje **ITableEntity**, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="e7539-116">Typically the type you use with **ICollector** derives from **TableEntity** or implements **ITableEntity**, but it doesn't have to.</span></span> <span data-ttu-id="e7539-117">Z následujících **osoba** třídy práce s kódem zobrazeným v předchozím **příjem příchozích dat** metoda.</span><span class="sxs-lookup"><span data-stu-id="e7539-117">Either of the following **Person** classes work with the code shown in the preceding **Ingress** method.</span></span>

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

<span data-ttu-id="e7539-118">Pokud chcete pracovat přímo s úložištěm Azure API, můžete přidat **CloudStorageAccount** parametru podpis metody.</span><span class="sxs-lookup"><span data-stu-id="e7539-118">If you want to work directly with the Azure storage API, you can add a **CloudStorageAccount** parameter to the method signature.</span></span>

## <a name="real-time-monitoring"></a><span data-ttu-id="e7539-119">Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="e7539-119">Real-time monitoring</span></span>
<span data-ttu-id="e7539-120">Protože funkce příchozí přenos dat často zpracování velkých objemů dat, řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="e7539-120">Because data ingress functions often process large volumes of data, the WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="e7539-121">**Volání protokolu** části oznamuje, zda je stále spuštěna funkce.</span><span class="sxs-lookup"><span data-stu-id="e7539-121">The **Invocation Log** section tells you if the function is still running.</span></span>

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

<span data-ttu-id="e7539-123">**Volání podrobnosti** stránky sestavy průběhu funkce (počet entit, které jsou zapsány) je spuštěn a vám dává příležitost k přerušení ho.</span><span class="sxs-lookup"><span data-stu-id="e7539-123">The **Invocation Details** page reports the function's progress (number of entities written) while it's running and gives you an opportunity to abort it.</span></span>

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

<span data-ttu-id="e7539-125">Po dokončení funkce **volání podrobnosti** stránky sestavy počet řádků, které jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="e7539-125">When the function finishes, the **Invocation Details** page reports the number of rows written.</span></span>

![Funkce pro příjem příchozích dat bylo dokončeno](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a><span data-ttu-id="e7539-127">Jak si více entit z tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7539-127">How to read multiple entities from a table</span></span>
<span data-ttu-id="e7539-128">Čtení tabulky, použijte **tabulky** atribut s **IQueryable<T>**  parametr kde zadejte **T** je odvozena z **TableEntity** nebo implementuje **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="e7539-128">To read a table, use the **Table** attribute with an **IQueryable<T>** parameter where type **T** derives from **TableEntity** or implements **ITableEntity**.</span></span>

<span data-ttu-id="e7539-129">Následující ukázka kódu čte a zaznamená všechny řádky z **příjem příchozích dat** tabulky:</span><span class="sxs-lookup"><span data-stu-id="e7539-129">The following code sample reads and logs all rows from the **Ingress** table:</span></span>

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

### <a name="how-to-read-a-single-entity-from-a-table"></a><span data-ttu-id="e7539-130">Informace o načtení jedné entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7539-130">How to read a single entity from a table</span></span>
<span data-ttu-id="e7539-131">Je **tabulky** konstruktoru atributu s dva další parametry, které umožňují zadat klíč oddílu a klíč řádku, pokud chcete vytvořit vazbu k jedné tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="e7539-131">There is a **Table** attribute constructor with two additional parameters that let you specify the partition key and row key when you want to bind to a single table entity.</span></span>

<span data-ttu-id="e7539-132">Následující ukázka kódu čte pro řádek tabulky **osoba** entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:</span><span class="sxs-lookup"><span data-stu-id="e7539-132">The following code sample reads a table row for a **Person** entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="e7539-133">**Osoba** třídy v tomto příkladu není nutné implementovat **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="e7539-133">The **Person** class in this example does not have to implement **ITableEntity**.</span></span>

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a><span data-ttu-id="e7539-134">Jak používat rozhraní API .NET úložiště přímo do tabulky</span><span class="sxs-lookup"><span data-stu-id="e7539-134">How to use the .NET Storage API directly to work with a table</span></span>
<span data-ttu-id="e7539-135">Můžete také **tabulky** atribut s **CloudTable** objekt pro větší flexibilitu při práci s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="e7539-135">You can also use the **Table** attribute with a **CloudTable** object for more flexibility in working with a table.</span></span>

<span data-ttu-id="e7539-136">Následující kód používá ukázka **CloudTable** objekt, který chcete přidat do jedné entity *příjem příchozích dat* tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7539-136">The following code sample uses a **CloudTable** object to add a single entity to the *Ingress* table.</span></span>

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

<span data-ttu-id="e7539-137">Další informace o tom, jak používat **CloudTable** objektu, najdete v části [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="e7539-137">For more information about how to use the **CloudTable** object, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

## <a name="related-topics-covered-by-the-queues-how-to-article"></a><span data-ttu-id="e7539-138">Související témata předmětem článek s postupy fronty</span><span class="sxs-lookup"><span data-stu-id="e7539-138">Related topics covered by the queues how-to article</span></span>
<span data-ttu-id="e7539-139">Informace o způsobu zpracování zpracování tabulky aktivovány zprávu fronty, nebo pro scénáře WebJobs SDK, které nejsou specifické pro zpracování tabulky, najdete v části [Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty)](vs-storage-webjobs-getting-started-queues.md).</span><span class="sxs-lookup"><span data-stu-id="e7539-139">For information about how to handle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific to table processing, see [Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)](vs-storage-webjobs-getting-started-queues.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7539-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7539-140">Next steps</span></span>
<span data-ttu-id="e7539-141">Tento článek poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="e7539-141">This article has provided code samples that show how to handle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="e7539-142">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="e7539-142">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

