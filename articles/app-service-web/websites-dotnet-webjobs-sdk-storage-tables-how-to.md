---
title: "aaaHow toouse úložiště tabulek Azure s hello WebJobs SDK"
description: "Zjistěte, jak toouse Azure table storage s hello WebJobs SDK. Vytváření tabulek, přidejte tootables entity a čtení existující tabulky."
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
ms.openlocfilehash: 8e28c69df4a934646add9e50c6de28e76dca1636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="3e946-104">Jak toouse Azure table storage s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="3e946-104">How toouse Azure table storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="3e946-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="3e946-105">Overview</span></span>
<span data-ttu-id="3e946-106">Tato příručka obsahuje C# – ukázky kódu, které zobrazují jak tooread a zápisu úložiště Azure tabulky pomocí [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="3e946-106">This guide provides C# code samples that show how tooread and write Azure storage tables by using [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="3e946-107">Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="3e946-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="3e946-108">Některé hello fragmenty kódu ukazují hello `Table` atribut používaných ve funkcích, které jsou [názvem ručně](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), která je není pomocí jeden z atributů hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="3e946-108">Some of hello code snippets show hello `Table` attribute used in functions that are [called manually](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), that is, not by using one of hello trigger attributes.</span></span> 

## <span data-ttu-id="3e946-109"><a id="ingress"></a>Jak tooadd entity tooa tabulky</span><span class="sxs-lookup"><span data-stu-id="3e946-109"><a id="ingress"></a> How tooadd entities tooa table</span></span>
<span data-ttu-id="3e946-110">tooadd entity tooa tabulky, použijte hello `Table` atribut s `ICollector<T>` nebo `IAsyncCollector<T>` parametr kde `T` Určuje schéma hello hello entit chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="3e946-110">tooadd entities tooa table, use hello `Table` attribute with an `ICollector<T>` or `IAsyncCollector<T>` parameter where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="3e946-111">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e946-111">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span> 

<span data-ttu-id="3e946-112">Přidá technologie Hello následující ukázka kódu `Person` entity tooa tabulku s názvem *příjem příchozích dat*.</span><span class="sxs-lookup"><span data-stu-id="3e946-112">hello following code sample adds `Person` entities tooa table named *Ingress*.</span></span>

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

<span data-ttu-id="3e946-113">Obvykle hello typ můžete používat s `ICollector` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="3e946-113">Typically hello type you use with `ICollector` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="3e946-114">Některý z následujících hello `Person` třídy práce s kódem hello uvedené v předchozí hello `Ingress` metoda.</span><span class="sxs-lookup"><span data-stu-id="3e946-114">Either of hello following `Person` classes work with hello code shown in hello preceding `Ingress` method.</span></span>

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

<span data-ttu-id="3e946-115">Pokud chcete toowork přímo s hello úložiště Azure API, můžete přidat `CloudStorageAccount` podpis metody toohello parametr.</span><span class="sxs-lookup"><span data-stu-id="3e946-115">If you want toowork directly with hello Azure storage API, you can add a `CloudStorageAccount` parameter toohello method signature.</span></span>

## <span data-ttu-id="3e946-116"><a id="monitor"></a>Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="3e946-116"><a id="monitor"></a> Real-time monitoring</span></span>
<span data-ttu-id="3e946-117">Protože funkce příchozí přenos dat často zpracování velkých objemů dat, hello řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3e946-117">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="3e946-118">Hello **volání protokolu** části řekne, pokud je stále spuštěná hello funkce.</span><span class="sxs-lookup"><span data-stu-id="3e946-118">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

<span data-ttu-id="3e946-120">Hello **volání podrobnosti** stránky sestavy hello funkce průběh (počet entit, které jsou zapsány) při běží a poskytuje tooabort možnost ho.</span><span class="sxs-lookup"><span data-stu-id="3e946-120">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span> 

![Příjem příchozích dat funkci spouštění](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

<span data-ttu-id="3e946-122">Když funkce hello dokončení hello **volání podrobnosti** stránky sestavy hello počet řádků, které jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="3e946-122">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Funkce pro příjem příchozích dat bylo dokončeno](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <span data-ttu-id="3e946-124"><a id="multiple"></a>Jak tooread více entit z tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e946-124"><a id="multiple"></a> How tooread multiple entities from a table</span></span>
<span data-ttu-id="3e946-125">tooread tabulky, použijte hello `Table` atribut s `IQueryable<T>` parametr kde zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="3e946-125">tooread a table, use hello `Table` attribute with an `IQueryable<T>` parameter where type `T` derives from `TableEntity` or implements `ITableEntity`.</span></span>

<span data-ttu-id="3e946-126">Hello následující ukázka kódu čte a zaznamená všechny řádky z hello `Ingress` tabulky:</span><span class="sxs-lookup"><span data-stu-id="3e946-126">hello following code sample reads and logs all rows from hello `Ingress` table:</span></span>

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

### <span data-ttu-id="3e946-127"><a id="readone"></a>Jak tooread jedné entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e946-127"><a id="readone"></a> How tooread a single entity from a table</span></span>
<span data-ttu-id="3e946-128">Došlo `Table` konstruktoru atributu s dva další parametry, které umožňují určit hello klíč oddílu a klíč řádku, pokud chcete toobind tooa jedné tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="3e946-128">There is a `Table` attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="3e946-129">Hello následující ukázka kódu čte pro řádek tabulky `Person` entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:</span><span class="sxs-lookup"><span data-stu-id="3e946-129">hello following code sample reads a table row for a `Person` entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="3e946-130">Hello `Person` třídy v tomto příkladu nemá tooimplement `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="3e946-130">hello `Person` class in this example does not have tooimplement `ITableEntity`.</span></span>

## <span data-ttu-id="3e946-131"><a id="storageapi"></a>Jak toouse hello .NET API úložiště přímo toowork s tabulkou</span><span class="sxs-lookup"><span data-stu-id="3e946-131"><a id="storageapi"></a> How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="3e946-132">Můžete taky hello `Table` atribut s `CloudTable` objekt pro větší flexibilitu při práci s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="3e946-132">You can also use hello `Table` attribute with a `CloudTable` object for more flexibility in working with a table.</span></span>

<span data-ttu-id="3e946-133">Hello následující kód používá ukázka `CloudTable` objektu tooadd jedné entity toohello *příjem příchozích dat* tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e946-133">hello following code sample uses a `CloudTable` object tooadd a single entity toohello *Ingress* table.</span></span> 

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

<span data-ttu-id="3e946-134">Další informace o tom, toouse hello `CloudTable` objektu, najdete v části [jak toouse úložiště Table z .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="3e946-134">For more information about how toouse hello `CloudTable` object, see [How toouse Table Storage from .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span> 

## <span data-ttu-id="3e946-135"><a id="queues"></a>Související témata předmětem hello fronty postupy tooarticle</span><span class="sxs-lookup"><span data-stu-id="3e946-135"><a id="queues"></a>Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="3e946-136">Informace o tom, jak zpracování tabulky toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není zpracování, najdete v části konkrétní tootable [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="3e946-136">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="3e946-137">Obsahuje následující témata v tomto článku hello následující:</span><span class="sxs-lookup"><span data-stu-id="3e946-137">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="3e946-138">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="3e946-138">Async functions</span></span>
* <span data-ttu-id="3e946-139">Více instancí</span><span class="sxs-lookup"><span data-stu-id="3e946-139">Multiple instances</span></span>
* <span data-ttu-id="3e946-140">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="3e946-140">Graceful shutdown</span></span>
* <span data-ttu-id="3e946-141">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="3e946-141">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="3e946-142">Nastavení hello SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="3e946-142">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="3e946-143">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="3e946-143">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="3e946-144">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="3e946-144">Trigger a function manually</span></span>
* <span data-ttu-id="3e946-145">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="3e946-145">Write logs</span></span>

## <span data-ttu-id="3e946-146"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e946-146"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="3e946-147">Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="3e946-147">This guide has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="3e946-148">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="3e946-148">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

