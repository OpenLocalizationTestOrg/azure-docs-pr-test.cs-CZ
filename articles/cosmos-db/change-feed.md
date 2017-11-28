---
title: "aaaWorking s hello změnu kanálu podpory v Azure Cosmos DB | Microsoft Docs"
description: "Použití Azure Cosmos DB změnit informačního kanálu podporu tootrack změny v dokumentech a provádět na základě událostí zpracování jako aktivační události a průběžná aktualizace mezipaměti a analýzy systémy."
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
ms.openlocfilehash: a4dcf4ceb476e3e08266dbcdcbee1d75e1d1eed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a><span data-ttu-id="81a2d-104">Práce s hello změnu informačního kanálu podpory v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="81a2d-104">Working with hello change feed support in Azure Cosmos DB</span></span>
<span data-ttu-id="81a2d-105">[Azure Cosmos DB](../cosmos-db/introduction.md) je rychlého a flexibilní globálně replikované databáze služby, který slouží k ukládání velkých objemů dat transakcí a funkční s latencí předvídatelný jednociferné milisekund pro čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is a fast and flexible globally replicated database service that is used for storing high-volume transactional and operational data with predictable single-digit millisecond latency for reads and writes.</span></span> <span data-ttu-id="81a2d-106">Díky tomu dobře hodí pro IoT, hry, maloobchodní a provozní protokolování aplikace.</span><span class="sxs-lookup"><span data-stu-id="81a2d-106">This makes it well-suited for IoT, gaming, retail, and operational logging applications.</span></span> <span data-ttu-id="81a2d-107">Běžné vzoru návrhu v těchto aplikacích je tootrack změny provedené tooAzure dat Cosmos databáze a aktualizovat materializovaným zobrazením, provádět analýzu v reálném čase, archivace dat toocold úložiště a aktivovat oznámení na určité události na základě těchto změn.</span><span class="sxs-lookup"><span data-stu-id="81a2d-107">A common design pattern in these applications is tootrack changes made tooAzure Cosmos DB data, and update materialized views, perform real-time analytics, archive data toocold storage, and trigger notifications on certain events based on these changes.</span></span> <span data-ttu-id="81a2d-108">Hello **změnu kanálu podporu** v Azure Cosmos DB vám umožní toobuild efektivní a škálovatelné řešení pro každou z těchto vzorků.</span><span class="sxs-lookup"><span data-stu-id="81a2d-108">hello **change feed support** in Azure Cosmos DB enables you toobuild efficient and scalable solutions for each of these patterns.</span></span>

<span data-ttu-id="81a2d-109">Změny kanálu podpory poskytuje Azure Cosmos DB seřazený seznam dokumenty v kolekci Azure Cosmos DB v hello pořadí, ve kterém byly upraveny.</span><span class="sxs-lookup"><span data-stu-id="81a2d-109">With change feed support, Azure Cosmos DB provides a sorted list of documents within an Azure Cosmos DB collection in hello order in which they were modified.</span></span> <span data-ttu-id="81a2d-110">Tento informační kanál lze použít toolisten pro úpravy toodata v rámci kolekce hello a provádět akce, jako například:</span><span class="sxs-lookup"><span data-stu-id="81a2d-110">This feed can be used toolisten for modifications toodata within hello collection and perform actions such as:</span></span>

* <span data-ttu-id="81a2d-111">Aktivovat tooan volání rozhraní API, kdy je dokument vložit nebo úpravě</span><span class="sxs-lookup"><span data-stu-id="81a2d-111">Trigger a call tooan API when a document is inserted or modified</span></span>
* <span data-ttu-id="81a2d-112">Na aktualizace provést zpracování v reálném čase (proud)</span><span class="sxs-lookup"><span data-stu-id="81a2d-112">Perform real-time (stream) processing on updates</span></span>
* <span data-ttu-id="81a2d-113">Synchronizaci dat s mezipaměti, vyhledávací web nebo datového skladu</span><span class="sxs-lookup"><span data-stu-id="81a2d-113">Synchronize data with a cache, search engine, or data warehouse</span></span>

<span data-ttu-id="81a2d-114">Změny v Azure Cosmos DB jsou nastavené jako trvalé může být zpracována asynchronně a distribuovaná do jednoho nebo více spotřebitelů pro paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="81a2d-114">Changes in Azure Cosmos DB are persisted and can be processed asynchronously, and distributed across one or more consumers for parallel processing.</span></span> <span data-ttu-id="81a2d-115">Podívejme se na hello rozhraní API pro změnu kanálu a jak lze využít toobuild škálovatelné aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="81a2d-115">Let's look at hello APIs for change feed and how you can use them toobuild scalable real-time applications.</span></span> <span data-ttu-id="81a2d-116">Tento článek ukazuje, jak změnit toowork s Azure Cosmos DB informační kanál a hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="81a2d-116">This article shows how toowork with Azure Cosmos DB change feed and hello DocumentDB API.</span></span> 

![Pomocí Azure Cosmos DB změnu kanálu toopower analýzu v reálném čase a událostmi řízené výpočetní scénáře](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> <span data-ttu-id="81a2d-118">Změna kanálu podpora je k dispozici pouze pro hello DocumentDB rozhraní API v tuto chvíli; Hello rozhraní Graph API a rozhraní API tabulky nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="81a2d-118">Change feed support is only provided for hello DocumentDB API at this time; hello Graph API and Table API are not currently supported.</span></span>

## <a name="use-cases-and-scenarios"></a><span data-ttu-id="81a2d-119">Případy použití a scénáře</span><span class="sxs-lookup"><span data-stu-id="81a2d-119">Use cases and scenarios</span></span>
<span data-ttu-id="81a2d-120">Změna kanálu umožňuje efektivní zpracování rozsáhlých datových sad k velkému počtu zápisy a nabízí tooidentify alternativní tooquerying celé datové sady, co se změnilo.</span><span class="sxs-lookup"><span data-stu-id="81a2d-120">Change feed allows for efficient processing of large datasets with a high volume of writes, and offers an alternative tooquerying entire datasets tooidentify what has changed.</span></span> <span data-ttu-id="81a2d-121">Například můžete provádět následující úlohy efektivně hello:</span><span class="sxs-lookup"><span data-stu-id="81a2d-121">For example, you can perform hello following tasks efficiently:</span></span>

* <span data-ttu-id="81a2d-122">Aktualizace mezipaměti, index vyhledávání nebo datového skladu s daty uloženými v databázi Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="81a2d-122">Update a cache, search index, or a data warehouse with data stored in Azure Cosmos DB.</span></span>
* <span data-ttu-id="81a2d-123">Využití dat vrstvení a archivaci úrovni aplikace, tedy ukládání "horkých dat." v Azure Cosmos DB a po určité době odstraněny "pomaleji přístupná data" příliš[Azure Blob Storage](../storage/common/storage-introduction.md) nebo [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="81a2d-123">Implement application-level data tiering and archival, that is, store "hot data" in Azure Cosmos DB, and age out "cold data" too[Azure Blob Storage](../storage/common/storage-introduction.md) or [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span></span>
* <span data-ttu-id="81a2d-124">Implementace batch analytics na data pomocí [Apache Hadoop](run-hadoop-with-hdinsight.md).</span><span class="sxs-lookup"><span data-stu-id="81a2d-124">Implement batch analytics on data using [Apache Hadoop](run-hadoop-with-hdinsight.md).</span></span>
* <span data-ttu-id="81a2d-125">Implementace [lambda kanály v Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81a2d-125">Implement [lambda pipelines on Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) with Azure Cosmos DB.</span></span> <span data-ttu-id="81a2d-126">Azure Cosmos DB poskytuje řešení škálovatelná databáze, které může zpracovat přijímání a dotazů a implementovat lambda architektury s nízkou celkové náklady na vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="81a2d-126">Azure Cosmos DB provides a scalable database solution that can handle both ingestion and query, and implement lambda architectures with low TCO.</span></span> 
* <span data-ttu-id="81a2d-127">Proveďte nulové době migrace tooanother účet Azure Cosmos DB jiné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-127">Perform zero down-time migrations tooanother Azure Cosmos DB account with a different partitioning scheme.</span></span>

<span data-ttu-id="81a2d-128">**Lambda kanálů s Azure DB Cosmos pro přijímání a dotazů:**</span><span class="sxs-lookup"><span data-stu-id="81a2d-128">**Lambda Pipelines with Azure Cosmos DB for ingestion and query:**</span></span>

![Azure Cosmos DB na základě lambda kanálu pro přijímání a dotazů](./media/change-feed/lambda.png)

<span data-ttu-id="81a2d-130">Můžete použít Azure Cosmos DB tooreceive a ukládání dat události ze zařízení, senzorů, infrastruktury a aplikace a zpracování těchto událostí v reálném čase pomocí [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), nebo [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="81a2d-130">You can use Azure Cosmos DB tooreceive and store event data from devices, sensors, infrastructure, and applications, and process these events in real-time with [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), or [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span></span> 

<span data-ttu-id="81a2d-131">V rámci vaší [bez serveru](http://azure.com/serverless) webových a mobilních aplikací, můžete sledovat událostmi, jako je například zákazníka tooyour změny profilu, předvolby nebo umístění tootrigger určité akce, jako je odesílání nabízených oznámení tootheir zařízení pomocí [Azure Functions](../azure-functions/functions-bindings-documentdb.md) nebo [aplikační služby](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="81a2d-131">Within your [serverless](http://azure.com/serverless) web and mobile apps, you can track events such as changes tooyour customer's profile, preferences, or location tootrigger certain actions like sending push notifications tootheir devices using [Azure Functions](../azure-functions/functions-bindings-documentdb.md) or [App Services](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="81a2d-132">Pokud používáte Azure Cosmos DB toobuild hry, můžete vytvořit, například změna tooimplement informačního kanálu v reálném čase tabulky podle skóre z dokončené hry.</span><span class="sxs-lookup"><span data-stu-id="81a2d-132">If you're using Azure Cosmos DB toobuild a game, you can, for example, use change feed tooimplement real-time leaderboards based on scores from completed games.</span></span>

## <a name="how-change-feed-works-in-azure-cosmos-db"></a><span data-ttu-id="81a2d-133">Jak funguje změnu informační kanál v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="81a2d-133">How change feed works in Azure Cosmos DB</span></span>
<span data-ttu-id="81a2d-134">Azure Cosmos DB poskytuje možnost tooincrementally hello číst aktualizace provedené tooan Azure Cosmos DB kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-134">Azure Cosmos DB provides hello ability tooincrementally read updates made tooan Azure Cosmos DB collection.</span></span> <span data-ttu-id="81a2d-135">Tento informační kanál změnu má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="81a2d-135">This change feed has hello following properties:</span></span>

* <span data-ttu-id="81a2d-136">Změny jsou trvalé v Azure Cosmos DB a může být zpracována asynchronně.</span><span class="sxs-lookup"><span data-stu-id="81a2d-136">Changes are persistent in Azure Cosmos DB and can be processed asynchronously.</span></span>
* <span data-ttu-id="81a2d-137">Toodocuments změny v kolekci jsou k dispozici okamžitě v kanálu změnu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-137">Changes toodocuments within a collection are available immediately in hello change feed.</span></span>
* <span data-ttu-id="81a2d-138">Každý dokument tooa změn se zobrazí právě jednou v kanálu změnu hello a klienti spravovat své logiku vytváření kontrolních bodů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-138">Each change tooa document appears exactly once in hello change feed, and clients manage their checkpointing logic.</span></span> <span data-ttu-id="81a2d-139">Knihovna informačního kanálu procesoru změn Hello poskytuje automatické vytváření kontrolních bodů a "alespoň jednou" sémantiku.</span><span class="sxs-lookup"><span data-stu-id="81a2d-139">hello change feed processor library provides automatic checkpointing and "at least once" semantics.</span></span>
* <span data-ttu-id="81a2d-140">Protokol změn hello je součástí pouze poslední změny hello u daného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-140">Only hello most recent change for a given document is included in hello change log.</span></span> <span data-ttu-id="81a2d-141">Přechodných změn nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81a2d-141">Intermediate changes may not be available.</span></span>
* <span data-ttu-id="81a2d-142">informační kanál změnu Hello je řazen u úpravy v rámci každé hodnotu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-142">hello change feed is sorted by order of modification within each partition key value.</span></span> <span data-ttu-id="81a2d-143">Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-143">There is no guaranteed order across partition-key values.</span></span>
* <span data-ttu-id="81a2d-144">Změny mohou být synchronizovány z jakékoli bodu v čase, tedy neexistuje žádné období uchovávání pevné dat, pro které jsou k dispozici změny.</span><span class="sxs-lookup"><span data-stu-id="81a2d-144">Changes can be synchronized from any point-in-time, that is, there is no fixed data retention period for which changes are available.</span></span>
* <span data-ttu-id="81a2d-145">Změny jsou k dispozici v bloky rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-145">Changes are available in chunks of partition key ranges.</span></span> <span data-ttu-id="81a2d-146">Tato možnost umožňuje změny z toobe rozsáhlých kolekcí, které jsou zpracovávány paralelně více příjemci nebo serverů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-146">This capability allows changes from large collections toobe processed in parallel by multiple consumers/servers.</span></span>
* <span data-ttu-id="81a2d-147">Aplikace můžete žádost o změnu více informačních kanálů současně na hello stejné kolekci.</span><span class="sxs-lookup"><span data-stu-id="81a2d-147">Applications can request for multiple change feeds simultaneously on hello same collection.</span></span>

<span data-ttu-id="81a2d-148">Informační kanál Azure Cosmos DB změnu je povoleno ve výchozím nastavení pro všechny účty.</span><span class="sxs-lookup"><span data-stu-id="81a2d-148">Azure Cosmos DB's change feed is enabled by default for all accounts.</span></span> <span data-ttu-id="81a2d-149">Můžete použít vaše [zřízené propustnosti](request-units.md) ve vaší oblasti zápisu ani žádné [číst oblast](distribute-data-globally.md) tooread z hello změnit informačního kanálu, stejně jako všechny ostatní operace z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81a2d-149">You can use your [provisioned throughput](request-units.md) in your write region or any [read region](distribute-data-globally.md) tooread from hello change feed, just like any other operation from Azure Cosmos DB.</span></span> <span data-ttu-id="81a2d-150">informační kanál změnu Hello zahrnuje vložení a operace aktualizace provedené toodocuments v rámci kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-150">hello change feed includes inserts and update operations made toodocuments within hello collection.</span></span> <span data-ttu-id="81a2d-151">Můžete zaznamenat odstranění nastavením příznak "soft odstranění" v rámci dokumentů místo odstranění.</span><span class="sxs-lookup"><span data-stu-id="81a2d-151">You can capture deletes by setting a "soft-delete" flag within your documents in place of deletes.</span></span> <span data-ttu-id="81a2d-152">Alternativně můžete nastavit konečný platnosti pro dokumentů prostřednictvím hello [TTL schopností](time-to-live.md)pro příklad, 24 hodin a použití hello hodnota této vlastnosti toocapture odstraní.</span><span class="sxs-lookup"><span data-stu-id="81a2d-152">Alternatively, you can set a finite expiration period for your documents via hello [TTL capability](time-to-live.md), for example, 24 hours and use hello value of that property toocapture deletes.</span></span> <span data-ttu-id="81a2d-153">Toto řešení máte tooprocess změny v časovém intervalu kratší než období platnosti TTL hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-153">With this solution, you have tooprocess changes within a shorter time interval than hello TTL expiration period.</span></span> <span data-ttu-id="81a2d-154">informační kanál změnu Hello je k dispozici pro každý oddíl klíče rozsahem v rámci kolekce dokumentů hello a proto mohou být distribuovány na jeden nebo více příjemce pro paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="81a2d-154">hello change feed is available for each partition key range within hello document collection, and thus can be distributed across one or more consumers for parallel processing.</span></span> 

![Distribuované zpracování změn Azure Cosmos DB kanálu](./media/change-feed/changefeedvisual.png)

<span data-ttu-id="81a2d-156">Máte několik možností v tom, jak implementovat změnu kanálu v klientském kódu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-156">You have a few options in how you implement a change feed in your client code.</span></span> <span data-ttu-id="81a2d-157">Hello částech, která okamžitě postupujte podle popisují, jak tooimplement hello změnu kanálu pomocí hello REST API služby Azure Cosmos DB a hello DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="81a2d-157">hello sections that immediately follow describe how tooimplement hello change feed using hello Azure Cosmos DB REST API and hello DocumentDB SDKs.</span></span> <span data-ttu-id="81a2d-158">Ale pro aplikace .NET, doporučujeme používat hello nové [změnu kanálu procesoru knihovna](#change-feed-processor) pro zpracování událostí z hello změnit kanálu jako jeho zjednodušuje čtení změny mezi oddílů a umožňuje více podprocesů práce paralelní.</span><span class="sxs-lookup"><span data-stu-id="81a2d-158">However, for .NET applications, we recommend using hello new [Change feed processor library](#change-feed-processor) for processing events from hello change feed as it simplifies reading changes across partitions and enables multiple threads working in parallel.</span></span> 

## <span data-ttu-id="81a2d-159"><a id="rest-apis"></a>Práce s hello REST API a DocumentDB SDK</span><span class="sxs-lookup"><span data-stu-id="81a2d-159"><a id="rest-apis"></a>Working with hello REST API and DocumentDB SDKs</span></span>
<span data-ttu-id="81a2d-160">Azure Cosmos DB poskytuje elastické kontejnerů úložiště a propustnost názvem **kolekce**.</span><span class="sxs-lookup"><span data-stu-id="81a2d-160">Azure Cosmos DB provides elastic containers of storage and throughput called **collections**.</span></span> <span data-ttu-id="81a2d-161">Data v rámci kolekce je logicky seskupeny pomocí [oddílu klíče](partition-data.md) a výkon a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="81a2d-161">Data within collections is logically grouped using [partition keys](partition-data.md) for scalability and performance.</span></span> <span data-ttu-id="81a2d-162">Azure Cosmos DB poskytuje různé rozhraní API pro přístup k těmto datům, včetně vyhledávání podle ID (pro čtení nebo získat), dotazů a číst informační kanály (kontroly).</span><span class="sxs-lookup"><span data-stu-id="81a2d-162">Azure Cosmos DB provides various APIs for accessing this data, including lookup by ID (Read/Get), query, and read-feeds (scans).</span></span> <span data-ttu-id="81a2d-163">Hello změnu kanálu je možné získat naplnění dva nové žádosti o hlavičky toohello DocumentDB `ReadDocumentFeed` rozhraní API, může zpracovat paralelní napříč rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-163">hello change feed can be obtained by populating two new request headers toohello DocumentDB `ReadDocumentFeed` API, and can be processed in parallel across ranges of partition keys.</span></span>

### <a name="readdocumentfeed-api"></a><span data-ttu-id="81a2d-164">ReadDocumentFeed rozhraní API</span><span class="sxs-lookup"><span data-stu-id="81a2d-164">ReadDocumentFeed API</span></span>
<span data-ttu-id="81a2d-165">Podívejme se stručný v tom, jak funguje ReadDocumentFeed.</span><span class="sxs-lookup"><span data-stu-id="81a2d-165">Let's take a brief look at how ReadDocumentFeed works.</span></span> <span data-ttu-id="81a2d-166">Azure Cosmos DB podporuje čtení informačního kanálu dokumentů v rámci kolekce prostřednictvím hello `ReadDocumentFeed` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="81a2d-166">Azure Cosmos DB supports reading a feed of documents within a collection via hello `ReadDocumentFeed` API.</span></span> <span data-ttu-id="81a2d-167">Například hello následující požadavek vrátí stránku dokumenty uvnitř hello `serverlogs` kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-167">For example, hello following request returns a page of documents inside hello `serverlogs` collection.</span></span> 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="81a2d-168">Výsledky mohou být omezeny pomocí hello `x-ms-max-item-count` záhlaví a čtení lze obnovit pomocí opětovným odesláním požadavku hello s `x-ms-continuation` záhlaví, vrátí se v předchozí odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-168">Results can be limited by using hello `x-ms-max-item-count` header, and reads can be resumed by resubmitting hello request with a `x-ms-continuation` header returned in hello previous response.</span></span> <span data-ttu-id="81a2d-169">Při provádění z jednoho klienta, `ReadDocumentFeed` iteruje výsledky napříč oddíly sériově.</span><span class="sxs-lookup"><span data-stu-id="81a2d-169">When performed from a single client, `ReadDocumentFeed` iterates through results across partitions serially.</span></span> 

<span data-ttu-id="81a2d-170">**Sériové čtení dokumentu kanálu**</span><span class="sxs-lookup"><span data-stu-id="81a2d-170">**Serial read document feed**</span></span>

<span data-ttu-id="81a2d-171">Můžete také načíst informační kanál hello dokumentů pomocí jedné z hello podporované [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="81a2d-171">You can also retrieve hello feed of documents using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="81a2d-172">Například hello následující fragment kódu ukazuje, jak toouse hello [ReadDocumentFeedAsync metoda](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="81a2d-172">For example, hello following snippet shows how toouse hello [ReadDocumentFeedAsync method](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) in .NET.</span></span>

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a><span data-ttu-id="81a2d-173">Distribuované provádění ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="81a2d-173">Distributed execution of ReadDocumentFeed</span></span>
<span data-ttu-id="81a2d-174">Pro kolekce, které obsahují terabajtů dat nebo víc nebo ingestování velký objem aktualizací nemusí být praktické sériové provádění čtení kanálu z jednoho klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="81a2d-174">For collections that contain terabytes of data or more, or ingest a large volume of updates, serial execution of read feed from a single client machine might not be practical.</span></span> <span data-ttu-id="81a2d-175">V pořadí toosupport tyto scénáře velkých objemů dat, Azure Cosmos DB poskytuje rozhraní API toodistribute `ReadDocumentFeed` volání transparentně napříč více klienta čtečky na příjemců.</span><span class="sxs-lookup"><span data-stu-id="81a2d-175">In order toosupport these big data scenarios, Azure Cosmos DB provides APIs toodistribute `ReadDocumentFeed` calls transparently across multiple client readers/consumers.</span></span> 

<span data-ttu-id="81a2d-176">**Informační kanál pro distribuované čtení dokumentu**</span><span class="sxs-lookup"><span data-stu-id="81a2d-176">**Distributed Read Document Feed**</span></span>

<span data-ttu-id="81a2d-177">tooprovide škálovatelné zpracování přírůstkové změny, Azure Cosmos DB podporuje model Škálováním na více systémů pro změnu hello kanálu rozhraní API založené na rozsahy klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-177">tooprovide scalable processing of incremental changes, Azure Cosmos DB supports a scale-out model for hello change feed API based on ranges of partition keys.</span></span>

* <span data-ttu-id="81a2d-178">Můžete získat seznam oddílu rozsahy klíčů pro kolekci provádění `ReadPartitionKeyRanges` volání.</span><span class="sxs-lookup"><span data-stu-id="81a2d-178">You can obtain a list of partition key ranges for a collection performing a `ReadPartitionKeyRanges` call.</span></span> 
* <span data-ttu-id="81a2d-179">Pro každý rozsah klíče oddílu můžete provádět `ReadDocumentFeed` tooread dokumentů pomocí klíčů oddílů v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-179">For each partition key range, you can perform a `ReadDocumentFeed` tooread documents with partition keys within that range.</span></span>

### <a name="retrieving-partition-key-ranges-for-a-collection"></a><span data-ttu-id="81a2d-180">Načítání oblastí klíče oddílu pro kolekci</span><span class="sxs-lookup"><span data-stu-id="81a2d-180">Retrieving partition key ranges for a collection</span></span>
<span data-ttu-id="81a2d-181">Rozsahy klíčů hello oddílu můžete načíst pomocí požadavku hello `pkranges` prostředků v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-181">You can retrieve hello partition key ranges by requesting hello `pkranges` resource within a collection.</span></span> <span data-ttu-id="81a2d-182">Například hello následující požadavek načte seznam hello rozsahy klíčů oddílu pro hello `serverlogs` kolekce:</span><span class="sxs-lookup"><span data-stu-id="81a2d-182">For example hello following request retrieves hello list of partition key ranges for hello `serverlogs` collection:</span></span>

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

<span data-ttu-id="81a2d-183">Tento požadavek vrátí hello následující odpověď obsahující metadata o rozsahy klíčů oddílu hello:</span><span class="sxs-lookup"><span data-stu-id="81a2d-183">This request returns hello following response containing metadata about hello partition key ranges:</span></span>

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


<span data-ttu-id="81a2d-184">**Oddílu Vlastnosti klíčové oblasti**: každý oddíl klíče rozsah obsahuje vlastnosti metadat hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="81a2d-184">**Partition key range properties**: Each partition key range includes hello metadata properties in hello following table:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="81a2d-185">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="81a2d-185">Header name</span></span></th>
        <th><span data-ttu-id="81a2d-186">Popis</span><span class="sxs-lookup"><span data-stu-id="81a2d-186">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-187">id</span><span class="sxs-lookup"><span data-stu-id="81a2d-187">id</span></span></td>
        <td>
            <p><span data-ttu-id="81a2d-188">Hello ID pro rozsah klíče oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-188">hello ID for hello partition key range.</span></span> <span data-ttu-id="81a2d-189">Toto je stabilní a jedinečné ID v každé kolekci.</span><span class="sxs-lookup"><span data-stu-id="81a2d-189">This is a stable and unique ID within each collection.</span></span></p>
            <p><span data-ttu-id="81a2d-190">Musí být použít v hello následující volání tooread změny podle rozsahu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-190">Must be used in hello following call tooread changes by partition key range.</span></span></p>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-191">maxExclusive</span><span class="sxs-lookup"><span data-stu-id="81a2d-191">maxExclusive</span></span></td>
        <td><span data-ttu-id="81a2d-192">Hodnota klíče hash Hello maximální oddílu pro rozsah klíče oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-192">hello maximum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="81a2d-193">Pro interní použití.</span><span class="sxs-lookup"><span data-stu-id="81a2d-193">For internal use.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-194">minInclusive</span><span class="sxs-lookup"><span data-stu-id="81a2d-194">minInclusive</span></span></td>
        <td><span data-ttu-id="81a2d-195">Hello oddíl minimální hodnota hash klíče hodnotu rozsahu klíče oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-195">hello minimum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="81a2d-196">Pro interní použití.</span><span class="sxs-lookup"><span data-stu-id="81a2d-196">For internal use.</span></span></td>
    </tr>       
</table>

<span data-ttu-id="81a2d-197">To provedete pomocí jedné z hello podporované [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="81a2d-197">You can do this using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="81a2d-198">Například hello následující fragment kódu ukazuje, jak klíč oddílu tooretrieve rozsahy v rozhraní .NET pomocí hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metoda.</span><span class="sxs-lookup"><span data-stu-id="81a2d-198">For example, hello following snippet shows how tooretrieve partition key ranges in .NET using hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) method.</span></span>

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

<span data-ttu-id="81a2d-199">Azure Cosmos DB podporuje načtení dokumentů na rozsah klíče oddílu podle nastavení hello volitelné `x-ms-documentdb-partitionkeyrangeid` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="81a2d-199">Azure Cosmos DB supports retrieval of documents per partition key range by setting hello optional `x-ms-documentdb-partitionkeyrangeid` header.</span></span> 

### <a name="performing-an-incremental-readdocumentfeed"></a><span data-ttu-id="81a2d-200">Provádění přírůstkové ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="81a2d-200">Performing an incremental ReadDocumentFeed</span></span>
<span data-ttu-id="81a2d-201">ReadDocumentFeed podporuje následující scénáře a úkoly pro přírůstkové zpracování změny v kolekcích Azure Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="81a2d-201">ReadDocumentFeed supports hello following scenarios/tasks for incremental processing of changes in Azure Cosmos DB collections:</span></span>

* <span data-ttu-id="81a2d-202">Číst všechny změny toodocuments od začátku hello, to znamená, od vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-202">Read all changes toodocuments from hello beginning, that is, from collection creation.</span></span>
* <span data-ttu-id="81a2d-203">Číst všechny změny toofuture aktualizace toodocuments z aktuální čas nebo změny od čas zadaného uživatelem.</span><span class="sxs-lookup"><span data-stu-id="81a2d-203">Read all changes toofuture updates toodocuments from current time, or any changes since a user-specified time.</span></span>
* <span data-ttu-id="81a2d-204">Číst všechny změny toodocuments z verze logické kolekce hello (ETag).</span><span class="sxs-lookup"><span data-stu-id="81a2d-204">Read all changes toodocuments from a logical version of hello collection (ETag).</span></span> <span data-ttu-id="81a2d-205">Můžete kontrolního bodu uživatele podle hello vrácená značka ETag přírůstkové požadavky kanálu pro čtení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-205">You can checkpoint your consumers based on hello returned ETag from incremental read-feed requests.</span></span>

<span data-ttu-id="81a2d-206">Hello změny zahrnují toodocuments operace INSERT a Update.</span><span class="sxs-lookup"><span data-stu-id="81a2d-206">hello changes include inserts and updates toodocuments.</span></span> <span data-ttu-id="81a2d-207">Odstraní toocapture, musíte použít vlastnost "obnovitelného odstranění" v rámci dokumentů, nebo použijte hello [předdefinované vlastnosti TTL](time-to-live.md) toosignal čeká na odstranění ve hello změnit informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-207">toocapture deletes, you must use a "soft delete" property within your documents, or use hello [built-in TTL property](time-to-live.md) toosignal a pending deletion in hello change feed.</span></span>

<span data-ttu-id="81a2d-208">Hello následující tabulky seznamy hello [požadavku](/rest/api/documentdb/common-documentdb-rest-request-headers.md) a [hlavičky odpovědi](/rest/api/documentdb/common-documentdb-rest-response-headers.md) pro ReadDocumentFeed operace.</span><span class="sxs-lookup"><span data-stu-id="81a2d-208">hello following table lists hello [request](/rest/api/documentdb/common-documentdb-rest-request-headers.md) and [response headers](/rest/api/documentdb/common-documentdb-rest-response-headers.md) for ReadDocumentFeed operations.</span></span>

<span data-ttu-id="81a2d-209">**Hlavičky požadavku pro přírůstkové ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="81a2d-209">**Request headers for incremental ReadDocumentFeed**:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="81a2d-210">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="81a2d-210">Header name</span></span></th>
        <th><span data-ttu-id="81a2d-211">Popis</span><span class="sxs-lookup"><span data-stu-id="81a2d-211">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-212">A ZASÍLÁNÍ RYCHLÝCH ZPRÁV</span><span class="sxs-lookup"><span data-stu-id="81a2d-212">A-IM</span></span></td>
        <td><span data-ttu-id="81a2d-213">Musí být nastaven příliš "Přírůstkové kanálu", nebo tento parametr vynechán jinak</span><span class="sxs-lookup"><span data-stu-id="81a2d-213">Must be set too"Incremental feed", or omitted otherwise</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-214">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="81a2d-214">If-None-Match</span></span></td>
        <td>
            <p><span data-ttu-id="81a2d-215">Žádné záhlaví: vrátí všechny změny z hello od (vytvoření kolekce)</span><span class="sxs-lookup"><span data-stu-id="81a2d-215">No header: returns all changes from hello beginning (collection creation)</span></span></p>
            <p><span data-ttu-id="81a2d-216">"*": vrátí všechny nové toodata změny v kolekci hello</span><span class="sxs-lookup"><span data-stu-id="81a2d-216">"*": returns all new changes toodata within hello collection</span></span></p>         
            <p><span data-ttu-id="81a2d-217">&lt;Značka Etag&gt;: Pokud nastavit tooa kolekce značka ETag, vrátí všechny změny provedené od této logické časové razítko</span><span class="sxs-lookup"><span data-stu-id="81a2d-217">&lt;etag&gt;: If set tooa collection ETag, returns all changes made since that logical timestamp</span></span></p>
        </td>
    </tr>
    <tr>    
        <td><span data-ttu-id="81a2d-218">Pokud upravit – od</span><span class="sxs-lookup"><span data-stu-id="81a2d-218">If-Modified-Since</span></span></td> 
        <td><span data-ttu-id="81a2d-219">Formát času RFC 1123; Pokud je zadána If-None-Match ignorována</span><span class="sxs-lookup"><span data-stu-id="81a2d-219">RFC 1123 time format; ignored if If-None-Match is specified</span></span></td> 
    </tr> 
    <tr>
        <td><span data-ttu-id="81a2d-220">x-ms-documentdb-partitionkeyrangeid</span><span class="sxs-lookup"><span data-stu-id="81a2d-220">x-ms-documentdb-partitionkeyrangeid</span></span></td>
        <td><span data-ttu-id="81a2d-221">ID klíče rozsahu oddílu Hello pro čtení dat</span><span class="sxs-lookup"><span data-stu-id="81a2d-221">hello partition key range ID for reading data.</span></span></td>
    </tr>
</table>

<span data-ttu-id="81a2d-222">**Hlavičky odpovědi pro přírůstkové ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="81a2d-222">**Response headers for incremental ReadDocumentFeed**:</span></span>

<table> <tr>
        <th><span data-ttu-id="81a2d-223">Název hlavičky</span><span class="sxs-lookup"><span data-stu-id="81a2d-223">Header name</span></span></th>
        <th><span data-ttu-id="81a2d-224">Popis</span><span class="sxs-lookup"><span data-stu-id="81a2d-224">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-225">Značka Etag</span><span class="sxs-lookup"><span data-stu-id="81a2d-225">etag</span></span></td>
        <td>
            <p><span data-ttu-id="81a2d-226">Hello logické pořadové číslo (položky LSN) poslední dokumentu vrácený v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-226">hello logical sequence number (LSN) of last document returned in hello response.</span></span></p>
            <p><span data-ttu-id="81a2d-227">Přírůstkové ReadDocumentFeed můžete obnovit pomocí opětovným odesláním tuto hodnotu v If-None-Match.</span><span class="sxs-lookup"><span data-stu-id="81a2d-227">Incremental ReadDocumentFeed can be resumed by resubmitting this value in If-None-Match.</span></span></p>
        </td>
    </tr>
</table>

<span data-ttu-id="81a2d-228">Tady je tooreturn požadavku ukázkové všechny přírůstkové změny v kolekci z ETag logické verze hello `28535` a rozdělit na oddíly klíče rozsah = `16`:</span><span class="sxs-lookup"><span data-stu-id="81a2d-228">Here's a sample request tooreturn all incremental changes in collection from hello logical version/ETag `28535` and partition key range = `16`:</span></span>

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

<span data-ttu-id="81a2d-229">Změny jsou seřazené podle času v rámci každé hodnotu klíče oddílu v rozsahu klíče oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-229">Changes are ordered by time within each partition key value within hello partition key range.</span></span> <span data-ttu-id="81a2d-230">Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-230">There is no guaranteed order across partition-key values.</span></span> <span data-ttu-id="81a2d-231">Pokud existují další výsledky, než lze zobrazit v jediné stránce, si můžete přečíst další stránky výsledků hello podle opětovným odesláním požadavku hello s hello `If-None-Match` hlavička s hodnota rovna toohello `etag` z předchozí odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-231">If there are more results than can fit in a single page, you can read hello next page of results by resubmitting hello request with hello `If-None-Match` header with value equal toohello `etag` from hello previous response.</span></span> <span data-ttu-id="81a2d-232">Pokud více dokumentů se přidají nebo aktualizují transakčně v rámci uložené procedury nebo aktivační událost, budou všechny vrátí v rámci hello stejné stránky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="81a2d-232">If multiple documents were inserted or updated transactionally within a stored procedure or trigger, they will all be returned within hello same response page.</span></span>

> [!NOTE]
> <span data-ttu-id="81a2d-233">S změnu kanálu, může získat více položek vrátí na stránce, než je zadáno v `x-ms-max-item-count` v případě hello více dokumentů, které se přidají nebo aktualizují v rámci uložené procedury nebo aktivační události.</span><span class="sxs-lookup"><span data-stu-id="81a2d-233">With change feed, you might get more items returned in a page than specified in `x-ms-max-item-count` in hello case of multiple documents inserted or updated inside a stored procedures or triggers.</span></span> 

<span data-ttu-id="81a2d-234">Pokud používáte hello .NET SDK (1.17.0), nastavte pole hello `StartTime` v `ChangeFeedOptions` toodirectly návratový změnit dokumenty od `StartTime` při volání metody `CreateDocumentChangeFeedQuery`.</span><span class="sxs-lookup"><span data-stu-id="81a2d-234">When using hello .NET SDK (1.17.0), set hello field `StartTime` in `ChangeFeedOptions` toodirectly return changed documents since `StartTime` when calling  `CreateDocumentChangeFeedQuery`.</span></span> <span data-ttu-id="81a2d-235">Zadáním `If-Modified-Since` pomocí hello REST API, vaši žádost o vrátí není hello dokumenty sami, ale spíš token pokračování hello nebo `etag` v hello hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="81a2d-235">By specifying `If-Modified-Since` using hello REST API, your request will return not hello documents themselves, but rather hello continuation token or `etag` in hello response header.</span></span> <span data-ttu-id="81a2d-236">dokumenty hello tooreturn upravené hello zadaný čas, token pokračování hello `etag` pak použije v hello další žádosti se `If-None-Match` tooreturn hello skutečné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="81a2d-236">tooreturn hello documents modified hello specified time, hello continuation token `etag` must then be used in hello next request with `If-None-Match` tooreturn hello actual documents.</span></span> 

<span data-ttu-id="81a2d-237">Hello .NET SDK poskytuje hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) a [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) pomocné rutiny třídy kolekce tooa tooaccess změny.</span><span class="sxs-lookup"><span data-stu-id="81a2d-237">hello .NET SDK provides hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) and [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper classes tooaccess changes made tooa collection.</span></span> <span data-ttu-id="81a2d-238">Hello následující fragment kódu ukazuje, jak tooretrieve všechny změny ze začátku hello pomocí .NET SDK hello z jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="81a2d-238">hello following snippet shows how tooretrieve all changes from hello beginning using hello .NET SDK from a single client.</span></span>

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
<span data-ttu-id="81a2d-239">A hello následující fragment kódu ukazuje, jak tooprocess změny v reálném čase s Azure DB Cosmos pomocí hello změnu kanálu podporu a hello předcházející funkce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-239">And hello following snippet shows how tooprocess changes in real-time with Azure Cosmos DB by using hello change feed support and hello preceding function.</span></span> <span data-ttu-id="81a2d-240">první volání Hello vrátí všechny dokumenty hello v kolekci hello a hello druhý pouze vrátí hello dva dokumenty vytvořené, které byly vytvořeny od posledního kontrolního bodu hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-240">hello first call returns all hello documents in hello collection, and hello second only returns hello two documents created that were created since hello last checkpoint.</span></span>

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

<span data-ttu-id="81a2d-241">Můžete také filtrovat hello změnu kanálu pomocí události procesu tooselectively logiku na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="81a2d-241">You can also filter hello change feed using client side logic tooselectively process events.</span></span> <span data-ttu-id="81a2d-242">Například zde je fragment, používající klienta straně LINQ tooprocess jen teploty události změn ze senzorů zařízení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-242">For example, here's a snippet that uses client side LINQ tooprocess only temperature change events from device sensors.</span></span>

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <span data-ttu-id="81a2d-243"><a id="change-feed-processor"></a>Knihovna kanálu procesoru změn</span><span class="sxs-lookup"><span data-stu-id="81a2d-243"><a id="change-feed-processor"></a>Change Feed Processor library</span></span>
<span data-ttu-id="81a2d-244">Další možností je toouse hello [knihovny Azure Cosmos DB změnit kanálu procesoru](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), který vám může pomoct snadno distribuovat zpracování z důvodu změny kanálu napříč více příjemců událostí.</span><span class="sxs-lookup"><span data-stu-id="81a2d-244">Another option is toouse hello [Azure Cosmos DB Change Feed Processor library](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), which can help you easily distribute event processing from a change feed across multiple consumers.</span></span> <span data-ttu-id="81a2d-245">Knihovna Hello je skvěle hodí k vytváření změnu kanálu čtečky na platformě .NET hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-245">hello library is great for building change feed readers on hello .NET platform.</span></span> <span data-ttu-id="81a2d-246">Některé pracovní postupy, které by zjednodušit pomocí knihovny změnu kanálu procesoru hello prostřednictvím metody hello součástí hello dalších sadách SDK DB Cosmos patří:</span><span class="sxs-lookup"><span data-stu-id="81a2d-246">Some workflows that would be simplified by using hello Change Feed Processor library over hello methods included in hello other Cosmos DB SDKs include:</span></span> 

* <span data-ttu-id="81a2d-247">Vyžádání aktualizace z změnu informačního kanálu, když data budou uložená napříč více oddílů</span><span class="sxs-lookup"><span data-stu-id="81a2d-247">Pulling updates from change feed when data is stored across multiple partitions</span></span>
* <span data-ttu-id="81a2d-248">Přesunutí nebo replikace dat z jedné kolekce tooanother</span><span class="sxs-lookup"><span data-stu-id="81a2d-248">Moving or replicating data from one collection tooanother</span></span>
* <span data-ttu-id="81a2d-249">Paralelní provádění akcí aktivovány toodata aktualizace a změny kanálu</span><span class="sxs-lookup"><span data-stu-id="81a2d-249">Parallel execution of actions triggered by updates toodata and change feed</span></span> 

<span data-ttu-id="81a2d-250">Při použití hello rozhraní API v hello Cosmos SDK poskytuje přesné přístup toochange kanálu aktualizace v každém oddílu, pomocí knihovny změnu kanálu procesoru hello zjednodušuje čtení změny mezi oddílů a paralelně fungujících více vláken.</span><span class="sxs-lookup"><span data-stu-id="81a2d-250">While using hello APIs in hello Cosmos SDKs provides precise access toochange feed updates in each partition, using hello Change Feed Processor library simplifies reading changes across partitions and multiple threads working in parallel.</span></span> <span data-ttu-id="81a2d-251">Místo ručně čtení změny z každý kontejner a ukládání token pokračování pro každý oddíl hello změnu kanálu procesoru automaticky spravuje čtení změny napříč oddíly používající mechanismus zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-251">Instead of manually reading changes from each container and saving a continuation token for each partition, hello Change Feed Processor automatically manages reading changes across partitions using a lease mechanism.</span></span>

<span data-ttu-id="81a2d-252">Knihovna Hello je k dispozici jako balíčku NuGet: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) a ze zdrojového kódu jako Githubu [ukázka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span><span class="sxs-lookup"><span data-stu-id="81a2d-252">hello library is available as a NuGet Package: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) and from source code as a Github [sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span></span> 

### <a name="understanding-change-feed-processor-library"></a><span data-ttu-id="81a2d-253">Principy změn kanálu procesoru knihovny</span><span class="sxs-lookup"><span data-stu-id="81a2d-253">Understanding Change Feed Processor library</span></span> 

<span data-ttu-id="81a2d-254">Existují čtyři hlavní součásti implementace hello změnu kanálu procesoru: hello monitorovat kolekce, kolekce hello zapůjčení, hello procesoru hostitele a spotřebitelé hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-254">There are four main components of implementing hello Change Feed Processor: hello monitored collection, hello lease collection, hello processor host, and hello consumers.</span></span> 

<span data-ttu-id="81a2d-255">**Monitorované kolekci:** hello monitorovat kolekce je hello dat, ze které hello se generuje změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-255">**Monitored Collection:** hello monitored collection is hello data from which hello change feed is generated.</span></span> <span data-ttu-id="81a2d-256">Jakékoli kolekce toohello monitorovat vložení a změny se projeví v kanálu změnu hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-256">Any inserts and changes toohello monitored collection are reflected in hello change feed of hello collection.</span></span> 

<span data-ttu-id="81a2d-257">**Kolekce zapůjčení:** hello souřadnice kolekce zapůjčení zpracování změn hello kanálu napříč více pracovníků.</span><span class="sxs-lookup"><span data-stu-id="81a2d-257">**Lease Collection:** hello lease collection coordinates processing hello change feed across multiple workers.</span></span> <span data-ttu-id="81a2d-258">Samostatné kolekce je použité toostore hello zapůjčení jeden zapůjčení na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="81a2d-258">A separate collection is used toostore hello leases with one lease per partition.</span></span> <span data-ttu-id="81a2d-259">Je výhodné toostore zápisu této kolekce zapůjčení na jiný účet s hello oblast blíže toowhere hello, které běží změnu kanálu procesoru.</span><span class="sxs-lookup"><span data-stu-id="81a2d-259">It is advantageous toostore this lease collection on a different account with hello write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="81a2d-260">Objekt zapůjčení obsahuje hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="81a2d-260">A lease object contains hello following attributes:</span></span> 
* <span data-ttu-id="81a2d-261">Vlastník: Určuje hello hostitele, který vlastní zapůjčení hello</span><span class="sxs-lookup"><span data-stu-id="81a2d-261">Owner: Specifies hello host that owns hello lease</span></span>
* <span data-ttu-id="81a2d-262">Pokračování: Určuje pozici hello (token pokračování) v kanálu pro určitý oddíl změn hello</span><span class="sxs-lookup"><span data-stu-id="81a2d-262">Continuation: Specifies hello position (continuation token) in hello change feed for a particular partition</span></span>
* <span data-ttu-id="81a2d-263">Časové razítko: Čas poslední zapůjčení byla aktualizována; časové razítko Hello může být použité toocheck, zda text hello zapůjčení je považována za ukončenou</span><span class="sxs-lookup"><span data-stu-id="81a2d-263">Timestamp: Last time lease was updated; hello timestamp can be used toocheck whether hello lease is considered expired</span></span> 

<span data-ttu-id="81a2d-264">**Procesor hostitele:** každého hostitele Určuje, kolik tooprocess oddíly na základě kolik instancí hostitelů mají aktivní zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-264">**Processor Host:** Each host determines how many partitions tooprocess based on how many other instances of hosts have active leases.</span></span> 
1.  <span data-ttu-id="81a2d-265">Po spuštění hostitele získá zapůjčení toobalance hello zatížení ve všech hostitelích.</span><span class="sxs-lookup"><span data-stu-id="81a2d-265">When a host starts up, it acquires leases toobalance hello workload across all hosts.</span></span> <span data-ttu-id="81a2d-266">Hostitel pravidelně obnovuje zapůjčení, aby zůstala aktivní zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-266">A host periodically renews leases, so leases remain active.</span></span> 
2.  <span data-ttu-id="81a2d-267">Hostitele kontrolní body hello poslední pokračovací token tooits zapůjčení pro každý čtení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-267">A host checkpoints hello last continuation token tooits lease for each read.</span></span> <span data-ttu-id="81a2d-268">tooensure souběžnosti zabezpečení, hostitel ověří hello ETag pro jednotlivé aktualizace zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-268">tooensure concurrency safety, a host checks hello ETag for each lease update.</span></span> <span data-ttu-id="81a2d-269">Jsou podporovány také další strategie kontrolního bodu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-269">Other checkpoint strategies are also supported.</span></span>  
3.  <span data-ttu-id="81a2d-270">Při vypnutí počítače Hostitel uvolní všechny zapůjčení ale udržuje hello pokračování informace, takže ho můžete obnovit čtení z uložené kontrolního bodu hello později.</span><span class="sxs-lookup"><span data-stu-id="81a2d-270">Upon shutdown, a host releases all leases but keeps hello continuation information, so it can resume reading from hello stored checkpoint later.</span></span> 

<span data-ttu-id="81a2d-271">V tuto chvíli nemůže být větší než hello počet oddílů (zapůjčení) hello počtu hostitelů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-271">At this time hello number of hosts cannot be greater than hello number of partitions (leases).</span></span>

<span data-ttu-id="81a2d-272">**Příjemci knihovny:** příjemci nebo pracovníci, jsou vláken, které provádějí změnu hello kanálu zpracování iniciovaná každého hostitele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-272">**Consumers:** Consumers, or workers, are threads that perform hello change feed processing initiated by each host.</span></span> <span data-ttu-id="81a2d-273">Každý hostitel procesoru může mít více příjemců.</span><span class="sxs-lookup"><span data-stu-id="81a2d-273">Each processor host can have multiple consumers.</span></span> <span data-ttu-id="81a2d-274">Každý příjemce čte hello změnu kanálu z hello oddílu je přiřazen tooand upozorní jeho hostitel změny a platnost zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-274">Each consumer reads hello change feed from hello partition it is assigned tooand notifies its host of changes and expired leases.</span></span>

<span data-ttu-id="81a2d-275">toofurther pochopit, jak tyto čtyři prvky změnu kanálu procesoru spolupracují, podíváme se na příklad v hello následující diagram.</span><span class="sxs-lookup"><span data-stu-id="81a2d-275">toofurther understand how these four elements of Change Feed Processor work together, let's look at an example in hello following diagram.</span></span> <span data-ttu-id="81a2d-276">Hello monitorované kolekci úložiště dokumentů a jako klíč oddílu hello používá města"hello".</span><span class="sxs-lookup"><span data-stu-id="81a2d-276">hello monitored collection stores documents and uses hello "city" as hello partition key.</span></span> <span data-ttu-id="81a2d-277">Vidíte, že hello blue oddíl obsahuje dokumentů s hello pole "city" z "A-E" a tak dále.</span><span class="sxs-lookup"><span data-stu-id="81a2d-277">We see that hello blue partition contains documents with hello "city" field from "A-E" and so on.</span></span> <span data-ttu-id="81a2d-278">Existují dva hostitele, každý se dvěma spotřebiteli čtení z hello čtyřmi oddíly paralelně.</span><span class="sxs-lookup"><span data-stu-id="81a2d-278">There are two hosts, each with two consumers reading from hello four partitions in parallel.</span></span> <span data-ttu-id="81a2d-279">Hello šipky zobrazují hello příjemci čtení z na určité místo v hello změnit informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-279">hello arrows show hello consumers reading from a specific spot in hello change feed.</span></span> <span data-ttu-id="81a2d-280">V první oddíl hello představuje tmavšího blue hello nepřečtená změny při světla blue hello představuje hello již číst změny na změnu hello informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-280">In hello first partition, hello darker blue represents unread changes while hello light blue represents hello already read changes on hello change feed.</span></span> <span data-ttu-id="81a2d-281">hello zapůjčení kolekce toostore zaznamenávat pro hodnotu tookeep "pokračování" hello aktuální pozici pro každý příjemce čtení nastavení používají hostitelé Hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-281">hello hosts use hello lease collection toostore a "continuation" value tookeep track of hello current reading position for each consumer.</span></span> 

![Pomocí Azure Cosmos DB změnu hello kanálu procesor hostitele](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a><span data-ttu-id="81a2d-283">Pomocí změnu kanálu procesoru knihovny</span><span class="sxs-lookup"><span data-stu-id="81a2d-283">Using Change Feed Processor Library</span></span> 
<span data-ttu-id="81a2d-284">Hello následující část popisuje, jak toouse hello knihovny změnu kanálu procesoru v kontextu hello replikace změn z zdroj kolekce tooa cílové kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-284">hello following section explains how toouse hello Change Feed Processor library in hello context of replicating changes from a source collection tooa destination collection.</span></span> <span data-ttu-id="81a2d-285">Zde hello zdrojové kolekci je kolekce hello monitorovat procesor změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-285">Here, hello source collection is hello monitored collection in Change Feed Processor.</span></span> 

<span data-ttu-id="81a2d-286">**Nainstalovat a zahrnout balíček NuGet procesoru kanálu změnu hello**</span><span class="sxs-lookup"><span data-stu-id="81a2d-286">**Install and include hello Change Feed Processor NuGet package**</span></span> 

<span data-ttu-id="81a2d-287">Před instalací balíčku NuGet změnu kanálu procesoru, nejprve nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="81a2d-287">Before installing Change Feed Processor NuGet Package, first install:</span></span> 
* <span data-ttu-id="81a2d-288">Microsoft.Azure.DocumentDB, verze 1.13.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="81a2d-288">Microsoft.Azure.DocumentDB, version 1.13.1 or above</span></span> 
* <span data-ttu-id="81a2d-289">Newtonsoft.Json, verze 9.0.1 nebo vyšší instalace `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` a její zahrnutí jako odkaz.</span><span class="sxs-lookup"><span data-stu-id="81a2d-289">Newtonsoft.Json, version 9.0.1 or above Install `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` and include it as a reference.</span></span>

<span data-ttu-id="81a2d-290">**Vytvoření monitorovaných, zapůjčení a cílové kolekce**</span><span class="sxs-lookup"><span data-stu-id="81a2d-290">**Create a monitored, lease and destination collection**</span></span> 

<span data-ttu-id="81a2d-291">V pořadí toouse hello knihovna procesoru kanálu změn musí kolekce zapůjčení hello toobe vytvořit před spuštěním hello procesor hostitel.</span><span class="sxs-lookup"><span data-stu-id="81a2d-291">In order toouse hello Change Feed Processor Library, hello lease collection needs toobe created before running hello processor host(s).</span></span> <span data-ttu-id="81a2d-292">Znovu doporučujeme uložit kolekci zapůjčení na jiný účet s co nejblíže toowhere hello oblast zápisu, že změna kanálu procesor běží.</span><span class="sxs-lookup"><span data-stu-id="81a2d-292">Again, we recommend storing a lease collection on a different account with a write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="81a2d-293">V tomto příkladu přesun dat potřebujeme toocreate hello cílové kolekce před spuštěním hello změnu kanálu procesoru hostitele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-293">In this data movement example, we need toocreate hello destination collection before running hello Change Feed Processor host.</span></span> <span data-ttu-id="81a2d-294">V ukázkovém kódu hello říkáme hello Pomocná metoda toocreate monitorovat pronajaté a cílové kolekce, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="81a2d-294">In hello sample code we call a helper method toocreate hello monitored, leased, and destination collections if they do not already exist.</span></span> 

> [!WARNING]
> <span data-ttu-id="81a2d-295">Vytvoření kolekce hradí, jako jsou rezervování propustnost pro hello toocommunicate aplikací s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81a2d-295">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="81a2d-296">Další podrobnosti naleznete na adrese hello [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="81a2d-296">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

<span data-ttu-id="81a2d-297">*Vytvoření procesor hostitele*</span><span class="sxs-lookup"><span data-stu-id="81a2d-297">*Creating a processor host*</span></span>

<span data-ttu-id="81a2d-298">Hello `ChangeFeedProcessorHost` třída poskytuje bezpečné pro přístup z více vláken, více procesů, bezpečné běhového prostředí pro implementace zpracovatelů událostí taky poskytuje možnost vytváření kontrolních bodů a oddíl správu zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-298">hello `ChangeFeedProcessorHost` class provides a thread-safe, multi-process, safe runtime environment for event processor implementations that also provides checkpointing and partition lease management.</span></span> <span data-ttu-id="81a2d-299">toouse hello `ChangeFeedProcessorHost` třídu, můžete implementovat `IChangeFeedObserver`.</span><span class="sxs-lookup"><span data-stu-id="81a2d-299">toouse hello `ChangeFeedProcessorHost` class, you can implement `IChangeFeedObserver`.</span></span> <span data-ttu-id="81a2d-300">Toto rozhraní obsahuje tři metody:</span><span class="sxs-lookup"><span data-stu-id="81a2d-300">This interface contains three methods:</span></span>

* <span data-ttu-id="81a2d-301">`OpenAsync`: Tato funkce je volána, když je otevřen pozorovatel změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-301">`OpenAsync`: This function is called when change feed observer is opened.</span></span> <span data-ttu-id="81a2d-302">Po otevření příjemce/pozorovatel může být upravený tooperform určité akce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-302">It can be modified tooperform a specific action when consumer/observer is opened.</span></span>  
* <span data-ttu-id="81a2d-303">`CloseAsync`: Tato funkce je volána, když je ukončen informačního kanálu pozorovatel změnu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-303">`CloseAsync`: This function is called when change feed observer is terminated.</span></span> <span data-ttu-id="81a2d-304">Může být upravený tooperform určité akce při zavření příjemce/pozorovatele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-304">It can be modified tooperform a specific action when consumer/observer is closed.</span></span>  
* <span data-ttu-id="81a2d-305">`ProcessChangesAsync`: Tato funkce je volána, když jsou k dispozici na změnu kanálu nové změny dokumentu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-305">`ProcessChangesAsync`: This function is called when document new changes are available on change feed.</span></span> <span data-ttu-id="81a2d-306">Může to být upravené tooperform určité akce při každé aktualizaci změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-306">It can be modified tooperform a specific action upon every change feed update.</span></span>  

<span data-ttu-id="81a2d-307">V našem příkladu jsme implementovat rozhraní hello `IChangeFeedObserver` prostřednictvím hello `DocumentFeedObserver` třídy.</span><span class="sxs-lookup"><span data-stu-id="81a2d-307">In our example, we implement hello interface `IChangeFeedObserver` through hello `DocumentFeedObserver` class.</span></span> <span data-ttu-id="81a2d-308">Zde hello `ProcessChangesAsync` funkce upserts (aktualizace) dokumentu z změnu kanálu do hello cílové kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-308">Here, hello `ProcessChangesAsync` function upserts (updates) a document from change feed into hello destination collection.</span></span> <span data-ttu-id="81a2d-309">V tomto příkladu je užitečná pro přesun dat z jedné kolekce tooanother v pořadí toochange hello klíč oddílu datové sady.</span><span class="sxs-lookup"><span data-stu-id="81a2d-309">This example is useful for moving data from one collection tooanother in order toochange hello partition key of a data set.</span></span> 

<span data-ttu-id="81a2d-310">*Spuštění hello procesor hostitele*</span><span class="sxs-lookup"><span data-stu-id="81a2d-310">*Running hello Processor Host*</span></span>

<span data-ttu-id="81a2d-311">Před zahájením zpracování událostí, jak změnit možnosti informačního kanálu a změnit možnosti informačního kanálu hostitele je možné upravit.</span><span class="sxs-lookup"><span data-stu-id="81a2d-311">Before beginning event processing, both change feed options and change feed host options can be customized.</span></span> 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval too15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
<span data-ttu-id="81a2d-312">Hello konkrétních polí, které se dají přizpůsobit jsou shrnuté v následujících tabulkách hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-312">hello specific fields that can be customized are summarized in hello following tables.</span></span> 

<span data-ttu-id="81a2d-313">**Změna možností informačního kanálu**:</span><span class="sxs-lookup"><span data-stu-id="81a2d-313">**Change Feed Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="81a2d-314">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="81a2d-314">Property Name</span></span></th>
        <th><span data-ttu-id="81a2d-315">Popis</span><span class="sxs-lookup"><span data-stu-id="81a2d-315">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-316">MaxItemCount</span><span class="sxs-lookup"><span data-stu-id="81a2d-316">MaxItemCount</span></span></td>
        <td><span data-ttu-id="81a2d-317">Získá nebo nastaví maximální počet položek toobe, vrátí se v hello výčtu operace v hello služba databáze Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-317">Gets or sets hello maximum number of items toobe returned in hello enumeration operation in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-318">PartitionKeyRangeId</span><span class="sxs-lookup"><span data-stu-id="81a2d-318">PartitionKeyRangeId</span></span></td>
        <td><span data-ttu-id="81a2d-319">Získá nebo nastaví id klíče rozsah hello oddílu pro aktuální požadavek hello v hello Azure Cosmos DB databáze služby.</span><span class="sxs-lookup"><span data-stu-id="81a2d-319">Gets or sets hello partition key range id for hello current request in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-320">RequestContinuation</span><span class="sxs-lookup"><span data-stu-id="81a2d-320">RequestContinuation</span></span></td>
        <td><span data-ttu-id="81a2d-321">Získá nebo nastaví token pokračování požadavku hello v hello Azure Cosmos DB databáze služby.</span><span class="sxs-lookup"><span data-stu-id="81a2d-321">Gets or sets hello request continuation token in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="81a2d-322">SessionToken</span><span class="sxs-lookup"><span data-stu-id="81a2d-322">SessionToken</span></span></td>
        <td><span data-ttu-id="81a2d-323">Získá nebo nastaví token hello relace pro použití s konzistence typu relace v hello služba databáze Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81a2d-323">Gets or sets hello session token for use with session consistency in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="81a2d-324">StartFromBeginning</span><span class="sxs-lookup"><span data-stu-id="81a2d-324">StartFromBeginning</span></span></td>
        <td><span data-ttu-id="81a2d-325">Získá nebo nastaví, zda změna kanálu v hello Azure Cosmos DB databáze služby by se měl spustit z hello od (true) nebo aktuální (false).</span><span class="sxs-lookup"><span data-stu-id="81a2d-325">Gets or sets whether change feed in hello Azure Cosmos DB database service should start from hello beginning (true) or from current (false).</span></span> <span data-ttu-id="81a2d-326">Ve výchozím nastavení spustí z aktuální (false).</span><span class="sxs-lookup"><span data-stu-id="81a2d-326">By default, it starts from current (false).</span></span></td>
    </tr>
</table>

<span data-ttu-id="81a2d-327">**Změna možností informačního kanálu hostitele**:</span><span class="sxs-lookup"><span data-stu-id="81a2d-327">**Change Feed Host Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="81a2d-328">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="81a2d-328">Property Name</span></span></th>
        <th><span data-ttu-id="81a2d-329">Typ</span><span class="sxs-lookup"><span data-stu-id="81a2d-329">Type</span></span></th>
        <th><span data-ttu-id="81a2d-330">Popis</span><span class="sxs-lookup"><span data-stu-id="81a2d-330">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-331">LeaseRenewInterval</span><span class="sxs-lookup"><span data-stu-id="81a2d-331">LeaseRenewInterval</span></span></td>
        <td><span data-ttu-id="81a2d-332">Časový interval</span><span class="sxs-lookup"><span data-stu-id="81a2d-332">TimeSpan</span></span></td>
        <td><span data-ttu-id="81a2d-333">Hello interval pro všechny zapůjčení pro oddíly, které jsou aktuálně držené hello ChangeFeedEventHost instance.</span><span class="sxs-lookup"><span data-stu-id="81a2d-333">hello interval for all leases for partitions currently held by hello ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-334">LeaseAcquireInterval</span><span class="sxs-lookup"><span data-stu-id="81a2d-334">LeaseAcquireInterval</span></span></td>
        <td><span data-ttu-id="81a2d-335">Časový interval</span><span class="sxs-lookup"><span data-stu-id="81a2d-335">TimeSpan</span></span></td>
        <td><span data-ttu-id="81a2d-336">interval tookick vypnout toocompute úloh Hello, zda oddíly jsou rovnoměrně rozdělené mezi instancí známé hostitele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-336">hello interval tookick off a task toocompute whether partitions are distributed evenly among known host instances.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-337">LeaseExpirationInterval</span><span class="sxs-lookup"><span data-stu-id="81a2d-337">LeaseExpirationInterval</span></span></td>
        <td><span data-ttu-id="81a2d-338">Časový interval</span><span class="sxs-lookup"><span data-stu-id="81a2d-338">TimeSpan</span></span></td>
        <td><span data-ttu-id="81a2d-339">Hello interval, pro které hello zapůjčení pořízené zapůjčení představující oddílu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-339">hello interval for which hello lease is taken on a lease representing a partition.</span></span> <span data-ttu-id="81a2d-340">Pokud během tohoto intervalu neobnovíte hello zapůjčení, vypršela platnost a vlastnictví hello oddílu přesune tooanother ChangeFeedEventHost instance.</span><span class="sxs-lookup"><span data-stu-id="81a2d-340">If hello lease is not renewed within this interval, it is expired and ownership of hello partition moves tooanother ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-341">FeedPollDelay</span><span class="sxs-lookup"><span data-stu-id="81a2d-341">FeedPollDelay</span></span></td>
        <td><span data-ttu-id="81a2d-342">Časový interval</span><span class="sxs-lookup"><span data-stu-id="81a2d-342">TimeSpan</span></span></td>
        <td><span data-ttu-id="81a2d-343">Hello zpoždění mezi dotazování oddíl pro nové změny na hello informačního kanálu, až se nečekaně všechny aktuální změny.</span><span class="sxs-lookup"><span data-stu-id="81a2d-343">hello delay between polling a partition for new changes on hello feed, after all current changes are drained.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-344">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="81a2d-344">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="81a2d-345">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="81a2d-345">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="81a2d-346">Hello frekvence toocheckpoint zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-346">hello frequency toocheckpoint leases.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-347">MinPartitionCount</span><span class="sxs-lookup"><span data-stu-id="81a2d-347">MinPartitionCount</span></span></td>
        <td><span data-ttu-id="81a2d-348">celá čísla</span><span class="sxs-lookup"><span data-stu-id="81a2d-348">Int</span></span></td>
        <td><span data-ttu-id="81a2d-349">počet oddílů minimální Hello hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-349">hello minimum partition count for hello host.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-350">MaxPartitionCount</span><span class="sxs-lookup"><span data-stu-id="81a2d-350">MaxPartitionCount</span></span></td>
        <td><span data-ttu-id="81a2d-351">celá čísla</span><span class="sxs-lookup"><span data-stu-id="81a2d-351">Int</span></span></td>
        <td><span data-ttu-id="81a2d-352">může sloužit Hello maximální počet oddílů hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="81a2d-352">hello maximum number of partitions hello host can serve.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="81a2d-353">DiscardExistingLeases</span><span class="sxs-lookup"><span data-stu-id="81a2d-353">DiscardExistingLeases</span></span></td>
        <td><span data-ttu-id="81a2d-354">BOOL</span><span class="sxs-lookup"><span data-stu-id="81a2d-354">Bool</span></span></td>
        <td><span data-ttu-id="81a2d-355">Jestli na hello spusťte hello hostitele všechny existující zapůjčení, měl by být odstraněn a hello hostitele by měla začít úplně od začátku.</span><span class="sxs-lookup"><span data-stu-id="81a2d-355">Whether on hello start of hello host all existing leases should be deleted and hello host should start from scratch.</span></span></td>
    </tr>
</table>


<span data-ttu-id="81a2d-356">zpracování událostí toostart doložit `ChangeFeedProcessorHost`, poskytuje hello příslušné parametry pro Azure Cosmos DB kolekce.</span><span class="sxs-lookup"><span data-stu-id="81a2d-356">toostart event processing, instantiate `ChangeFeedProcessorHost`, providing hello appropriate parameters for your Azure Cosmos DB collection.</span></span> <span data-ttu-id="81a2d-357">Potom zavolejte `RegisterObserverAsync` tooregister vaše `IChangeFeedObserver` implementace (DocumentFeedObserver v tomto příkladu) s hello runtime.</span><span class="sxs-lookup"><span data-stu-id="81a2d-357">Then, call `RegisterObserverAsync` tooregister your `IChangeFeedObserver` (DocumentFeedObserver in this example) implementation with hello runtime.</span></span> <span data-ttu-id="81a2d-358">V tomto okamžiku hello hostitele pokusí tooacquire zapůjčení každých klíče rozsahu oddílu v kolekci Azure Cosmos DB hello použitím "chamtivého" algoritmu.</span><span class="sxs-lookup"><span data-stu-id="81a2d-358">At this point, hello host attempts tooacquire a lease on every partition key range in hello Azure Cosmos DB collection using a "greedy" algorithm.</span></span> <span data-ttu-id="81a2d-359">Tato zapůjčení poslední po stanovenou dobu a následně musí být obnovena.</span><span class="sxs-lookup"><span data-stu-id="81a2d-359">These leases last for a given timeframe and must then be renewed.</span></span> <span data-ttu-id="81a2d-360">Jako nové uzly a instancí pracovního procesu, v takovém případě režimu online, se umístí své rezervace zapůjčení a časem hello zatížení posune mezi uzly, protože každý hostitel pokusí tooacquire další zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="81a2d-360">As new nodes, worker instances, in this case, come online, they place lease reservations and over time hello load shifts between nodes as each host attempts tooacquire more leases.</span></span> 

<span data-ttu-id="81a2d-361">V ukázkovém kódu hello, budeme používat toocreate – třída (DocumentFeedObserverFactory.cs) objektu pro vytváření pozorovatel a hello `RegistObserverFactoryAsync` pozorovatel tooregister hello.</span><span class="sxs-lookup"><span data-stu-id="81a2d-361">In hello sample code, we use a factory class (DocumentFeedObserverFactory.cs) toocreate an observer and hello `RegistObserverFactoryAsync` tooregister hello observer.</span></span> 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter toostop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
<span data-ttu-id="81a2d-362">Postupem času se dosáhne rovnováhy.</span><span class="sxs-lookup"><span data-stu-id="81a2d-362">Over time, an equilibrium is established.</span></span> <span data-ttu-id="81a2d-363">Tato dynamická funkce umožňuje na základě procesoru automatické škálování toobe použít tooconsumers pro škálování nahoru a dolů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-363">This dynamic capability enables CPU-based auto-scaling toobe applied tooconsumers for both scale-up and scale-down.</span></span> <span data-ttu-id="81a2d-364">Pokud jsou k dispozici v Azure Cosmos DB s vyšší rychlostí než mohou příjemci zpracovat změny, může být hello zvýšení využití procesoru na spotřebitele toocause použité k automatickému škálování počtu instancí pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="81a2d-364">If changes are available in Azure Cosmos DB at a faster rate than consumers can process, hello CPU increase on consumers can be used toocause an auto-scale on worker instance count.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81a2d-365">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81a2d-365">Next steps</span></span>
* <span data-ttu-id="81a2d-366">Zkuste hello [Azure Cosmos DB změnu kanálu ukázky kódu na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span><span class="sxs-lookup"><span data-stu-id="81a2d-366">Try hello [Azure Cosmos DB Change feed code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span></span>
* <span data-ttu-id="81a2d-367">Začínáme s hello kódování [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="81a2d-367">Get started coding with hello [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
