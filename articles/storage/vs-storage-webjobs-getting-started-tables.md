---
title: "aaaGetting Začínáme s Azure storage a Visual Studio připojené služeb (webové úlohy projekty)"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Table storage v Azure WebJobs projektu v sadě Visual Studio"
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
ms.openlocfilehash: 448dee3369207062ee85c6636c84bcb4e26a2085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a><span data-ttu-id="2e7ae-103">Začínáme s Azure Storage (webová úloha Azure projekty)</span><span class="sxs-lookup"><span data-stu-id="2e7ae-103">Getting Started with Azure Storage (Azure WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="2e7ae-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2e7ae-104">Overview</span></span>
<span data-ttu-id="2e7ae-105">Tento článek obsahuje zobrazit C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello služby úložiště Azure table.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-105">This article provides C# code samples that show show how toouse hello Azure WebJobs SDK version 1.x with hello Azure table storage service.</span></span> <span data-ttu-id="2e7ae-106">Ukázky kódu Hello použít hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="2e7ae-107">Hello služba úložiště Azure Table umožňuje toostore velkých objemů strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="2e7ae-108">Hello služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="2e7ae-109">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-109">Azure tables are ideal for storing structured, non-relational data.</span></span>  <span data-ttu-id="2e7ae-110">V tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md#create-a-table) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-110">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md#create-a-table) for more information.</span></span>

<span data-ttu-id="2e7ae-111">Některé hello fragmenty kódu ukazují hello **tabulky** atribut používaných ve funkcích, které se nazývají ručně, tedy ne prostřednictvím jeden z atributů hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-111">Some of hello code snippets show hello **Table** attribute used in functions that are called manually, that is, not by using one of hello trigger attributes.</span></span>

## <a name="how-tooadd-entities-tooa-table"></a><span data-ttu-id="2e7ae-112">Jak tooadd entity tooa tabulky</span><span class="sxs-lookup"><span data-stu-id="2e7ae-112">How tooadd entities tooa table</span></span>
<span data-ttu-id="2e7ae-113">tooadd entity tooa tabulky, použijte hello **tabulky** atribut s **ICollector<T>**  nebo **IAsyncCollector<T>**  parametr kde **T** Určuje schéma hello hello entit chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-113">tooadd entities tooa table, use hello **Table** attribute with an **ICollector<T>** or **IAsyncCollector<T>** parameter where **T** specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="2e7ae-114">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-114">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span>

<span data-ttu-id="2e7ae-115">Přidá technologie Hello následující ukázka kódu **osoba** entity tooa tabulku s názvem *příjem příchozích dat*.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-115">hello following code sample adds **Person** entities tooa table named *Ingress*.</span></span>

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

<span data-ttu-id="2e7ae-116">Obvykle hello typ můžete používat s **ICollector** je odvozena z **TableEntity** nebo implementuje **ITableEntity**, ale nemusí to.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-116">Typically hello type you use with **ICollector** derives from **TableEntity** or implements **ITableEntity**, but it doesn't have to.</span></span> <span data-ttu-id="2e7ae-117">Některý z následujících hello **osoba** třídy práce s kódem hello uvedené v předchozí hello **příjem příchozích dat** metoda.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-117">Either of hello following **Person** classes work with hello code shown in hello preceding **Ingress** method.</span></span>

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

<span data-ttu-id="2e7ae-118">Pokud chcete toowork přímo s hello úložiště Azure API, můžete přidat **CloudStorageAccount** podpis metody toohello parametr.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-118">If you want toowork directly with hello Azure storage API, you can add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="real-time-monitoring"></a><span data-ttu-id="2e7ae-119">Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="2e7ae-119">Real-time monitoring</span></span>
<span data-ttu-id="2e7ae-120">Protože funkce příchozí přenos dat často zpracování velkých objemů dat, hello řídicím panelu WebJobs SDK poskytuje data monitorování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-120">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="2e7ae-121">Hello **volání protokolu** části řekne, pokud je stále spuštěná hello funkce.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-121">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

<span data-ttu-id="2e7ae-123">Hello **volání podrobnosti** stránky sestavy hello funkce průběh (počet entit, které jsou zapsány) při běží a poskytuje tooabort možnost ho.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-123">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span>

![Příjem příchozích dat funkci spouštění](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

<span data-ttu-id="2e7ae-125">Když funkce hello dokončení hello **volání podrobnosti** stránky sestavy hello počet řádků, které jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-125">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Funkce pro příjem příchozích dat bylo dokončeno](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a><span data-ttu-id="2e7ae-127">Jak tooread více entit z tabulky.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-127">How tooread multiple entities from a table</span></span>
<span data-ttu-id="2e7ae-128">tooread tabulky, použijte hello **tabulky** atribut s **IQueryable<T>**  parametr kde zadejte **T** je odvozena z **TableEntity**nebo implementuje **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-128">tooread a table, use hello **Table** attribute with an **IQueryable<T>** parameter where type **T** derives from **TableEntity** or implements **ITableEntity**.</span></span>

<span data-ttu-id="2e7ae-129">Hello následující ukázka kódu čte a zaznamená všechny řádky z hello **příjem příchozích dat** tabulky:</span><span class="sxs-lookup"><span data-stu-id="2e7ae-129">hello following code sample reads and logs all rows from hello **Ingress** table:</span></span>

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

### <a name="how-tooread-a-single-entity-from-a-table"></a><span data-ttu-id="2e7ae-130">Jak tooread jedné entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-130">How tooread a single entity from a table</span></span>
<span data-ttu-id="2e7ae-131">Je **tabulky** konstruktoru atributu s dva další parametry, které umožňují určit hello klíč oddílu a klíč řádku, pokud chcete toobind tooa jedné tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-131">There is a **Table** attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="2e7ae-132">Hello následující ukázka kódu čte pro řádek tabulky **osoba** entit na základě oddílu klíč a řádek hodnot klíče dostali zprávu fronty:</span><span class="sxs-lookup"><span data-stu-id="2e7ae-132">hello following code sample reads a table row for a **Person** entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="2e7ae-133">Hello **osoba** třídy v tomto příkladu nemá tooimplement **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-133">hello **Person** class in this example does not have tooimplement **ITableEntity**.</span></span>

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a><span data-ttu-id="2e7ae-134">Jak toouse hello .NET API úložiště přímo toowork s tabulkou</span><span class="sxs-lookup"><span data-stu-id="2e7ae-134">How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="2e7ae-135">Můžete taky hello **tabulky** atribut s **CloudTable** objekt pro větší flexibilitu při práci s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-135">You can also use hello **Table** attribute with a **CloudTable** object for more flexibility in working with a table.</span></span>

<span data-ttu-id="2e7ae-136">Hello následující kód používá ukázka **CloudTable** objektu tooadd jedné entity toohello *příjem příchozích dat* tabulky.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-136">hello following code sample uses a **CloudTable** object tooadd a single entity toohello *Ingress* table.</span></span>

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

<span data-ttu-id="2e7ae-137">Další informace o tom, toouse hello **CloudTable** objektu, najdete v části [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="2e7ae-137">For more information about how toouse hello **CloudTable** object, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a><span data-ttu-id="2e7ae-138">Související témata předmětem hello fronty postupy tooarticle</span><span class="sxs-lookup"><span data-stu-id="2e7ae-138">Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="2e7ae-139">Informace o tom, jak zpracování tabulky toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není zpracování, najdete v části konkrétní tootable [Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty) ](vs-storage-webjobs-getting-started-queues.md).</span><span class="sxs-lookup"><span data-stu-id="2e7ae-139">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)](vs-storage-webjobs-getting-started-queues.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e7ae-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e7ae-140">Next steps</span></span>
<span data-ttu-id="2e7ae-141">Tento článek poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="2e7ae-141">This article has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="2e7ae-142">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="2e7ae-142">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

