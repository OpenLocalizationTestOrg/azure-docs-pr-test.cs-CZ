---
title: "aaaAzure Cosmos DB škálování a testování výkonu | Microsoft Docs"
description: "Zjistěte, jak tooperform škálování a výkon testování pomocí Azure Cosmos DB"
keywords: "Testování výkonu"
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: arramac
ms.openlocfilehash: 46d1217e11a39ee970a868de9a5c5dfcf52cedf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a><span data-ttu-id="14f67-104">Výkonu a možností škálování testování pomocí Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="14f67-104">Performance and scale testing with Azure Cosmos DB</span></span>
<span data-ttu-id="14f67-105">Testování výkonu a škálování je klíče krok při vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-105">Performance and scale testing is a key step in application development.</span></span> <span data-ttu-id="14f67-106">Mnoho aplikací hello databázové vrstvy, má značný vliv hello celkový výkon a škálovatelnost, a je proto důležité součást výkonu testování.</span><span class="sxs-lookup"><span data-stu-id="14f67-106">For many applications, hello database tier has a significant impact on hello overall performance and scalability, and is therefore a critical component of performance testing.</span></span> <span data-ttu-id="14f67-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je vytvořeného pro tento účel pro elastické škálování a předvídatelný výkon a proto skvělé přizpůsobit pro aplikace, které potřebují a vysoce výkonné databázové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="14f67-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier.</span></span> 

<span data-ttu-id="14f67-108">Tento článek je odkaz pro vývojáře implementace sad testů výkonu pro zatížení Cosmos DB nebo vyhodnocení Cosmos DB pro vysoce výkonné aplikace scénáře.</span><span class="sxs-lookup"><span data-stu-id="14f67-108">This article is a reference for developers implementing performance test suites for their Cosmos DB workloads, or evaluating Cosmos DB for high-performance application scenarios.</span></span> <span data-ttu-id="14f67-109">Se zaměřují hlavně na test výkonnosti izolované databáze hello, ale také obsahuje osvědčené postupy pro výrobní aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-109">It focuses primarily on isolated performance testing of hello database, but also includes best practices for production applications.</span></span>

<span data-ttu-id="14f67-110">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="14f67-110">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="14f67-111">Kde najdu ukázkovou aplikaci klienta rozhraní .NET pro testování výkonu databáze Cosmos?</span><span class="sxs-lookup"><span data-stu-id="14f67-111">Where can I find a sample .NET client application for performance testing of Cosmos DB?</span></span> 
* <span data-ttu-id="14f67-112">Jak dosáhnout vysoké propustnosti úrovně s Cosmos DB z mé aplikace klienta?</span><span class="sxs-lookup"><span data-stu-id="14f67-112">How do I achieve high throughput levels with Cosmos DB from my client application?</span></span>

<span data-ttu-id="14f67-113">tooget spuštění s kódem, stáhněte si prosím hello projekt z [Azure Cosmos DB výkonu testování Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="14f67-113">tooget started with code, please download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span></span> 

> [!NOTE]
> <span data-ttu-id="14f67-114">Hello cílem této aplikace je toodemonstrate osvědčené postupy pro extrahování lepší výkon mimo Cosmos DB s malý počet klientských počítačů.</span><span class="sxs-lookup"><span data-stu-id="14f67-114">hello goal of this application is toodemonstrate best practices for extracting better performance out of Cosmos DB with a small number of client machines.</span></span> <span data-ttu-id="14f67-115">To nebyl proveden toodemonstrate hello ve špičce kapacity hello služby, který můžete škálovat neomezeně rozšiřovat.</span><span class="sxs-lookup"><span data-stu-id="14f67-115">This was not made toodemonstrate hello peak capacity of hello service, which can scale limitlessly.</span></span>
> 
> 

<span data-ttu-id="14f67-116">Pokud hledáte tooimprove možnosti konfigurace na straně klienta Cosmos DB výkonu, najdete v části [tipy pro zvýšení výkonu Azure Cosmos DB](performance-tips.md).</span><span class="sxs-lookup"><span data-stu-id="14f67-116">If you're looking for client-side configuration options tooimprove Cosmos DB performance, see [Azure Cosmos DB performance tips](performance-tips.md).</span></span>

## <a name="run-hello-performance-testing-application"></a><span data-ttu-id="14f67-117">Spuštění testování aplikace hello výkonu</span><span class="sxs-lookup"><span data-stu-id="14f67-117">Run hello performance testing application</span></span>
<span data-ttu-id="14f67-118">Hello tooget nejrychlejší způsob, jak spustit je toocompile a spuštění hello .NET ukázková níže, jak je popsáno v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-118">hello quickest way tooget started is toocompile and run hello .NET sample below, as described in hello steps below.</span></span> <span data-ttu-id="14f67-119">Můžete také zkontrolovat hello zdrojového kódu a implementovat podobné konfigurace tooyour vlastní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-119">You can also review hello source code and implement similar configurations tooyour own client applications.</span></span>

<span data-ttu-id="14f67-120">**Krok 1:** projekt hello stahování z [Azure Cosmos DB výkonu testování Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), nebo úložiště GitHub rozvětvení hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-120">**Step 1:** Download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork hello GitHub repository.</span></span>

<span data-ttu-id="14f67-121">**Krok 2:** upravit hello nastavení adresa URL koncového bodu, authorizationkey tak, CollectionThroughput a DocumentTemplate (volitelné) v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="14f67-121">**Step 2:** Modify hello settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.</span></span>

> [!NOTE]
> <span data-ttu-id="14f67-122">Než kolekce s vysokou propustnost, naleznete v toohello [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello náklady na kolekci.</span><span class="sxs-lookup"><span data-stu-id="14f67-122">Before provisioning collections with high throughput, please refer toohello [Pricing Page](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello costs per collection.</span></span> <span data-ttu-id="14f67-123">Azure účtuje poplatky za úložiště Cosmos databáze a propustnost nezávisle na hodinu, takže odstraněním nebo snížení hello propustnost vaší kolekce Azure Cosmos DB po testování můžete uložit náklady.</span><span class="sxs-lookup"><span data-stu-id="14f67-123">Azure Cosmos DB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering hello throughput of your Azure Cosmos DB collections after testing.</span></span>
> 
> 

<span data-ttu-id="14f67-124">**Krok 3:** zkompilování a spuštění z příkazového řádku hello hello konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="14f67-124">**Step 3:** Compile and run hello console app from hello command line.</span></span> <span data-ttu-id="14f67-125">Měli byste vidět výstup podobný hello následující:</span><span class="sxs-lookup"><span data-stu-id="14f67-125">You should see output like hello following:</span></span>

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


<span data-ttu-id="14f67-126">**Krok 4 (v případě potřeby):** hello propustnost hlášené (RU/s) z nástroje hello by měla být stejná nebo vyšší než hello zřízené propustnosti hello kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-126">**Step 4 (if necessary):** hello throughput reported (RU/s) from hello tool should be hello same or higher than hello provisioned throughput of hello collection.</span></span> <span data-ttu-id="14f67-127">V opačném případě se zvyšující hello DegreeOfParallelism v malých přírůstcích mohou pomoci při dosažení limitu hello.</span><span class="sxs-lookup"><span data-stu-id="14f67-127">If not, increasing hello DegreeOfParallelism in small increments may help you reach hello limit.</span></span> <span data-ttu-id="14f67-128">Pokud náhorních plošinách hello propustnost z vaší klientské aplikace, spouštění více instancí aplikace hello na hello stejné nebo různé počítače vám pomůže dosáhnout limit hello zřídit na hello různé instance.</span><span class="sxs-lookup"><span data-stu-id="14f67-128">If hello throughput from your client app plateaus, launching multiple instances of hello app on hello same or different machines will help you reach hello provisioned limit across hello different instances.</span></span> <span data-ttu-id="14f67-129">Pokud potřebujete pomoc s Tento krok, prosím, zápis e-mailu tooaskcosmosdb@microsoft.com nebo soubor lístek podpory z hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14f67-129">If you need help with this step, please, write an email tooaskcosmosdb@microsoft.com or file a support ticket from hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="14f67-130">Jakmile máte hello aplikaci spuštěnou, můžete zkusit jiný [indexování zásady](indexing-policies.md) a [úrovně konzistence](consistency-levels.md) toounderstand jejich dopad na propustnosti a latence.</span><span class="sxs-lookup"><span data-stu-id="14f67-130">Once you have hello app running, you can try different [Indexing policies](indexing-policies.md) and [Consistency levels](consistency-levels.md) toounderstand their impact on throughput and latency.</span></span> <span data-ttu-id="14f67-131">Můžete také zkontrolovat hello zdrojového kódu a implementovat podobné konfigurace tooyour vlastní testovacích sad nebo výrobní aplikace.</span><span class="sxs-lookup"><span data-stu-id="14f67-131">You can also review hello source code and implement similar configurations tooyour own test suites or production applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14f67-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14f67-132">Next steps</span></span>
<span data-ttu-id="14f67-133">V tomto článku jsme se podívali na tom, jak můžete provádět výkonu a možností škálování testování pomocí DB Cosmos pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="14f67-133">In this article, we looked at how you can perform performance and scale testing with Cosmos DB using a .NET console app.</span></span> <span data-ttu-id="14f67-134">Další informace o práci s Azure Cosmos DB naleznete toohello najdete pod odkazy níže.</span><span class="sxs-lookup"><span data-stu-id="14f67-134">Please refer toohello links below for additional information on working with Azure Cosmos DB.</span></span>

* [<span data-ttu-id="14f67-135">Testování ukázkové Azure Cosmos DB výkonu</span><span class="sxs-lookup"><span data-stu-id="14f67-135">Azure Cosmos DB performance testing sample</span></span>](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [<span data-ttu-id="14f67-136">Tooimprove možnosti konfigurace klienta Azure Cosmos DB výkonu</span><span class="sxs-lookup"><span data-stu-id="14f67-136">Client configuration options tooimprove Azure Cosmos DB performance</span></span>](performance-tips.md)
* [<span data-ttu-id="14f67-137">Dělení na straně serveru v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="14f67-137">Server-side partitioning in Azure Cosmos DB</span></span>](partition-data.md)


