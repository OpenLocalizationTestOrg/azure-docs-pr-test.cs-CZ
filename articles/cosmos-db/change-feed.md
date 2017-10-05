---
title: "Práce se změnami kanálu podpory v Azure Cosmos DB | Microsoft Docs"
description: "Použijte Azure Cosmos DB změnu informačního kanálu podporu sledování změn v dokumentech a provádět na základě událostí zpracování jako aktivační události a průběžná aktualizace mezipaměti a analýzy systémy."
keywords: "Změna kanálu"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 160fbc98e0f3dcc7d17cbe0c7f7425811596a896
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a><span data-ttu-id="c4fb4-104">Práce se změnami kanálu podpory v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c4fb4-104">Working with the change feed support in Azure Cosmos DB</span></span>
<span data-ttu-id="c4fb4-105">[Azure Cosmos DB](../cosmos-db/introduction.md) je rychlého a flexibilní globálně replikované databáze služby, který slouží k ukládání velkých objemů dat transakcí a funkční s latencí předvídatelný jednociferné milisekund pro čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is a fast and flexible globally replicated database service that is used for storing high-volume transactional and operational data with predictable single-digit millisecond latency for reads and writes.</span></span> <span data-ttu-id="c4fb4-106">Díky tomu dobře hodí pro IoT, hry, maloobchodní a provozní protokolování aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-106">This makes it well-suited for IoT, gaming, retail, and operational logging applications.</span></span> <span data-ttu-id="c4fb4-107">Sledovat změny provedené v Azure Cosmos DB dat a aktualizovat materializovaným zobrazením, provádět analýzu v reálném čase, archivaci dat na studené úložiště a aktivovat oznámení na určité události na základě těchto změn je běžný vzor návrhu v těchto aplikacích.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-107">A common design pattern in these applications is to track changes made to Azure Cosmos DB data, and update materialized views, perform real-time analytics, archive data to cold storage, and trigger notifications on certain events based on these changes.</span></span> <span data-ttu-id="c4fb4-108">**Změnu kanálu podporu** v Azure Cosmos DB umožňuje vytvářet efektivní a škálovatelné řešení pro každou z těchto vzorků.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-108">The **change feed support** in Azure Cosmos DB enables you to build efficient and scalable solutions for each of these patterns.</span></span>

<span data-ttu-id="c4fb4-109">Změny kanálu podpory poskytuje Azure Cosmos DB seřazený seznam dokumenty v kolekci Azure Cosmos DB v pořadí, ve kterém byly upraveny.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-109">With change feed support, Azure Cosmos DB provides a sorted list of documents within an Azure Cosmos DB collection in the order in which they were modified.</span></span> <span data-ttu-id="c4fb4-110">Tento informační kanál lze použít k naslouchání změny dat v rámci kolekce a provádět akce, jako:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-110">This feed can be used to listen for modifications to data within the collection and perform actions such as:</span></span>

* <span data-ttu-id="c4fb4-111">Aktivovat volání rozhraní API, kdy je dokument vložit nebo úpravě</span><span class="sxs-lookup"><span data-stu-id="c4fb4-111">Trigger a call to an API when a document is inserted or modified</span></span>
* <span data-ttu-id="c4fb4-112">Na aktualizace provést zpracování v reálném čase (proud)</span><span class="sxs-lookup"><span data-stu-id="c4fb4-112">Perform real-time (stream) processing on updates</span></span>
* <span data-ttu-id="c4fb4-113">Synchronizaci dat s mezipaměti, vyhledávací web nebo datového skladu</span><span class="sxs-lookup"><span data-stu-id="c4fb4-113">Synchronize data with a cache, search engine, or data warehouse</span></span>

<span data-ttu-id="c4fb4-114">Změny v Azure Cosmos DB jsou nastavené jako trvalé může být zpracována asynchronně a distribuovaná do jednoho nebo více spotřebitelů pro paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-114">Changes in Azure Cosmos DB are persisted and can be processed asynchronously, and distributed across one or more consumers for parallel processing.</span></span> <span data-ttu-id="c4fb4-115">Podívejme se na rozhraní API pro změnu kanálu a jak je můžete použít k vytváření škálovatelné aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-115">Let's look at the APIs for change feed and how you can use them to build scalable real-time applications.</span></span> <span data-ttu-id="c4fb4-116">Tento článek ukazuje, jak pracovat s Azure Cosmos DB změn kanálu a rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-116">This article shows how to work with Azure Cosmos DB change feed and the DocumentDB API.</span></span> 

![Pomocí Azure Cosmos DB změnu kanálu power analýzu v reálném čase a událostmi řízené výpočetní scénáře](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> <span data-ttu-id="c4fb4-118">Změna kanálu podpora je k dispozici pouze pro rozhraní API DocumentDB v tuto chvíli; rozhraní Graph API a rozhraní API tabulky nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-118">Change feed support is only provided for the DocumentDB API at this time; the Graph API and Table API are not currently supported.</span></span>

## <a name="use-cases-and-scenarios"></a><span data-ttu-id="c4fb4-119">Případy použití a scénáře</span><span class="sxs-lookup"><span data-stu-id="c4fb4-119">Use cases and scenarios</span></span>
<span data-ttu-id="c4fb4-120">Změna kanálu umožňuje efektivní zpracování rozsáhlých datových sad k velkému počtu zápisy a nabízí alternativu k dotazování celé datové sady pro identifikaci, co se změnilo.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-120">Change feed allows for efficient processing of large datasets with a high volume of writes, and offers an alternative to querying entire datasets to identify what has changed.</span></span> <span data-ttu-id="c4fb4-121">Například můžete provádět následující úlohy efektivně:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-121">For example, you can perform the following tasks efficiently:</span></span>

* <span data-ttu-id="c4fb4-122">Aktualizace mezipaměti, index vyhledávání nebo datového skladu s daty uloženými v databázi Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-122">Update a cache, search index, or a data warehouse with data stored in Azure Cosmos DB.</span></span>
* <span data-ttu-id="c4fb4-123">Aplikační úrovni využití dat vrstvení a archivaci, tedy ukládání "horkých dat." v Azure Cosmos DB a po určité době odstraněny "pomaleji přístupná data" k [Azure Blob Storage](../storage/common/storage-introduction.md) nebo [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-123">Implement application-level data tiering and archival, that is, store "hot data" in Azure Cosmos DB, and age out "cold data" to [Azure Blob Storage](../storage/common/storage-introduction.md) or [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span></span>
* <span data-ttu-id="c4fb4-124">Implementace batch analytics na data pomocí [Apache Hadoop](run-hadoop-with-hdinsight.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-124">Implement batch analytics on data using [Apache Hadoop](run-hadoop-with-hdinsight.md).</span></span>
* <span data-ttu-id="c4fb4-125">Implementace [lambda kanály v Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-125">Implement [lambda pipelines on Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) with Azure Cosmos DB.</span></span> <span data-ttu-id="c4fb4-126">Azure Cosmos DB poskytuje řešení škálovatelná databáze, které může zpracovat přijímání a dotazů a implementovat lambda architektury s nízkou celkové náklady na vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-126">Azure Cosmos DB provides a scalable database solution that can handle both ingestion and query, and implement lambda architectures with low TCO.</span></span> 
* <span data-ttu-id="c4fb4-127">Provést nulové době migrací na jiný účet Azure Cosmos DB jiné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-127">Perform zero down-time migrations to another Azure Cosmos DB account with a different partitioning scheme.</span></span>

<span data-ttu-id="c4fb4-128">**Lambda kanálů s Azure DB Cosmos pro přijímání a dotazů:**</span><span class="sxs-lookup"><span data-stu-id="c4fb4-128">**Lambda Pipelines with Azure Cosmos DB for ingestion and query:**</span></span>

![Azure Cosmos DB na základě lambda kanálu pro přijímání a dotazů](./media/change-feed/lambda.png)

<span data-ttu-id="c4fb4-130">Můžete použít Azure Cosmos DB přijmout a ukládání dat události ze zařízení, senzorů, infrastruktury a aplikace a zpracování těchto událostí v reálném čase pomocí [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), nebo [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-130">You can use Azure Cosmos DB to receive and store event data from devices, sensors, infrastructure, and applications, and process these events in real-time with [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), or [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span></span> 

<span data-ttu-id="c4fb4-131">V rámci vaší [bez serveru](http://azure.com/serverless) webových a mobilních aplikací, můžete sledovat události, jako jsou například změny do vašeho zákazníka profilu, předvolby nebo umístění pro spuštění určitých akcí, jako je odesílání nabízených oznámení do jejich zařízení pomocí [ Azure Functions](../azure-functions/functions-bindings-documentdb.md) nebo [aplikační služby](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-131">Within your [serverless](http://azure.com/serverless) web and mobile apps, you can track events such as changes to your customer's profile, preferences, or location to trigger certain actions like sending push notifications to their devices using [Azure Functions](../azure-functions/functions-bindings-documentdb.md) or [App Services](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="c4fb4-132">Pokud používáte databázi Cosmos Azure k vytvoření hry, můžete, například použití změnit informačního kanálu v reálném čase tabulky podle skóre z dokončené hry implementovat.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-132">If you're using Azure Cosmos DB to build a game, you can, for example, use change feed to implement real-time leaderboards based on scores from completed games.</span></span>

## <a name="how-change-feed-works-in-azure-cosmos-db"></a><span data-ttu-id="c4fb4-133">Jak funguje změnu informační kanál v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c4fb4-133">How change feed works in Azure Cosmos DB</span></span>
<span data-ttu-id="c4fb4-134">Azure Cosmos DB poskytuje možnost číst přírůstkové aktualizace provedené na kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-134">Azure Cosmos DB provides the ability to incrementally read updates made to an Azure Cosmos DB collection.</span></span> <span data-ttu-id="c4fb4-135">Tento informační kanál změnu má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-135">This change feed has the following properties:</span></span>

* <span data-ttu-id="c4fb4-136">Změny jsou trvalé v Azure Cosmos DB a může být zpracována asynchronně.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-136">Changes are persistent in Azure Cosmos DB and can be processed asynchronously.</span></span>
* <span data-ttu-id="c4fb4-137">Jsou k dispozici okamžitě změnu kanálu změny do dokumentů v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-137">Changes to documents within a collection are available immediately in the change feed.</span></span>
* <span data-ttu-id="c4fb4-138">Každé změně do dokumentu se zobrazí právě jednou v změnu kanálu a klienti spravovat své logiku vytváření kontrolních bodů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-138">Each change to a document appears exactly once in the change feed, and clients manage their checkpointing logic.</span></span> <span data-ttu-id="c4fb4-139">Knihovna informačního kanálu procesoru změn poskytuje automatické vytváření kontrolních bodů a "alespoň jednou" sémantiku.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-139">The change feed processor library provides automatic checkpointing and "at least once" semantics.</span></span>
* <span data-ttu-id="c4fb4-140">Protokol změn je součástí pouze poslední změny u daného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-140">Only the most recent change for a given document is included in the change log.</span></span> <span data-ttu-id="c4fb4-141">Přechodných změn nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-141">Intermediate changes may not be available.</span></span>
* <span data-ttu-id="c4fb4-142">Změna informačního kanálu je řazen u úpravy v rámci každé hodnotu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-142">The change feed is sorted by order of modification within each partition key value.</span></span> <span data-ttu-id="c4fb4-143">Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-143">There is no guaranteed order across partition-key values.</span></span>
* <span data-ttu-id="c4fb4-144">Změny mohou být synchronizovány z jakékoli bodu v čase, tedy neexistuje žádné období uchovávání pevné dat, pro které jsou k dispozici změny.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-144">Changes can be synchronized from any point-in-time, that is, there is no fixed data retention period for which changes are available.</span></span>
* <span data-ttu-id="c4fb4-145">Změny jsou k dispozici v bloky rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-145">Changes are available in chunks of partition key ranges.</span></span> <span data-ttu-id="c4fb4-146">Tato možnost umožňuje změny z rozsáhlých kolekcí, které mají být zpracovány současně více příjemci nebo servery.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-146">This capability allows changes from large collections to be processed in parallel by multiple consumers/servers.</span></span>
* <span data-ttu-id="c4fb4-147">Aplikace může požadovat pro více informačních kanálů změnu současně na stejné kolekci.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-147">Applications can request for multiple change feeds simultaneously on the same collection.</span></span>

<span data-ttu-id="c4fb4-148">Informační kanál Azure Cosmos DB změnu je povoleno ve výchozím nastavení pro všechny účty.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-148">Azure Cosmos DB's change feed is enabled by default for all accounts.</span></span> <span data-ttu-id="c4fb4-149">Můžete použít vaše [zřízené propustnosti](request-units.md) ve vaší oblasti zápisu ani žádné [číst oblast](distribute-data-globally.md) číst z změnu kanálu, stejně jako všechny ostatní operace z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-149">You can use your [provisioned throughput](request-units.md) in your write region or any [read region](distribute-data-globally.md) to read from the change feed, just like any other operation from Azure Cosmos DB.</span></span> <span data-ttu-id="c4fb4-150">Informační kanál změnu zahrnuje vložení a operace aktualizace provedené na dokumenty v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-150">The change feed includes inserts and update operations made to documents within the collection.</span></span> <span data-ttu-id="c4fb4-151">Můžete zaznamenat odstranění nastavením příznak "soft odstranění" v rámci dokumentů místo odstranění.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-151">You can capture deletes by setting a "soft-delete" flag within your documents in place of deletes.</span></span> <span data-ttu-id="c4fb4-152">Alternativně můžete nastavit konečný platnosti pro dokumentů prostřednictvím [TTL schopností](time-to-live.md), například 24 hodin a použití hodnota této vlastnosti k zachycení odstranění.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-152">Alternatively, you can set a finite expiration period for your documents via the [TTL capability](time-to-live.md), for example, 24 hours and use the value of that property to capture deletes.</span></span> <span data-ttu-id="c4fb4-153">S tímto řešením je nutné zpracovat změny v časovém intervalu kratší než doba TTL vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-153">With this solution, you have to process changes within a shorter time interval than the TTL expiration period.</span></span> <span data-ttu-id="c4fb4-154">Změna informačního kanálu je k dispozici pro každý oddíl klíče rozsahem v rámci kolekce dokumentů a proto mohou být distribuovány na jeden nebo více příjemce pro paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-154">The change feed is available for each partition key range within the document collection, and thus can be distributed across one or more consumers for parallel processing.</span></span> 

![Distribuované zpracování změn Azure Cosmos DB kanálu](./media/change-feed/changefeedvisual.png)

<span data-ttu-id="c4fb4-156">Máte několik možností v tom, jak implementovat změnu kanálu v klientském kódu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-156">You have a few options in how you implement a change feed in your client code.</span></span> <span data-ttu-id="c4fb4-157">Okamžitě následujících částech popisují, jak k implementaci změny kanálu pomocí rozhraní REST API Azure Cosmos DB a DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-157">The sections that immediately follow describe how to implement the change feed using the Azure Cosmos DB REST API and the DocumentDB SDKs.</span></span> <span data-ttu-id="c4fb4-158">Ale pro aplikace .NET, doporučujeme používat novou [změnu kanálu procesoru knihovna](#change-feed-processor) pro zpracování události z změnu kanálu tak, jak zjednodušuje čtení změny mezi oddílů a umožňuje více podprocesů práce paralelní.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-158">However, for .NET applications, we recommend using the new [Change feed processor library](#change-feed-processor) for processing events from the change feed as it simplifies reading changes across partitions and enables multiple threads working in parallel.</span></span> 

## <span data-ttu-id="c4fb4-159"><a id="rest-apis"></a>Práce s rozhraní REST API a DocumentDB SDK</span><span class="sxs-lookup"><span data-stu-id="c4fb4-159"><a id="rest-apis"></a>Working with the REST API and DocumentDB SDKs</span></span>
<span data-ttu-id="c4fb4-160">Azure Cosmos DB poskytuje elastické kontejnerů úložiště a propustnost názvem **kolekce**.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-160">Azure Cosmos DB provides elastic containers of storage and throughput called **collections**.</span></span> <span data-ttu-id="c4fb4-161">Data v rámci kolekce je logicky seskupeny pomocí [oddílu klíče](partition-data.md) a výkon a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-161">Data within collections is logically grouped using [partition keys](partition-data.md) for scalability and performance.</span></span> <span data-ttu-id="c4fb4-162">Azure Cosmos DB poskytuje různé rozhraní API pro přístup k těmto datům, včetně vyhledávání podle ID (pro čtení nebo získat), dotazů a číst informační kanály (kontroly).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-162">Azure Cosmos DB provides various APIs for accessing this data, including lookup by ID (Read/Get), query, and read-feeds (scans).</span></span> <span data-ttu-id="c4fb4-163">Změna informačního kanálu je možné získat naplnění dva nové hlavičky požadavku k DocumentDB `ReadDocumentFeed` rozhraní API, může zpracovat paralelní napříč rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-163">The change feed can be obtained by populating two new request headers to the DocumentDB `ReadDocumentFeed` API, and can be processed in parallel across ranges of partition keys.</span></span>

### <a name="readdocumentfeed-api"></a><span data-ttu-id="c4fb4-164">ReadDocumentFeed rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c4fb4-164">ReadDocumentFeed API</span></span>
<span data-ttu-id="c4fb4-165">Podívejme se stručný v tom, jak funguje ReadDocumentFeed.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-165">Let's take a brief look at how ReadDocumentFeed works.</span></span> <span data-ttu-id="c4fb4-166">Azure Cosmos DB podporuje čtení dokumentů v rámci kolekce prostřednictvím informačního kanálu `ReadDocumentFeed` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-166">Azure Cosmos DB supports reading a feed of documents within a collection via the `ReadDocumentFeed` API.</span></span> <span data-ttu-id="c4fb4-167">Například následující požadavek vrátí stránku dokumenty uvnitř `serverlogs` kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-167">For example, the following request returns a page of documents inside the `serverlogs` collection.</span></span> 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="c4fb4-168">Výsledky mohou být omezeny pomocí `x-ms-max-item-count` záhlaví a čtení lze obnovit pomocí opětovným odesláním požadavek s `x-ms-continuation` záhlaví, vrátí se v předchozí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-168">Results can be limited by using the `x-ms-max-item-count` header, and reads can be resumed by resubmitting the request with a `x-ms-continuation` header returned in the previous response.</span></span> <span data-ttu-id="c4fb4-169">Při provádění z jednoho klienta, `ReadDocumentFeed` iteruje výsledky napříč oddíly sériově.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-169">When performed from a single client, `ReadDocumentFeed` iterates through results across partitions serially.</span></span> 

<span data-ttu-id="c4fb4-170">**Sériové čtení dokumentu kanálu**</span><span class="sxs-lookup"><span data-stu-id="c4fb4-170">**Serial read document feed**</span></span>

<span data-ttu-id="c4fb4-171">Můžete také načíst informační kanál dokumentů pomocí jedné z podporovaném [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-171">You can also retrieve the feed of documents using one of the supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="c4fb4-172">Například následující fragment kódu ukazuje způsob použití [ReadDocumentFeedAsync metoda](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-172">For example, the following snippet shows how to use the [ReadDocumentFeedAsync method](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) in .NET.</span></span>

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a><span data-ttu-id="c4fb4-173">Distribuované provádění ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="c4fb4-173">Distributed execution of ReadDocumentFeed</span></span>
<span data-ttu-id="c4fb4-174">Pro kolekce, které obsahují terabajtů dat nebo víc nebo ingestování velký objem aktualizací nemusí být praktické sériové provádění čtení kanálu z jednoho klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-174">For collections that contain terabytes of data or more, or ingest a large volume of updates, serial execution of read feed from a single client machine might not be practical.</span></span> <span data-ttu-id="c4fb4-175">Aby bylo možné podporovat scénáře velkých objemů dat, Azure Cosmos DB poskytuje rozhraní API pro distribuci `ReadDocumentFeed` volání transparentně napříč více klienta čtečky na příjemců.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-175">In order to support these big data scenarios, Azure Cosmos DB provides APIs to distribute `ReadDocumentFeed` calls transparently across multiple client readers/consumers.</span></span> 

<span data-ttu-id="c4fb4-176">**Informační kanál pro distribuované čtení dokumentu**</span><span class="sxs-lookup"><span data-stu-id="c4fb4-176">**Distributed Read Document Feed**</span></span>

<span data-ttu-id="c4fb4-177">Zajistit škálovatelné zpracování přírůstkové změny Azure Cosmos DB podporuje model Škálováním na více systémů pro změnu kanálu rozhraní API založené na rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-177">To provide scalable processing of incremental changes, Azure Cosmos DB supports a scale-out model for the change feed API based on ranges of partition keys.</span></span>

* <span data-ttu-id="c4fb4-178">Můžete získat seznam oddílu rozsahy klíčů pro kolekci provádění `ReadPartitionKeyRanges` volání.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-178">You can obtain a list of partition key ranges for a collection performing a `ReadPartitionKeyRanges` call.</span></span> 
* <span data-ttu-id="c4fb4-179">Pro každý rozsah klíče oddílu můžete provádět `ReadDocumentFeed` čtení dokumentů pomocí klíčů oddílů v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-179">For each partition key range, you can perform a `ReadDocumentFeed` to read documents with partition keys within that range.</span></span>

### <a name="retrieving-partition-key-ranges-for-a-collection"></a><span data-ttu-id="c4fb4-180">Načítání oblastí klíče oddílu pro kolekci</span><span class="sxs-lookup"><span data-stu-id="c4fb4-180">Retrieving partition key ranges for a collection</span></span>
<span data-ttu-id="c4fb4-181">Rozsahy klíčů oddílu můžete načíst tím, že požádá `pkranges` prostředků v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-181">You can retrieve the partition key ranges by requesting the `pkranges` resource within a collection.</span></span> <span data-ttu-id="c4fb4-182">Například následující požadavek načte seznam rozsahy klíče oddílu `serverlogs` kolekce:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-182">For example the following request retrieves the list of partition key ranges for the `serverlogs` collection:</span></span>

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

<span data-ttu-id="c4fb4-183">Tento požadavek vrátí následující odpověď obsahující metadata o rozsahy klíče oddílu:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-183">This request returns the following response containing metadata about the partition key ranges:</span></span>

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


<span data-ttu-id="c4fb4-184">**Oddílu Vlastnosti klíčové oblasti**: každý rozsah klíče oddílů zahrnují metadata vlastnosti v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-184">**Partition key range properties**: Each partition key range includes the metadata properties in the following table:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="c4fb4-185">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="c4fb4-185">Header name</span></span></th>
        <th><span data-ttu-id="c4fb4-186">Popis</span><span class="sxs-lookup"><span data-stu-id="c4fb4-186">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-187">id</span><span class="sxs-lookup"><span data-stu-id="c4fb4-187">id</span></span></td>
        <td>
            <p><span data-ttu-id="c4fb4-188">ID pro rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-188">The ID for the partition key range.</span></span> <span data-ttu-id="c4fb4-189">Toto je stabilní a jedinečné ID v každé kolekci.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-189">This is a stable and unique ID within each collection.</span></span></p>
            <p><span data-ttu-id="c4fb4-190">Musí být použít v následující volání ke čtení změny podle rozsahu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-190">Must be used in the following call to read changes by partition key range.</span></span></p>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-191">maxExclusive</span><span class="sxs-lookup"><span data-stu-id="c4fb4-191">maxExclusive</span></span></td>
        <td><span data-ttu-id="c4fb4-192">Hodnota maximální oddílu hodnota hash klíče pro rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-192">The maximum partition key hash value for the partition key range.</span></span> <span data-ttu-id="c4fb4-193">Pro interní použití.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-193">For internal use.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-194">minInclusive</span><span class="sxs-lookup"><span data-stu-id="c4fb4-194">minInclusive</span></span></td>
        <td><span data-ttu-id="c4fb4-195">Hodnota hash klíče minimální oddílu pro rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-195">The minimum partition key hash value for the partition key range.</span></span> <span data-ttu-id="c4fb4-196">Pro interní použití.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-196">For internal use.</span></span></td>
    </tr>       
</table>

<span data-ttu-id="c4fb4-197">To provedete pomocí jedné z podporovaném [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-197">You can do this using one of the supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="c4fb4-198">Například následující fragment kódu ukazuje, jak načíst rozsahy klíčů oddílu v rozhraní .NET pomocí [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metoda.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-198">For example, the following snippet shows how to retrieve partition key ranges in .NET using the [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) method.</span></span>

```csharp
string pkRangesResponseContinuation = null;
List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

do
{
    FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri, 
        new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
}
while (pkRangesResponseContinuation != null);
```

<span data-ttu-id="c4fb4-199">Azure Cosmos DB podporuje načtení dokumentů na rozsah klíče oddílu pomocí nastavení volitelné `x-ms-documentdb-partitionkeyrangeid` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-199">Azure Cosmos DB supports retrieval of documents per partition key range by setting the optional `x-ms-documentdb-partitionkeyrangeid` header.</span></span> 

### <a name="performing-an-incremental-readdocumentfeed"></a><span data-ttu-id="c4fb4-200">Provádění přírůstkové ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="c4fb4-200">Performing an incremental ReadDocumentFeed</span></span>
<span data-ttu-id="c4fb4-201">ReadDocumentFeed podporuje následující scénáře a úkoly pro přírůstkové zpracování změny v kolekcích Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-201">ReadDocumentFeed supports the following scenarios/tasks for incremental processing of changes in Azure Cosmos DB collections:</span></span>

* <span data-ttu-id="c4fb4-202">Číst všechny změny do dokumentů od začátku, tedy z vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-202">Read all changes to documents from the beginning, that is, from collection creation.</span></span>
* <span data-ttu-id="c4fb4-203">Číst všechny změny pro budoucí aktualizace do dokumentů z aktuálního času nebo změny od čas zadaného uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-203">Read all changes to future updates to documents from current time, or any changes since a user-specified time.</span></span>
* <span data-ttu-id="c4fb4-204">Číst všechny změny do dokumentů z verze logické kolekce (ETag).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-204">Read all changes to documents from a logical version of the collection (ETag).</span></span> <span data-ttu-id="c4fb4-205">Kontrolní bod můžete uživatele podle vrácená značka ETag z přírůstkové žádostí kanálu pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-205">You can checkpoint your consumers based on the returned ETag from incremental read-feed requests.</span></span>

<span data-ttu-id="c4fb4-206">Změny zahrnují vložení a aktualizace do dokumentů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-206">The changes include inserts and updates to documents.</span></span> <span data-ttu-id="c4fb4-207">Můžete zaznamenat odstranění, musí používat vlastnost "obnovitelného odstranění" v rámci dokumentů nebo použít [předdefinované vlastnosti TTL](time-to-live.md) signál čeká na odstranění v změn informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-207">To capture deletes, you must use a "soft delete" property within your documents, or use the [built-in TTL property](time-to-live.md) to signal a pending deletion in the change feed.</span></span>

<span data-ttu-id="c4fb4-208">Následující tabulka uvádí [požadavku](/rest/api/documentdb/common-documentdb-rest-request-headers.md) a [hlavičky odpovědi](/rest/api/documentdb/common-documentdb-rest-response-headers.md) pro ReadDocumentFeed operace.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-208">The following table lists the [request](/rest/api/documentdb/common-documentdb-rest-request-headers.md) and [response headers](/rest/api/documentdb/common-documentdb-rest-response-headers.md) for ReadDocumentFeed operations.</span></span>

<span data-ttu-id="c4fb4-209">**Hlavičky požadavku pro přírůstkové ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-209">**Request headers for incremental ReadDocumentFeed**:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="c4fb4-210">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="c4fb4-210">Header name</span></span></th>
        <th><span data-ttu-id="c4fb4-211">Popis</span><span class="sxs-lookup"><span data-stu-id="c4fb4-211">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-212">A ZASÍLÁNÍ RYCHLÝCH ZPRÁV</span><span class="sxs-lookup"><span data-stu-id="c4fb4-212">A-IM</span></span></td>
        <td><span data-ttu-id="c4fb4-213">Musí být nastavena na "Přírůstkové informační kanál", nebo tento parametr vynechán jinak</span><span class="sxs-lookup"><span data-stu-id="c4fb4-213">Must be set to "Incremental feed", or omitted otherwise</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-214">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="c4fb4-214">If-None-Match</span></span></td>
        <td>
            <p><span data-ttu-id="c4fb4-215">Žádné záhlaví: vrátí všechny změny od začátku (vytvoření kolekce)</span><span class="sxs-lookup"><span data-stu-id="c4fb4-215">No header: returns all changes from the beginning (collection creation)</span></span></p>
            <p><span data-ttu-id="c4fb4-216">"*": vrátí všechny nové změny dat v rámci kolekce</span><span class="sxs-lookup"><span data-stu-id="c4fb4-216">"*": returns all new changes to data within the collection</span></span></p>           
            <p><span data-ttu-id="c4fb4-217">&lt;Značka Etag&gt;: Pokud vrátí sadu do kolekce značka ETag, všechny změny provedené od této logické časové razítko</span><span class="sxs-lookup"><span data-stu-id="c4fb4-217">&lt;etag&gt;: If set to a collection ETag, returns all changes made since that logical timestamp</span></span></p>
        </td>
    </tr>
    <tr>    
        <td><span data-ttu-id="c4fb4-218">Pokud upravit – od</span><span class="sxs-lookup"><span data-stu-id="c4fb4-218">If-Modified-Since</span></span></td> 
        <td><span data-ttu-id="c4fb4-219">Formát času RFC 1123; Pokud je zadána If-None-Match ignorována</span><span class="sxs-lookup"><span data-stu-id="c4fb4-219">RFC 1123 time format; ignored if If-None-Match is specified</span></span></td> 
    </tr> 
    <tr>
        <td><span data-ttu-id="c4fb4-220">x-ms-documentdb-partitionkeyrangeid</span><span class="sxs-lookup"><span data-stu-id="c4fb4-220">x-ms-documentdb-partitionkeyrangeid</span></span></td>
        <td><span data-ttu-id="c4fb4-221">ID klíče rozsahu oddílu pro čtení dat</span><span class="sxs-lookup"><span data-stu-id="c4fb4-221">The partition key range ID for reading data.</span></span></td>
    </tr>
</table>

<span data-ttu-id="c4fb4-222">**Hlavičky odpovědi pro přírůstkové ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-222">**Response headers for incremental ReadDocumentFeed**:</span></span>

<table> <tr>
        <th><span data-ttu-id="c4fb4-223">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="c4fb4-223">Header name</span></span></th>
        <th><span data-ttu-id="c4fb4-224">Popis</span><span class="sxs-lookup"><span data-stu-id="c4fb4-224">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-225">Značka Etag</span><span class="sxs-lookup"><span data-stu-id="c4fb4-225">etag</span></span></td>
        <td>
            <p><span data-ttu-id="c4fb4-226">Logické pořadové číslo (položky LSN) poslední dokumentu vrácený v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-226">The logical sequence number (LSN) of last document returned in the response.</span></span></p>
            <p><span data-ttu-id="c4fb4-227">Přírůstkové ReadDocumentFeed můžete obnovit pomocí opětovným odesláním tuto hodnotu v If-None-Match.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-227">Incremental ReadDocumentFeed can be resumed by resubmitting this value in If-None-Match.</span></span></p>
        </td>
    </tr>
</table>

<span data-ttu-id="c4fb4-228">Zde je ukázka požadavek na vrácení všechny přírůstkové změny v kolekci z logické verze nebo ETag `28535` a rozdělit na oddíly klíče rozsah = `16`:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-228">Here's a sample request to return all incremental changes in collection from the logical version/ETag `28535` and partition key range = `16`:</span></span>

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="c4fb4-229">Změny jsou seřazené podle času v rámci každé hodnotu klíče oddílu v rozsahu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-229">Changes are ordered by time within each partition key value within the partition key range.</span></span> <span data-ttu-id="c4fb4-230">Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-230">There is no guaranteed order across partition-key values.</span></span> <span data-ttu-id="c4fb4-231">Pokud existují další výsledky, než lze zobrazit v jediné stránce, si můžete přečíst další stránky výsledků podle opětovným odesláním požadavek s `If-None-Match` záhlaví s hodnotou rovna `etag` z předchozí odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-231">If there are more results than can fit in a single page, you can read the next page of results by resubmitting the request with the `If-None-Match` header with value equal to the `etag` from the previous response.</span></span> <span data-ttu-id="c4fb4-232">Pokud více dokumentů se přidají nebo aktualizují transakčně v rámci uložené procedury nebo aktivační událost, budou se všechny vrátí na jedné stránce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-232">If multiple documents were inserted or updated transactionally within a stored procedure or trigger, they will all be returned within the same response page.</span></span>

> [!NOTE]
> <span data-ttu-id="c4fb4-233">S změnu kanálu, může získat více položek vrátí na stránce, než je zadáno v `x-ms-max-item-count` v případě více dokumentů, které se přidají nebo aktualizují v rámci uložené procedury nebo aktivační události.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-233">With change feed, you might get more items returned in a page than specified in `x-ms-max-item-count` in the case of multiple documents inserted or updated inside a stored procedures or triggers.</span></span> 

<span data-ttu-id="c4fb4-234">Když pomocí sady .NET SDK (1.17.0), nastavte hodnotu pole `StartTime` v `ChangeFeedOptions` přímo vrátit změněné dokumenty od `StartTime` při volání metody `CreateDocumentChangeFeedQuery`.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-234">When using the .NET SDK (1.17.0), set the field `StartTime` in `ChangeFeedOptions` to directly return changed documents since `StartTime` when calling  `CreateDocumentChangeFeedQuery`.</span></span> <span data-ttu-id="c4fb4-235">Zadáním `If-Modified-Since` pomocí rozhraní REST API, vaši žádost o vrátí není dokumenty sami, ale spíš token pokračování nebo `etag` v hlavičce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-235">By specifying `If-Modified-Since` using the REST API, your request will return not the documents themselves, but rather the continuation token or `etag` in the response header.</span></span> <span data-ttu-id="c4fb4-236">Vrátit zadaný čas, token pro pokračování změny dokumenty `etag` pak použije v další požadavek s `If-None-Match` vrátit skutečné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-236">To return the documents modified the specified time, the continuation token `etag` must then be used in the next request with `If-None-Match` to return the actual documents.</span></span> 

<span data-ttu-id="c4fb4-237">.NET SDK poskytuje [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) a [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) pomocné třídy pro přístup k změny provedené v kolekci.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-237">The .NET SDK provides the [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) and [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper classes to access changes made to a collection.</span></span> <span data-ttu-id="c4fb4-238">Následující fragment kódu ukazuje, jak načíst všechny změny od začátku pomocí sady .NET SDK z jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-238">The following snippet shows how to retrieve all changes from the beginning using the .NET SDK from a single client.</span></span>

```csharp
private async Task<Dictionary<string, string>> GetChanges(
    DocumentClient client,
    string collection,
    Dictionary<string, string> checkpoints)
{
    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

    foreach (PartitionKeyRange pkRange in partitionKeyRanges)
    {
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);

        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collection,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = 1
            });

        while (query.HasMoreResults)
        {
            FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

            foreach (DeviceReading changedDocument in readChangesResponse)
            {
                Console.WriteLine(changedDocument.Id);
            }

            checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
        }
    }

    return checkpoints;
}
```
<span data-ttu-id="c4fb4-239">A následující fragment kódu ukazuje, jak zpracovat změny v reálném čase s Azure DB Cosmos pomocí kanálu změnu podporu a předchozí funkce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-239">And the following snippet shows how to process changes in real-time with Azure Cosmos DB by using the change feed support and the preceding function.</span></span> <span data-ttu-id="c4fb4-240">První volání vrátí všechny dokumenty v kolekci, a druhá pouze vrátí vytvořili dva dokumenty, které byly vytvořeny od posledního kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-240">The first call returns all the documents in the collection, and the second only returns the two documents created that were created since the last checkpoint.</span></span>

```csharp
// Returns all documents in the collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only the two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

<span data-ttu-id="c4fb4-241">Můžete také filtrovat změnu kanálu pomocí logiky straně klienta se selektivně zpracovat události.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-241">You can also filter the change feed using client side logic to selectively process events.</span></span> <span data-ttu-id="c4fb4-242">Například zde je fragment kódu, který používá na straně klienta LINQ ke zpracování události změny pouze teploty ze senzorů zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-242">For example, here's a snippet that uses client side LINQ to process only temperature change events from device sensors.</span></span>

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <span data-ttu-id="c4fb4-243"><a id="change-feed-processor"></a>Knihovna kanálu procesoru změn</span><span class="sxs-lookup"><span data-stu-id="c4fb4-243"><a id="change-feed-processor"></a>Change Feed Processor library</span></span>
<span data-ttu-id="c4fb4-244">Další možností je použít [knihovny Azure Cosmos DB změnit kanálu procesoru](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), který vám může pomoct snadno distribuovat zpracování z důvodu změny kanálu napříč více příjemců událostí.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-244">Another option is to use the [Azure Cosmos DB Change Feed Processor library](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), which can help you easily distribute event processing from a change feed across multiple consumers.</span></span> <span data-ttu-id="c4fb4-245">Knihovna je skvěle hodí k vytváření změnu kanálu čtečky na platformě .NET.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-245">The library is great for building change feed readers on the .NET platform.</span></span> <span data-ttu-id="c4fb4-246">Některé pracovní postupy, které by zjednodušit pomocí knihovně procesoru kanálu změn prostřednictvím metod obsažených v jiných SDK DB Cosmos patří:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-246">Some workflows that would be simplified by using the Change Feed Processor library over the methods included in the other Cosmos DB SDKs include:</span></span> 

* <span data-ttu-id="c4fb4-247">Vyžádání aktualizace z změnu informačního kanálu, když data budou uložená napříč více oddílů</span><span class="sxs-lookup"><span data-stu-id="c4fb4-247">Pulling updates from change feed when data is stored across multiple partitions</span></span>
* <span data-ttu-id="c4fb4-248">Přesunutí nebo replikaci dat z jedné kolekce do jiného</span><span class="sxs-lookup"><span data-stu-id="c4fb4-248">Moving or replicating data from one collection to another</span></span>
* <span data-ttu-id="c4fb4-249">Paralelní provádění akcí, které jsou aktivovány aktualizace dat a změna kanálu</span><span class="sxs-lookup"><span data-stu-id="c4fb4-249">Parallel execution of actions triggered by updates to data and change feed</span></span> 

<span data-ttu-id="c4fb4-250">Zatímco pomocí rozhraní API v Cosmos SDK poskytuje přesné přístup k změnit informačního kanálu aktualizace v každém oddílu, pomocí knihovny změnit kanálu procesoru zjednodušuje čtení změny mezi oddílů a paralelně fungujících více vláken.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-250">While using the APIs in the Cosmos SDKs provides precise access to change feed updates in each partition, using the Change Feed Processor library simplifies reading changes across partitions and multiple threads working in parallel.</span></span> <span data-ttu-id="c4fb4-251">Procesor kanálu změnu místo ručně čtení změny z každý kontejner a ukládání token pokračování pro každý oddíl, automaticky spravuje čtení změny napříč oddíly používající mechanismus zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-251">Instead of manually reading changes from each container and saving a continuation token for each partition, the Change Feed Processor automatically manages reading changes across partitions using a lease mechanism.</span></span>

<span data-ttu-id="c4fb4-252">Je k dispozici jako balíčku NuGet, knihovna: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) a ze zdrojového kódu jako Githubu [ukázka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-252">The library is available as a NuGet Package: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) and from source code as a Github [sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span></span> 

### <a name="understanding-change-feed-processor-library"></a><span data-ttu-id="c4fb4-253">Principy změn kanálu procesoru knihovny</span><span class="sxs-lookup"><span data-stu-id="c4fb4-253">Understanding Change Feed Processor library</span></span> 

<span data-ttu-id="c4fb4-254">Existují čtyři hlavní součásti implementace změnu kanálu procesoru: monitorovaných kolekce, kolekce zapůjčení, procesoru hostitele a uživatelé.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-254">There are four main components of implementing the Change Feed Processor: the monitored collection, the lease collection, the processor host, and the consumers.</span></span> 

<span data-ttu-id="c4fb4-255">**Monitorované kolekci:** monitorovaných kolekce je dat, ze které se generuje změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-255">**Monitored Collection:** The monitored collection is the data from which the change feed is generated.</span></span> <span data-ttu-id="c4fb4-256">Všechny operace INSERT a změny monitorovaných kolekce se projeví v informačním kanálu změnu kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-256">Any inserts and changes to the monitored collection are reflected in the change feed of the collection.</span></span> 

<span data-ttu-id="c4fb4-257">**Kolekce zapůjčení:** souřadnice kolekce zapůjčení zpracování změn kanálu napříč více pracovníků.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-257">**Lease Collection:** The lease collection coordinates processing the change feed across multiple workers.</span></span> <span data-ttu-id="c4fb4-258">Samostatné kolekce slouží k uložení zapůjčení jeden zapůjčení na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-258">A separate collection is used to store the leases with one lease per partition.</span></span> <span data-ttu-id="c4fb4-259">Je výhodné pro uložení této kolekce zapůjčení na jiný účet s oblasti zápisu blíže k se spuštěným systémem změnu kanálu procesoru.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-259">It is advantageous to store this lease collection on a different account with the write region closer to where the Change Feed Processor is running.</span></span> <span data-ttu-id="c4fb4-260">Objekt zapůjčení obsahuje následující atributy:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-260">A lease object contains the following attributes:</span></span> 
* <span data-ttu-id="c4fb4-261">Vlastník: Určuje hostitele, který vlastní zapůjčení</span><span class="sxs-lookup"><span data-stu-id="c4fb4-261">Owner: Specifies the host that owns the lease</span></span>
* <span data-ttu-id="c4fb4-262">Pokračování: Určuje pozici (token pokračování) v kanálu pro určitý oddíl změn</span><span class="sxs-lookup"><span data-stu-id="c4fb4-262">Continuation: Specifies the position (continuation token) in the change feed for a particular partition</span></span>
* <span data-ttu-id="c4fb4-263">Časové razítko: Čas poslední zapůjčení byla aktualizována; časové razítko slouží ke kontrole, jestli zapůjčení je považována za ukončenou</span><span class="sxs-lookup"><span data-stu-id="c4fb4-263">Timestamp: Last time lease was updated; the timestamp can be used to check whether the lease is considered expired</span></span> 

<span data-ttu-id="c4fb4-264">**Procesor hostitele:** každého hostitele Určuje, kolik oddíly procesu na základě kolik instancí hostitelů mají aktivní zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-264">**Processor Host:** Each host determines how many partitions to process based on how many other instances of hosts have active leases.</span></span> 
1.  <span data-ttu-id="c4fb4-265">Po spuštění hostitele získá zapůjčení vyrovnávat zatížení ve všech hostitelích.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-265">When a host starts up, it acquires leases to balance the workload across all hosts.</span></span> <span data-ttu-id="c4fb4-266">Hostitel pravidelně obnovuje zapůjčení, aby zůstala aktivní zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-266">A host periodically renews leases, so leases remain active.</span></span> 
2.  <span data-ttu-id="c4fb4-267">Kontrolní body hostitele poslední token pokračování jeho zapůjčení pro každou přečíst.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-267">A host checkpoints the last continuation token to its lease for each read.</span></span> <span data-ttu-id="c4fb4-268">K zajištění bezpečnosti souběžnosti, zkontroluje hostitele ETag pro jednotlivé aktualizace zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-268">To ensure concurrency safety, a host checks the ETag for each lease update.</span></span> <span data-ttu-id="c4fb4-269">Jsou podporovány také další strategie kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-269">Other checkpoint strategies are also supported.</span></span>  
3.  <span data-ttu-id="c4fb4-270">Při vypnutí počítače Hostitel uvolní všechny zapůjčení, ale zachová pokračování informace, takže ho můžete obnovit čtení z uložené kontrolního bodu později.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-270">Upon shutdown, a host releases all leases but keeps the continuation information, so it can resume reading from the stored checkpoint later.</span></span> 

<span data-ttu-id="c4fb4-271">V tuto chvíli nemůže být větší než počet oddílů (zapůjčení) počtu hostitelů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-271">At this time the number of hosts cannot be greater than the number of partitions (leases).</span></span>

<span data-ttu-id="c4fb4-272">**Příjemci knihovny:** příjemci nebo pracovníci, jsou vláken, které provádějí změnu kanálu zpracování iniciovaná každého hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-272">**Consumers:** Consumers, or workers, are threads that perform the change feed processing initiated by each host.</span></span> <span data-ttu-id="c4fb4-273">Každý hostitel procesoru může mít více příjemců.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-273">Each processor host can have multiple consumers.</span></span> <span data-ttu-id="c4fb4-274">Jednotliví spotřebitelé přečte změnu kanálu z oddílu, který je přiřazen k a upozorní jeho hostitel změny a platnost zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-274">Each consumer reads the change feed from the partition it is assigned to and notifies its host of changes and expired leases.</span></span>

<span data-ttu-id="c4fb4-275">Abyste pochopili, jak tyto čtyři prvky změnu kanálu procesoru spolupracují, podíváme se na příklad na následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-275">To further understand how these four elements of Change Feed Processor work together, let's look at an example in the following diagram.</span></span> <span data-ttu-id="c4fb4-276">Monitorované kolekci ukládá dokumenty a používá "city" jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-276">The monitored collection stores documents and uses the "city" as the partition key.</span></span> <span data-ttu-id="c4fb4-277">Vidíte, že blue oddíl obsahuje dokumentů s poli "city" od "A-E" a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-277">We see that the blue partition contains documents with the "city" field from "A-E" and so on.</span></span> <span data-ttu-id="c4fb4-278">Existují dva hostitele, každý se dvěma spotřebiteli čtení ze čtyř oddílů souběžně.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-278">There are two hosts, each with two consumers reading from the four partitions in parallel.</span></span> <span data-ttu-id="c4fb4-279">Šipky zobrazují příjemci čtení z na určité místo v změn informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-279">The arrows show the consumers reading from a specific spot in the change feed.</span></span> <span data-ttu-id="c4fb4-280">Do prvního oddílu představuje tmavšího blue nepřečtená změny při světla blue představuje již čtení změny při změně kanálu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-280">In the first partition, the darker blue represents unread changes while the light blue represents the already read changes on the change feed.</span></span> <span data-ttu-id="c4fb4-281">Hostitele použít k ukládání hodnotu "pokračování" ke sledování aktuální pozici čtení pro každý příjemce kolekci zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-281">The hosts use the lease collection to store a "continuation" value to keep track of the current reading position for each consumer.</span></span> 

![Pomocí Azure Cosmos DB změnu kanálu procesor hostitele](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a><span data-ttu-id="c4fb4-283">Pomocí změnu kanálu procesoru knihovny</span><span class="sxs-lookup"><span data-stu-id="c4fb4-283">Using Change Feed Processor Library</span></span> 
<span data-ttu-id="c4fb4-284">V následující části vysvětluje, jak na používání knihovny změnu kanálu procesoru v kontextu replikace změn z kolekce zdrojové do cílové kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-284">The following section explains how to use the Change Feed Processor library in the context of replicating changes from a source collection to a destination collection.</span></span> <span data-ttu-id="c4fb4-285">Zde zdrojové kolekci je monitorované kolekci změnu kanálu procesor.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-285">Here, the source collection is the monitored collection in Change Feed Processor.</span></span> 

<span data-ttu-id="c4fb4-286">**Nainstalovat a zahrnout balíček NuGet procesoru kanálu změn**</span><span class="sxs-lookup"><span data-stu-id="c4fb4-286">**Install and include the Change Feed Processor NuGet package**</span></span> 

<span data-ttu-id="c4fb4-287">Před instalací balíčku NuGet změnu kanálu procesoru, nejprve nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-287">Before installing Change Feed Processor NuGet Package, first install:</span></span> 
* <span data-ttu-id="c4fb4-288">Microsoft.Azure.DocumentDB, verze 1.13.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="c4fb4-288">Microsoft.Azure.DocumentDB, version 1.13.1 or above</span></span> 
* <span data-ttu-id="c4fb4-289">Newtonsoft.Json, verze 9.0.1 nebo vyšší instalace `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` a její zahrnutí jako odkaz.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-289">Newtonsoft.Json, version 9.0.1 or above Install `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` and include it as a reference.</span></span>

<span data-ttu-id="c4fb4-290">**Vytvoření monitorovaných, zapůjčení a cílové kolekce**</span><span class="sxs-lookup"><span data-stu-id="c4fb4-290">**Create a monitored, lease and destination collection**</span></span> 

<span data-ttu-id="c4fb4-291">Chcete-li použít knihovna procesoru kanálu změn, musí být vytvořen před spuštěním procesor hostitel kolekci zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-291">In order to use the Change Feed Processor Library, the lease collection needs to be created before running the processor host(s).</span></span> <span data-ttu-id="c4fb4-292">Znovu doporučujeme uložit kolekci zapůjčení na jiný účet s oblastí zápisu blíže k se spuštěným systémem změnu kanálu procesoru.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-292">Again, we recommend storing a lease collection on a different account with a write region closer to where the Change Feed Processor is running.</span></span> <span data-ttu-id="c4fb4-293">V tomto příkladu přesun dat potřebujeme vytvoření cílové kolekce před spuštěním změnu kanálu procesoru hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-293">In this data movement example, we need to create the destination collection before running the Change Feed Processor host.</span></span> <span data-ttu-id="c4fb4-294">Ukázkový kód jsme volat metodu helper k vytvoření monitorovaných, pronajaté, a cílové kolekce, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-294">In the sample code we call a helper method to create the monitored, leased, and destination collections if they do not already exist.</span></span> 

> [!WARNING]
> <span data-ttu-id="c4fb4-295">Vytvoření kolekce hradí, jako jsou rezervování propustnost pro aplikace komunikovat s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-295">Creating a collection has pricing implications, as you are reserving throughput for the application to communicate with Azure Cosmos DB.</span></span> <span data-ttu-id="c4fb4-296">Další podrobnosti naleznete [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="c4fb4-296">For more details, please visit the [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

<span data-ttu-id="c4fb4-297">*Vytvoření procesor hostitele*</span><span class="sxs-lookup"><span data-stu-id="c4fb4-297">*Creating a processor host*</span></span>

<span data-ttu-id="c4fb4-298">`ChangeFeedProcessorHost` Třída poskytuje bezpečné pro přístup z více vláken, více procesů, bezpečné běhového prostředí pro implementace zpracovatelů událostí taky poskytuje možnost vytváření kontrolních bodů a oddíl správu zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-298">The `ChangeFeedProcessorHost` class provides a thread-safe, multi-process, safe runtime environment for event processor implementations that also provides checkpointing and partition lease management.</span></span> <span data-ttu-id="c4fb4-299">Použít `ChangeFeedProcessorHost` třídu, můžete implementovat `IChangeFeedObserver`.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-299">To use the `ChangeFeedProcessorHost` class, you can implement `IChangeFeedObserver`.</span></span> <span data-ttu-id="c4fb4-300">Toto rozhraní obsahuje tři metody:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-300">This interface contains three methods:</span></span>

* <span data-ttu-id="c4fb4-301">`OpenAsync`: Tato funkce je volána, když je otevřen pozorovatel změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-301">`OpenAsync`: This function is called when change feed observer is opened.</span></span> <span data-ttu-id="c4fb4-302">Ho můžete upravit tak, aby odpovídat určité akci, když je otevřen příjemce/pozorovatele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-302">It can be modified to perform a specific action when consumer/observer is opened.</span></span>  
* <span data-ttu-id="c4fb4-303">`CloseAsync`: Tato funkce je volána, když je ukončen informačního kanálu pozorovatel změnu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-303">`CloseAsync`: This function is called when change feed observer is terminated.</span></span> <span data-ttu-id="c4fb4-304">Může být upraven k provedení určité akce při zavření příjemce/pozorovatele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-304">It can be modified to perform a specific action when consumer/observer is closed.</span></span>  
* <span data-ttu-id="c4fb4-305">`ProcessChangesAsync`: Tato funkce je volána, když jsou k dispozici na změnu kanálu nové změny dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-305">`ProcessChangesAsync`: This function is called when document new changes are available on change feed.</span></span> <span data-ttu-id="c4fb4-306">Může být upraven k provedení určité akce při každé změně kanálu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-306">It can be modified to perform a specific action upon every change feed update.</span></span>  

<span data-ttu-id="c4fb4-307">V našem příkladu jsme toto rozhraní implementovat `IChangeFeedObserver` prostřednictvím `DocumentFeedObserver` třídy.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-307">In our example, we implement the interface `IChangeFeedObserver` through the `DocumentFeedObserver` class.</span></span> <span data-ttu-id="c4fb4-308">Zde `ProcessChangesAsync` funkce upserts (aktualizace) dokumentu z změnu kanálu do cílové kolekce.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-308">Here, the `ProcessChangesAsync` function upserts (updates) a document from change feed into the destination collection.</span></span> <span data-ttu-id="c4fb4-309">V tomto příkladu je užitečná pro přesun dat z jedné kolekce do jiného, aby bylo možné změnit klíč oddílu datové sady.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-309">This example is useful for moving data from one collection to another in order to change the partition key of a data set.</span></span> 

<span data-ttu-id="c4fb4-310">*Spuštění procesoru hostitele*</span><span class="sxs-lookup"><span data-stu-id="c4fb4-310">*Running the Processor Host*</span></span>

<span data-ttu-id="c4fb4-311">Před zahájením zpracování událostí, jak změnit možnosti informačního kanálu a změnit možnosti informačního kanálu hostitele je možné upravit.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-311">Before beginning event processing, both change feed options and change feed host options can be customized.</span></span> 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval to 15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
<span data-ttu-id="c4fb4-312">V následujících tabulkách jsou shrnuté konkrétních polí, které se dají přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-312">The specific fields that can be customized are summarized in the following tables.</span></span> 

<span data-ttu-id="c4fb4-313">**Změna možností informačního kanálu**:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-313">**Change Feed Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="c4fb4-314">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c4fb4-314">Property Name</span></span></th>
        <th><span data-ttu-id="c4fb4-315">Popis</span><span class="sxs-lookup"><span data-stu-id="c4fb4-315">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-316">MaxItemCount</span><span class="sxs-lookup"><span data-stu-id="c4fb4-316">MaxItemCount</span></span></td>
        <td><span data-ttu-id="c4fb4-317">Získá nebo nastaví maximální počet položek má být vrácen v operaci výčtu ve službě Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-317">Gets or sets the maximum number of items to be returned in the enumeration operation in the Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-318">PartitionKeyRangeId</span><span class="sxs-lookup"><span data-stu-id="c4fb4-318">PartitionKeyRangeId</span></span></td>
        <td><span data-ttu-id="c4fb4-319">Získá nebo nastaví id klíče rozsahu oddílu pro aktuální požadavek ve službě Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-319">Gets or sets the partition key range id for the current request in the Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-320">RequestContinuation</span><span class="sxs-lookup"><span data-stu-id="c4fb4-320">RequestContinuation</span></span></td>
        <td><span data-ttu-id="c4fb4-321">Získá nebo nastaví token pokračování požadavku ve službě Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-321">Gets or sets the request continuation token in the Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="c4fb4-322">SessionToken</span><span class="sxs-lookup"><span data-stu-id="c4fb4-322">SessionToken</span></span></td>
        <td><span data-ttu-id="c4fb4-323">Získá nebo nastaví token relace pro použití s konzistence typu relace ve službě Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-323">Gets or sets the session token for use with session consistency in the Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="c4fb4-324">StartFromBeginning</span><span class="sxs-lookup"><span data-stu-id="c4fb4-324">StartFromBeginning</span></span></td>
        <td><span data-ttu-id="c4fb4-325">Získá nebo nastaví, zda změna kanálu ve službě Azure Cosmos DB databáze by se měl spustit od začátku (true) nebo z aktuální (false).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-325">Gets or sets whether change feed in the Azure Cosmos DB database service should start from the beginning (true) or from current (false).</span></span> <span data-ttu-id="c4fb4-326">Ve výchozím nastavení spustí z aktuální (false).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-326">By default, it starts from current (false).</span></span></td>
    </tr>
</table>

<span data-ttu-id="c4fb4-327">**Změna možností informačního kanálu hostitele**:</span><span class="sxs-lookup"><span data-stu-id="c4fb4-327">**Change Feed Host Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="c4fb4-328">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c4fb4-328">Property Name</span></span></th>
        <th><span data-ttu-id="c4fb4-329">Typ</span><span class="sxs-lookup"><span data-stu-id="c4fb4-329">Type</span></span></th>
        <th><span data-ttu-id="c4fb4-330">Popis</span><span class="sxs-lookup"><span data-stu-id="c4fb4-330">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-331">LeaseRenewInterval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-331">LeaseRenewInterval</span></span></td>
        <td><span data-ttu-id="c4fb4-332">Časový interval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-332">TimeSpan</span></span></td>
        <td><span data-ttu-id="c4fb4-333">Interval pro všechny zapůjčení pro oddíly, které jsou aktuálně uchovávat ChangeFeedEventHost instance.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-333">The interval for all leases for partitions currently held by the ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-334">LeaseAcquireInterval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-334">LeaseAcquireInterval</span></span></td>
        <td><span data-ttu-id="c4fb4-335">Časový interval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-335">TimeSpan</span></span></td>
        <td><span data-ttu-id="c4fb4-336">Interval, který ji úlohu k výpočtu, zda jsou oddíly rovnoměrně rozdělené mezi instancí známé hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-336">The interval to kick off a task to compute whether partitions are distributed evenly among known host instances.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-337">LeaseExpirationInterval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-337">LeaseExpirationInterval</span></span></td>
        <td><span data-ttu-id="c4fb4-338">Časový interval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-338">TimeSpan</span></span></td>
        <td><span data-ttu-id="c4fb4-339">Interval, pro kterou je zapůjčení pořízené zapůjčení představující oddílu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-339">The interval for which the lease is taken on a lease representing a partition.</span></span> <span data-ttu-id="c4fb4-340">Pokud během tohoto intervalu neobnovíte zapůjčení, vypršela platnost a vlastnictví oddílu přesune do jiné instance ChangeFeedEventHost.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-340">If the lease is not renewed within this interval, it is expired and ownership of the partition moves to another ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-341">FeedPollDelay</span><span class="sxs-lookup"><span data-stu-id="c4fb4-341">FeedPollDelay</span></span></td>
        <td><span data-ttu-id="c4fb4-342">Časový interval</span><span class="sxs-lookup"><span data-stu-id="c4fb4-342">TimeSpan</span></span></td>
        <td><span data-ttu-id="c4fb4-343">Zpoždění mezi dotazování oddíl pro nové změny na informačního kanálu, po všechny aktuální změny se nečekaně.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-343">The delay between polling a partition for new changes on the feed, after all current changes are drained.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-344">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="c4fb4-344">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="c4fb4-345">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="c4fb4-345">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="c4fb4-346">Četnost zapůjčení kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-346">The frequency to checkpoint leases.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-347">MinPartitionCount</span><span class="sxs-lookup"><span data-stu-id="c4fb4-347">MinPartitionCount</span></span></td>
        <td><span data-ttu-id="c4fb4-348">celá čísla</span><span class="sxs-lookup"><span data-stu-id="c4fb4-348">Int</span></span></td>
        <td><span data-ttu-id="c4fb4-349">Minimální počet pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-349">The minimum partition count for the host.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-350">MaxPartitionCount</span><span class="sxs-lookup"><span data-stu-id="c4fb4-350">MaxPartitionCount</span></span></td>
        <td><span data-ttu-id="c4fb4-351">celá čísla</span><span class="sxs-lookup"><span data-stu-id="c4fb4-351">Int</span></span></td>
        <td><span data-ttu-id="c4fb4-352">Maximální počet oddílů, které hostitele může obsluhovat.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-352">The maximum number of partitions the host can serve.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="c4fb4-353">DiscardExistingLeases</span><span class="sxs-lookup"><span data-stu-id="c4fb4-353">DiscardExistingLeases</span></span></td>
        <td><span data-ttu-id="c4fb4-354">BOOL</span><span class="sxs-lookup"><span data-stu-id="c4fb4-354">Bool</span></span></td>
        <td><span data-ttu-id="c4fb4-355">Jestli se při spuštění hostitele, měla by být odstraněna všechna existující zapůjčení a hostitele by měla začít úplně od začátku.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-355">Whether on the start of the host all existing leases should be deleted and the host should start from scratch.</span></span></td>
    </tr>
</table>


<span data-ttu-id="c4fb4-356">Pokud chcete zahájit zpracování událostí, vytvořte instanci `ChangeFeedProcessorHost`, poskytnutím příslušných parametrů pro svoji kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-356">To start event processing, instantiate `ChangeFeedProcessorHost`, providing the appropriate parameters for your Azure Cosmos DB collection.</span></span> <span data-ttu-id="c4fb4-357">Potom zavolejte `RegisterObserverAsync` k registraci vašeho `IChangeFeedObserver` implementace (DocumentFeedObserver v tomto příkladu) s modulem runtime.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-357">Then, call `RegisterObserverAsync` to register your `IChangeFeedObserver` (DocumentFeedObserver in this example) implementation with the runtime.</span></span> <span data-ttu-id="c4fb4-358">V tomto okamžiku hostitel pokusí "zapůjčit" každý oddíl klíče rozsahu v Azure Cosmos DB kolekce s použitím "chamtivého" algoritmu.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-358">At this point, the host attempts to acquire a lease on every partition key range in the Azure Cosmos DB collection using a "greedy" algorithm.</span></span> <span data-ttu-id="c4fb4-359">Tato zapůjčení poslední po stanovenou dobu a následně musí být obnovena.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-359">These leases last for a given timeframe and must then be renewed.</span></span> <span data-ttu-id="c4fb4-360">Jako nové uzly a instancí pracovního procesu, v takovém případě režimu online, se umístí své rezervace zapůjčení a průběhu času zatížení posune mezi uzly, protože každý hostitel bude snažit získat další zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-360">As new nodes, worker instances, in this case, come online, they place lease reservations and over time the load shifts between nodes as each host attempts to acquire more leases.</span></span> 

<span data-ttu-id="c4fb4-361">V ukázkovém kódu používáme třídu objektů factory (DocumentFeedObserverFactory.cs) k vytvoření pozorovatele a `RegistObserverFactoryAsync` k registraci pozorovatele.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-361">In the sample code, we use a factory class (DocumentFeedObserverFactory.cs) to create an observer and the `RegistObserverFactoryAsync` to register the observer.</span></span> 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter to stop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
<span data-ttu-id="c4fb4-362">Postupem času se dosáhne rovnováhy.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-362">Over time, an equilibrium is established.</span></span> <span data-ttu-id="c4fb4-363">Tato dynamická funkce umožňuje využití procesoru na základě automatického škálování má být použita k příjemce pro škálování nahoru a dolů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-363">This dynamic capability enables CPU-based auto-scaling to be applied to consumers for both scale-up and scale-down.</span></span> <span data-ttu-id="c4fb4-364">Pokud jsou k dispozici v Azure Cosmos DB s vyšší rychlostí než mohou příjemci zpracovat změny, zvýšení využití procesoru na spotřebitele slouží k dojít k automatickému škálování počtu instancí pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="c4fb4-364">If changes are available in Azure Cosmos DB at a faster rate than consumers can process, the CPU increase on consumers can be used to cause an auto-scale on worker instance count.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4fb4-365">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4fb4-365">Next steps</span></span>
* <span data-ttu-id="c4fb4-366">Zkuste [Azure Cosmos DB změnu kanálu ukázky kódu na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span><span class="sxs-lookup"><span data-stu-id="c4fb4-366">Try the [Azure Cosmos DB Change feed code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span></span>
* <span data-ttu-id="c4fb4-367">Začínáme s kódování [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) nebo [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="c4fb4-367">Get started coding with the [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/).</span></span>
