---
title: "Metriky dotazů SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
description: "Další informace o tom, jak instrumentace a ladění výkon dotazů SQL Azure Cosmos DB požadavků."
keywords: "syntaxe SQL, dotaz sql, sql dotazy, json dotazovací jazyk, databázových koncepcí a sql, agregační funkce"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: b2fa8e8f-7291-45a3-9bd1-7284ed9077f8
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: d928113e809e5ad43901e79dc256a8a39c210181
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a><span data-ttu-id="ad37f-104">Ladění výkonu dotazů s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ad37f-104">Tuning query performance with Azure Cosmos DB</span></span>
<span data-ttu-id="ad37f-105">Poskytuje Azure Cosmos DB [SQL rozhraní API pro dotazování na data](documentdb-sql-query.md), aniž byste museli schématu nebo sekundární indexy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-105">Azure Cosmos DB provides a [SQL API for querying data](documentdb-sql-query.md), without requiring schema or secondary indexes.</span></span> <span data-ttu-id="ad37f-106">Tento článek obsahuje následující informace pro vývojáře:</span><span class="sxs-lookup"><span data-stu-id="ad37f-106">This article provides the following information for developers:</span></span>

* <span data-ttu-id="ad37f-107">Nejdůležitější podrobnosti o způsobu fungování spuštění dotazu SQL Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ad37f-107">High-level details on how Azure Cosmos DB's SQL query execution works</span></span>
* <span data-ttu-id="ad37f-108">Informace o hlavičkách žádostí a odpovědí dotazu a možnosti klienta SDK</span><span class="sxs-lookup"><span data-stu-id="ad37f-108">Details on query request and response headers, and client SDK options</span></span>
* <span data-ttu-id="ad37f-109">Tipy a osvědčené postupy pro výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="ad37f-109">Tips and best practices for query performance</span></span>
* <span data-ttu-id="ad37f-110">Příklady, jak využívat statistik provádění SQL k ladění výkonu dotazů</span><span class="sxs-lookup"><span data-stu-id="ad37f-110">Examples of how to utilize SQL execution statistics to debug query performance</span></span>

## <a name="about-sql-query-execution"></a><span data-ttu-id="ad37f-111">O spuštění dotazu SQL</span><span class="sxs-lookup"><span data-stu-id="ad37f-111">About SQL query execution</span></span>

<span data-ttu-id="ad37f-112">V Azure Cosmos DB, ukládat data do kontejnerů, které můžete dosáhnout žádné [velikost nebo žádostí o propustnost úložiště](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-112">In Azure Cosmos DB, you store data in containers, which can grow to any [storage size or request throughput](partition-data.md).</span></span> <span data-ttu-id="ad37f-113">Azure Cosmos DB bezproblémově škáluje data mezi fyzické oddíly skrytě zpracovat nárůst dat nebo zvyšte v zřízené propustnosti.</span><span class="sxs-lookup"><span data-stu-id="ad37f-113">Azure Cosmos DB seamlessly scales data across physical partitions under the covers to handle data growth or increase in provisioned throughput.</span></span> <span data-ttu-id="ad37f-114">Můžete použít dotazy SQL pro každý kontejner pomocí rozhraní REST API nebo jeden z podporovaném [DocumentDB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-114">You can issue SQL queries to any container using the REST API or one of the supported [DocumentDB SDKs](documentdb-sdk-dotnet.md).</span></span>

<span data-ttu-id="ad37f-115">Stručný přehled vytváření oddílů: definování klíč oddílu, jako je "city", která určuje, jak je rozdělit data do fyzického oddíly.</span><span class="sxs-lookup"><span data-stu-id="ad37f-115">A brief overview of partitioning: you define a partition key like "city", which determines how data is split across physical partitions.</span></span> <span data-ttu-id="ad37f-116">Data patřící do jednoho oddílu klíč (například "city" == "Seattle") je uložen v rámci fyzické oddílu, ale obvykle jednoho oddílu fyzického má více klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="ad37f-116">Data belonging to a single partition key (for example, "city" == "Seattle") is stored within a physical partition, but typically a single physical partition has multiple partition keys.</span></span> <span data-ttu-id="ad37f-117">V případě oddíl dosáhne velikosti úložiště, služba bezproblémově rozdělí oddílu na dva nové oddíly a se rovnoměrně rozděluje klíč oddílu mezi tyto oddíly.</span><span class="sxs-lookup"><span data-stu-id="ad37f-117">When a partition reaches its storage size, the service seamlessly splits the partition into two new partitions, and divides the partition key evenly across these partitions.</span></span> <span data-ttu-id="ad37f-118">Vzhledem k tomu, že oddíly jsou přechodné, pomocí rozhraní API abstrakci "oddílu klíče rozsahu", který označuje rozsahy hodnoty hash klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-118">Since partitions are transient, the APIs use an abstraction of a "partition key range", which denotes the ranges of partition key hashes.</span></span> 

<span data-ttu-id="ad37f-119">Pokud vydáte dotaz do databáze Cosmos Azure, SDK provádí tyto logických kroků:</span><span class="sxs-lookup"><span data-stu-id="ad37f-119">When you issue a query to Azure Cosmos DB, the SDK performs these logical steps:</span></span>

* <span data-ttu-id="ad37f-120">Analyzovat dotaz SQL k určení plán spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-120">Parse the SQL query to determine the query execution plan.</span></span> 
* <span data-ttu-id="ad37f-121">Pokud dotaz obsahuje filtr proti klíč oddílu, jako například `SELECT * FROM c WHERE c.city = "Seattle"`, se směruje na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="ad37f-121">If the query includes a filter against the partition key, like `SELECT * FROM c WHERE c.city = "Seattle"`, it is routed to a single partition.</span></span> <span data-ttu-id="ad37f-122">Pokud dotaz nemá filtr na klíč oddílu, pak se spustí v všechny oddíly a výsledky sloučení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ad37f-122">If the query does not have a filter on partition key, then it is executed in all partitions, and results are merged client side.</span></span>
* <span data-ttu-id="ad37f-123">Dotaz je provést v rámci každý oddíl v řadě nebo paralelní, v závislosti na konfiguraci klienta.</span><span class="sxs-lookup"><span data-stu-id="ad37f-123">The query is executed within each partition in series or parallel, based on client configuration.</span></span> <span data-ttu-id="ad37f-124">V rámci každého oddílu dotaz může si ho nebo další zpátečních cest v závislosti na složitosti dotazu nakonfigurovaná velikost stránky a zřízené propustnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="ad37f-124">Within each partition, the query might make one or more round trips depending on the query complexity, configured page size, and provisioned throughput of the collection.</span></span> <span data-ttu-id="ad37f-125">Každé spuštění vrátí počet [požadované jednotky](request-units.md) spotřebované spuštění dotazu a volitelně statistik provádění dotazů.</span><span class="sxs-lookup"><span data-stu-id="ad37f-125">Each execution returns the number of [request units](request-units.md) consumed by query execution, and optionally, query execution statistics.</span></span> 
* <span data-ttu-id="ad37f-126">Sada SDK provede shrnutí výsledků dotazu napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="ad37f-126">The SDK performs a summarization of the query results across partitions.</span></span> <span data-ttu-id="ad37f-127">Například pokud dotaz zahrnuje ORDER BY napříč oddíly, pak výsledky z jednotlivých oddílů jsou seřazeny sloučení vracet výsledky na globálně seřazené pořadí.</span><span class="sxs-lookup"><span data-stu-id="ad37f-127">For example, if the query involves an ORDER BY across partitions, then results from individual partitions are merge-sorted to return results in globally sorted order.</span></span> <span data-ttu-id="ad37f-128">Pokud je dotaz agregace jako `COUNT`, počty z jednotlivých oddílů jsou sečteny k vytvoření celkového počtu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-128">If the query is an aggregation like `COUNT`, the counts from individual partitions are summed to produce the overall count.</span></span>

<span data-ttu-id="ad37f-129">Sady SDK poskytují různé možnosti pro spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-129">The SDKs provide various options for query execution.</span></span> <span data-ttu-id="ad37f-130">Například v rozhraní .NET tyto možnosti jsou dostupné v `FeedOptions` třídy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-130">For example, in .NET these options are available in the `FeedOptions` class.</span></span> <span data-ttu-id="ad37f-131">Následující tabulka popisuje tyto možnosti a jak budou mít vliv doba provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-131">The following table describes these options and how they impact query execution time.</span></span> 

| <span data-ttu-id="ad37f-132">Možnost</span><span class="sxs-lookup"><span data-stu-id="ad37f-132">Option</span></span> | <span data-ttu-id="ad37f-133">Popis</span><span class="sxs-lookup"><span data-stu-id="ad37f-133">Description</span></span> |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | <span data-ttu-id="ad37f-134">Musí být nastavena na hodnotu true pro žádný dotaz, který vyžaduje, aby provést napříč více než jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="ad37f-134">Must be set to true for any query that requires to be executed across more than one partition.</span></span> <span data-ttu-id="ad37f-135">Jedná se explicitní příznak, který vám umožní provádět kompromisy vědomá toho výkonu během doby vývoje.</span><span class="sxs-lookup"><span data-stu-id="ad37f-135">This is an explicit flag to enable you to make conscious performance tradeoffs during development time.</span></span> |
| `EnableScanInQuery` | <span data-ttu-id="ad37f-136">Musí být nastavena na hodnotu true, pokud jste zvolili mimo indexování, ale chcete přesto spustit dotaz prostřednictvím kontrolu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-136">Must be set to true if you have opted out of indexing, but want to run the query via a scan anyway.</span></span> <span data-ttu-id="ad37f-137">Pouze použito pouze v případě indexování pro cestu požadovaný filtr je zakázána.</span><span class="sxs-lookup"><span data-stu-id="ad37f-137">Only applicable if indexing for the requested filter path is disabled.</span></span> | 
| `MaxItemCount` | <span data-ttu-id="ad37f-138">Maximální počet položek k vrácení za dobu odezvy na server.</span><span class="sxs-lookup"><span data-stu-id="ad37f-138">The maximum number of items to return per round trip to the server.</span></span> <span data-ttu-id="ad37f-139">Nastavení na hodnotu -1, můžete je nechat server spravovat počet položek.</span><span class="sxs-lookup"><span data-stu-id="ad37f-139">By setting to -1, you can let the server manage the number of items.</span></span> <span data-ttu-id="ad37f-140">Nebo můžete snížit tuto hodnotu načíst pouze malý počet položek na dobu odezvy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-140">Or, you can lower this value to retrieve only a small number of items per round trip.</span></span> 
| `MaxBufferedItemCount` | <span data-ttu-id="ad37f-141">Toto je možnost na straně klienta a používá k omezení využití paměti při provádění cross-partition ORDER BY.</span><span class="sxs-lookup"><span data-stu-id="ad37f-141">This is a client-side option, and used to limit the memory consumption when performing cross-partition ORDER BY.</span></span> <span data-ttu-id="ad37f-142">Vyšší hodnota pomáhá snížit latenci mezi oddílu řazení.</span><span class="sxs-lookup"><span data-stu-id="ad37f-142">A higher value helps reduce the latency of cross-partition sorting.</span></span> |
| `MaxDegreeOfParallelism` | <span data-ttu-id="ad37f-143">Získá nebo nastaví počet souběžných operací spustit na straně klienta během provádění paralelního dotazu v databázi služby Azure DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="ad37f-143">Gets or sets the number of concurrent operations run client side during parallel query execution in the Azure DocumentDB database service.</span></span> <span data-ttu-id="ad37f-144">Hodnotu vlastnosti kladné omezuje počet souběžných operací nastavte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-144">A positive property value limits the number of concurrent operations to the set value.</span></span> <span data-ttu-id="ad37f-145">Pokud je nastavena na hodnotu menší než 0, systém automaticky rozhoduje, počet souběžných operací ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="ad37f-145">If it is set to less than 0, the system automatically decides the number of concurrent operations to run.</span></span> |
| `PopulateQueryMetrics` | <span data-ttu-id="ad37f-146">Doba načítání umožní podrobné protokolování statistiky času stráveného v různých fázích provádění dotazu jako čas kompilace, index smyčky čas a dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-146">Enables detailed logging of statistics of time spent in various phases of query execution like compilation time, index loop time, and document load time.</span></span> <span data-ttu-id="ad37f-147">Výstup z Statistika dotazu můžete sdílet s podporu Azure o diagnostice problémů s výkonem dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-147">You can share output from query statistics with Azure Support to diagnose query performance issues.</span></span> |
| `RequestContinuation` | <span data-ttu-id="ad37f-148">Provádění dotazů můžete obnovit pomocí předávání neprůhledné pokračovací token vrácený jakýkoli dotaz.</span><span class="sxs-lookup"><span data-stu-id="ad37f-148">You can resume query execution by passing in the opaque continuation token returned by any query.</span></span> <span data-ttu-id="ad37f-149">Token pro pokračování zapouzdří všechny stavy, které jsou potřebné pro spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-149">The continuation token encapsulates all state required for query execution.</span></span> |
| `ResponseContinuationTokenLimitInKb` | <span data-ttu-id="ad37f-150">Můžete omezit maximální velikost token pro pokračování vrácená serverem.</span><span class="sxs-lookup"><span data-stu-id="ad37f-150">You can limit the maximum size of the continuation token returned by the server.</span></span> <span data-ttu-id="ad37f-151">Možná budete muset nastavit Pokud hostitele vaší aplikace má omezení velikosti hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ad37f-151">You might need to set this if your application host has limits on response header size.</span></span> <span data-ttu-id="ad37f-152">Toto nastavení může zvýšit celkový doba trvání a RUs využité pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="ad37f-152">Setting this may increase the overall duration and RUs consumed for the query.</span></span>  |

<span data-ttu-id="ad37f-153">Například příklad dotazu podívejme na požadovanou na kolekci s klíčem oddílu `/city` jako oddíl klíče a s propustností 100 000 RU/s přiděleným.</span><span class="sxs-lookup"><span data-stu-id="ad37f-153">For example, let's take an example query on partition key requested on a collection with `/city` as the partition key and provisioned with 100,000 RU/s of throughput.</span></span> <span data-ttu-id="ad37f-154">Požádáte o dotaz pomocí `CreateDocumentQuery<T>` v rozhraní .NET takto:</span><span class="sxs-lookup"><span data-stu-id="ad37f-154">You request this query using `CreateDocumentQuery<T>` in .NET like the following:</span></span>

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

<span data-ttu-id="ad37f-155">Fragmentu SDK uvedené výše, odpovídá následující požadavku REST API:</span><span class="sxs-lookup"><span data-stu-id="ad37f-155">The SDK snippet shown above, corresponds to the following REST API request:</span></span>

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

<span data-ttu-id="ad37f-156">Každé stránce provádění dotazu odpovídá rozhraní REST API `POST` s `Accept: application/query+json` záhlaví a dotazu SQL v těle.</span><span class="sxs-lookup"><span data-stu-id="ad37f-156">Each query execution page corresponds to a REST API `POST` with the `Accept: application/query+json` header, and the SQL query in the body.</span></span> <span data-ttu-id="ad37f-157">Každý dotaz je jeden nebo více cest k serveru se zaokrouhlí `x-ms-continuation` token opakována mezi klientem a serverem, chcete-li pokračovat v provádění.</span><span class="sxs-lookup"><span data-stu-id="ad37f-157">Each query makes one or more round trips to the server with the `x-ms-continuation` token echoed between the client and server to resume execution.</span></span> <span data-ttu-id="ad37f-158">Možnosti konfigurace v FeedOptions jsou předány na server ve formě hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="ad37f-158">The configuration options in FeedOptions are passed to the server in the form of request headers.</span></span> <span data-ttu-id="ad37f-159">Například `MaxItemCount` odpovídá `x-ms-max-item-count`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-159">For example, `MaxItemCount` corresponds to `x-ms-max-item-count`.</span></span> 

<span data-ttu-id="ad37f-160">Požadavek vrátí odpověď na následující (zkrácená čitelnější):</span><span class="sxs-lookup"><span data-stu-id="ad37f-160">The request returns the following (truncated for readability) response:</span></span>

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

<span data-ttu-id="ad37f-161">Hlavičky odpovědi klíče vrácená z dotazu zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="ad37f-161">The key response headers returned from the query include the following:</span></span>

| <span data-ttu-id="ad37f-162">Možnost</span><span class="sxs-lookup"><span data-stu-id="ad37f-162">Option</span></span> | <span data-ttu-id="ad37f-163">Popis</span><span class="sxs-lookup"><span data-stu-id="ad37f-163">Description</span></span> |
| ------ | ----------- |
| `x-ms-item-count` | <span data-ttu-id="ad37f-164">Počet položek, které vrácený v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ad37f-164">The number of items returned in the response.</span></span> <span data-ttu-id="ad37f-165">Toto je závislá na zadaných `x-ms-max-item-count`, počet položek, které můžete přizpůsobit velikost datové části maximální odpovědi, zřízené propustnosti a doba provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-165">This is dependent on the supplied `x-ms-max-item-count`, the number of items that can be fit within the maximum response payload size, the provisioned throughput, and query execution time.</span></span> |  
| `x-ms-continuation:` | <span data-ttu-id="ad37f-166">Pokračovací token, který má-li pokračovat v provádění dotazu, pokud jsou k dispozici další výsledky.</span><span class="sxs-lookup"><span data-stu-id="ad37f-166">The continuation token to resume execution of the query, if additional results are available.</span></span> | 
| `x-ms-documentdb-query-metrics` | <span data-ttu-id="ad37f-167">Statistika dotazu pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="ad37f-167">The query statistics for the execution.</span></span> <span data-ttu-id="ad37f-168">Toto je oddělený řetězec obsahující statistiky času stráveného v různých fázích spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-168">This is a delimited string containing statistics of time spent in the various phases of query execution.</span></span> <span data-ttu-id="ad37f-169">Vrácené v případě `x-ms-documentdb-populatequerymetrics` je nastaven na `True`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-169">Returned if `x-ms-documentdb-populatequerymetrics` is set to `True`.</span></span> | 
| `x-ms-request-charge` | <span data-ttu-id="ad37f-170">Počet [požadované jednotky](request-units.md) spotřebovávají dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-170">The number of [request units](request-units.md) consumed by the query.</span></span> | 

<span data-ttu-id="ad37f-171">Podrobnosti o hlavičky požadavku REST API a možnostech najdete v tématu [dotaz na prostředky pomocí DocumentDB REST API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).</span><span class="sxs-lookup"><span data-stu-id="ad37f-171">For details on the REST API request headers and options, see [Querying resources using the DocumentDB REST API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).</span></span>

## <a name="best-practices-for-query-performance"></a><span data-ttu-id="ad37f-172">Osvědčené postupy pro výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="ad37f-172">Best practices for query performance</span></span>
<span data-ttu-id="ad37f-173">Níže jsou většiny běžných faktorů, které mít vliv na výkon dotazů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ad37f-173">The following are the most common factors that impact Azure Cosmos DB query performance.</span></span> <span data-ttu-id="ad37f-174">Jsme podrobněji prozkoumat každý z těchto témat v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ad37f-174">We dig deeper into each of these topics in this article.</span></span>

| <span data-ttu-id="ad37f-175">Koeficient</span><span class="sxs-lookup"><span data-stu-id="ad37f-175">Factor</span></span> | <span data-ttu-id="ad37f-176">Tip</span><span class="sxs-lookup"><span data-stu-id="ad37f-176">Tip</span></span> | 
| ------ | -----| 
| <span data-ttu-id="ad37f-177">Zřízená propustnost</span><span class="sxs-lookup"><span data-stu-id="ad37f-177">Provisioned throughput</span></span> | <span data-ttu-id="ad37f-178">Měření RU na jeden dotaz a ujistěte se, že máte požadované zřízené propustnosti pro své dotazy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-178">Measure RU per query, and ensure that you have the required provisioned throughput for your queries.</span></span> | 
| <span data-ttu-id="ad37f-179">Dělení a klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="ad37f-179">Partitioning and partition keys</span></span> | <span data-ttu-id="ad37f-180">Upřednostnit dotazů s hodnotou klíče oddílu v klauzuli filtru pro s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="ad37f-180">Favor queries with the partition key value in the filter clause for low latency.</span></span> |
| <span data-ttu-id="ad37f-181">Možnosti sady SDK a dotazů</span><span class="sxs-lookup"><span data-stu-id="ad37f-181">SDK and query options</span></span> | <span data-ttu-id="ad37f-182">Doporučené postupy SDK jako přímé připojení a ladit možnosti provedení dotazu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ad37f-182">Follow SDK best practices like direct connectivity, and tune client-side query execution options.</span></span> |
| <span data-ttu-id="ad37f-183">Latence sítě</span><span class="sxs-lookup"><span data-stu-id="ad37f-183">Network latency</span></span> | <span data-ttu-id="ad37f-184">Účet pro síť režie v měření a použít více funkci rozhraní API ke čtení z nejbližší oblast.</span><span class="sxs-lookup"><span data-stu-id="ad37f-184">Account for network overhead in measurement, and use multi-homing APIs to read from the nearest region.</span></span> |
| <span data-ttu-id="ad37f-185">Zásady indexování</span><span class="sxs-lookup"><span data-stu-id="ad37f-185">Indexing Policy</span></span> | <span data-ttu-id="ad37f-186">Ujistěte se, že máte požadované indexování cesty nebo zásady pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="ad37f-186">Ensure that you have the required indexing paths/policy for the query.</span></span> |
| <span data-ttu-id="ad37f-187">Metriky spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="ad37f-187">Query execution metrics</span></span> | <span data-ttu-id="ad37f-188">Analyzujte metriky provádění dotazu identifikovat potenciální přepisů dotaz a datové obrazce.</span><span class="sxs-lookup"><span data-stu-id="ad37f-188">Analyze the query execution metrics to identify potential rewrites of query and data shapes.</span></span>  |

### <a name="provisioned-throughput"></a><span data-ttu-id="ad37f-189">Zřízená propustnost</span><span class="sxs-lookup"><span data-stu-id="ad37f-189">Provisioned throughput</span></span>
<span data-ttu-id="ad37f-190">V systému Cosmos databáze můžete vytvořit kontejnery dat, každý s vyhrazenou propustností vyjádřené v žádosti o jednotkách (RU) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-190">In Cosmos DB, you create containers of data, each with reserved throughput expressed in request units (RU) per-second.</span></span> <span data-ttu-id="ad37f-191">Čtení 1 KB dokumentu je 1 RU a každé operace (včetně dotazů) je normalizovány na pevný počet RUs podle jeho složitost.</span><span class="sxs-lookup"><span data-stu-id="ad37f-191">A read of a 1-KB document is 1 RU, and every operation (including queries) is normalized to a fixed number of RUs based on its complexity.</span></span> <span data-ttu-id="ad37f-192">Například pokud máte 1000 zřízení RU/s pro váš kontejner, a máte dotaz jako `SELECT * FROM c WHERE c.city = 'Seattle'` , který využívá 5 RUs a potom můžete provést (1000 RU/s) / (5 RU/dotazu) = 200 dotazu nebo s takové dotazy za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-192">For example, if you have 1000 RU/s provisioned for your container, and you have a query like `SELECT * FROM c WHERE c.city = 'Seattle'` that consumes 5 RUs, then you can perform (1000 RU/s) / (5 RU/query) = 200 query/s such queries per second.</span></span> 

<span data-ttu-id="ad37f-193">Pokud odešlete více než 200 dotazů za sekundu, spustí službu omezení rychlosti příchozí požadavky nad 200/s.</span><span class="sxs-lookup"><span data-stu-id="ad37f-193">If you submit more than 200 queries/sec, the service starts rate-limiting incoming requests above 200/s.</span></span> <span data-ttu-id="ad37f-194">Sady SDK automaticky zpracovávat tento případ provedením omezení rychlosti nebo opakování, proto možná jste si všimli vyšší latence pro tyto dotazy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-194">The SDKs automatically handle this case by performing a backoff/retry, therefore you might notice a higher latency for these queries.</span></span> <span data-ttu-id="ad37f-195">Zvýšení zřízené propustnosti na požadovaná hodnota zlepšuje dotazu latence a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="ad37f-195">Increasing the provisioned throughput to the required value improves your query latency and throughput.</span></span> 

<span data-ttu-id="ad37f-196">Další informace o jednotkách žádosti najdete v tématu [požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-196">To learn more about request units, see [Request units](request-units.md).</span></span>

### <a name="partitioning-and-partition-keys"></a><span data-ttu-id="ad37f-197">Dělení a klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="ad37f-197">Partitioning and partition keys</span></span>
<span data-ttu-id="ad37f-198">S Azure DB Cosmos obvykle dotazy provádět v uvedeném pořadí z nejrychlejší nebo většinu efektivní pomalejší nebo méně efektivní.</span><span class="sxs-lookup"><span data-stu-id="ad37f-198">With Azure Cosmos DB, typically queries perform in the following order from fastest/most efficient to slower/less efficient.</span></span> 

* <span data-ttu-id="ad37f-199">ZÍSKAT klíč jednoho oddílu a klíč položky</span><span class="sxs-lookup"><span data-stu-id="ad37f-199">GET on a single partition key and item key</span></span>
* <span data-ttu-id="ad37f-200">Dotaz s klauzulí filtru pro jeden oddíl klíč</span><span class="sxs-lookup"><span data-stu-id="ad37f-200">Query with a filter clause on a single partition key</span></span>
* <span data-ttu-id="ad37f-201">Dotazování bez klauzule filtru rovnosti nebo rozsah na žádnou vlastnost</span><span class="sxs-lookup"><span data-stu-id="ad37f-201">Query without an equality or range filter clause on any property</span></span>
* <span data-ttu-id="ad37f-202">Dotazování bez filtry</span><span class="sxs-lookup"><span data-stu-id="ad37f-202">Query without filters</span></span>

<span data-ttu-id="ad37f-203">Dotazy, které potřebují najdete všechny oddíly potřebovat zhorší latenci a vyšší RUs spotřebovat.</span><span class="sxs-lookup"><span data-stu-id="ad37f-203">Queries that need to consult all partitions need higher latency, and can consume higher RUs.</span></span> <span data-ttu-id="ad37f-204">Vzhledem k tomu, že každý oddíl má automatické indexování pro všechny vlastnosti, dotaz nelze zpracovat efektivně od indexu v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="ad37f-204">Since each partition has automatic indexing against all properties, the query can be served efficiently from the index in this case.</span></span> <span data-ttu-id="ad37f-205">Můžete provést dotazy, které jsou rozmístěny oddíly rychlejší s použitím možností paralelismus.</span><span class="sxs-lookup"><span data-stu-id="ad37f-205">You can make queries that span partitions faster by using the parallelism options.</span></span>

<span data-ttu-id="ad37f-206">Další informace o vytváření oddílů a klíče oddílů, najdete v části [vytváření oddílů v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-206">To learn more about partitioning and partition keys, see [Partitioning in Azure Cosmos DB](partition-data.md).</span></span>

### <a name="sdk-and-query-options"></a><span data-ttu-id="ad37f-207">Možnosti sady SDK a dotazů</span><span class="sxs-lookup"><span data-stu-id="ad37f-207">SDK and query options</span></span>
<span data-ttu-id="ad37f-208">V tématu [tipy pro zvýšení výkonu](performance-tips.md) a [testování výkonu](performance-testing.md) pro získání nejlepší výkon klienta z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ad37f-208">See [Performance Tips](performance-tips.md) and [Performance testing](performance-testing.md) for how to get the best client-side performance from Azure Cosmos DB.</span></span> <span data-ttu-id="ad37f-209">To zahrnuje pomocí nejnovější SDK, konfigurace specifických pro platformy konfigurace jako výchozí počet připojení, frekvenci uvolňování paměti a pomocí možnosti lightweight připojení jako přímé/TCP.</span><span class="sxs-lookup"><span data-stu-id="ad37f-209">This includes using the latest SDKs, configuring platform-specific configurations like default number of connections, frequency of garbage collection, and using lightweight connectivity options like Direct/TCP.</span></span> 


#### <a name="max-item-count"></a><span data-ttu-id="ad37f-210">Počet položek. maximální počet</span><span class="sxs-lookup"><span data-stu-id="ad37f-210">Max Item Count</span></span>
<span data-ttu-id="ad37f-211">Pro dotazy, hodnota `MaxItemCount` může mít významný dopad na dobu začátku do konce dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-211">For queries, the value of `MaxItemCount` can have a significant impact on end-to-end query time.</span></span> <span data-ttu-id="ad37f-212">Vrátí maximálně počet položek v každé výměně zpráv serveru `MaxItemCount` (výchozí 100 položek).</span><span class="sxs-lookup"><span data-stu-id="ad37f-212">Each round trip to the server will return no more than the number of items in `MaxItemCount` (Default of 100 items).</span></span> <span data-ttu-id="ad37f-213">Toto nastavení na vyšší hodnotu (-1 je maximální a doporučené), který umožní zvýšení celkové doby trvání dotaz tak, že omezí počet zpátečních cest mezi serverem a klientem, zejména pro dotazy s velké množství výsledků.</span><span class="sxs-lookup"><span data-stu-id="ad37f-213">Setting this to a higher value (-1 is maximum, and recommended) will improve your query duration overall by limiting the number of round trips between server and client, especially for queries with large result sets.</span></span>

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a><span data-ttu-id="ad37f-214">Maximální počet stupně paralelního zpracování</span><span class="sxs-lookup"><span data-stu-id="ad37f-214">Max Degree of Parallelism</span></span>
<span data-ttu-id="ad37f-215">Pro dotazy, ladit `MaxDegreeOfParallelism` k identifikaci doporučené konfigurace pro aplikace, zejména v případě, že můžete provádět dotazy cross-partition (bez filtru na základě hodnoty klíč oddílu).</span><span class="sxs-lookup"><span data-stu-id="ad37f-215">For queries, tune the `MaxDegreeOfParallelism` to identify the best configurations for your application, especially if you perform cross-partition queries (without a filter on the partition-key value).</span></span> <span data-ttu-id="ad37f-216">`MaxDegreeOfParallelism`Určuje maximální počet paralelních úkolů, například maximální počet oddílů návštěvy paralelně.</span><span class="sxs-lookup"><span data-stu-id="ad37f-216">`MaxDegreeOfParallelism`  controls the maximum number of parallel tasks, i.e., the maximum of partitions to be visited in parallel.</span></span> 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

<span data-ttu-id="ad37f-217">Předpokládejme, že</span><span class="sxs-lookup"><span data-stu-id="ad37f-217">Let’s assume that</span></span>
* <span data-ttu-id="ad37f-218">D = výchozí maximální počet paralelních úloh (= celkový počet procesorů v klientském počítači.)</span><span class="sxs-lookup"><span data-stu-id="ad37f-218">D = Default Maximum number of parallel tasks (= total number of processor in the client machine)</span></span>
* <span data-ttu-id="ad37f-219">P = zadán uživatel maximální počet paralelních úloh</span><span class="sxs-lookup"><span data-stu-id="ad37f-219">P = User-specified maximum number of parallel tasks</span></span>
* <span data-ttu-id="ad37f-220">N = počet oddílů, které potřebuje návštěvy odpovědí na dotazy</span><span class="sxs-lookup"><span data-stu-id="ad37f-220">N = Number of partitions that needs  to be visited for answering a query</span></span>

<span data-ttu-id="ad37f-221">Toto jsou důsledky chování paralelní dotazy pro různé hodnoty P.</span><span class="sxs-lookup"><span data-stu-id="ad37f-221">Following are implications of how the parallel queries would behave for different values of P.</span></span>
* <span data-ttu-id="ad37f-222">(P == 0) = > sériové režimu</span><span class="sxs-lookup"><span data-stu-id="ad37f-222">(P == 0) => Serial Mode</span></span>
* <span data-ttu-id="ad37f-223">(P == 1) = > maximálně jeden úkol</span><span class="sxs-lookup"><span data-stu-id="ad37f-223">(P == 1) => Maximum of one task</span></span>
* <span data-ttu-id="ad37f-224">(P > 1) = > Min (P, N) paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="ad37f-224">(P > 1) => Min (P, N) parallel tasks</span></span> 
* <span data-ttu-id="ad37f-225">(P < 1) = > Min (N, D) paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="ad37f-225">(P < 1) => Min (N, D) parallel tasks</span></span>

<span data-ttu-id="ad37f-226">Poznámky k verzi sady SDK a najdete v části Podrobnosti o implementované třídy a metody [DocumentDB SDK](documentdb-sdk-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="ad37f-226">For SDK release notes, and details on implemented classes and methods see [DocumentDB SDKs](documentdb-sdk-dotnet.md)</span></span>

### <a name="network-latency"></a><span data-ttu-id="ad37f-227">Latence sítě</span><span class="sxs-lookup"><span data-stu-id="ad37f-227">Network latency</span></span>
<span data-ttu-id="ad37f-228">V tématu [globální distribuční databázi Cosmos Azure](tutorial-global-distribution-documentdb.md) jak nastavit globální distribuční a připojte se k nejbližší oblast.</span><span class="sxs-lookup"><span data-stu-id="ad37f-228">See [Azure Cosmos DB global distribution](tutorial-global-distribution-documentdb.md) for how to set up global distribution, and connect to the closest region.</span></span> <span data-ttu-id="ad37f-229">Latence sítě nemá významný dopad na výkon dotazů, když potřebujete udělat vícenásobný nebo načíst velké výslednou sadu z dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-229">Network latency has a significant impact on query performance when you need to make multiple round-trips or retrieve a large result set from the query.</span></span> 

<span data-ttu-id="ad37f-230">V části o metrikách provádění dotazu vysvětluje, jak načíst server čas provádění dotazů ( `totalExecutionTimeInMs`), takže můžete rozlišit mezi času stráveného při provádění dotazu a čas strávený v síti přenosu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-230">The section on query execution metrics explains how to retrieve the server execution time of queries ( `totalExecutionTimeInMs`), so that you can differentiate between time spent in query execution and time spent in network transit.</span></span>

### <a name="indexing-policy"></a><span data-ttu-id="ad37f-231">Zásady indexování</span><span class="sxs-lookup"><span data-stu-id="ad37f-231">Indexing policy</span></span>
<span data-ttu-id="ad37f-232">V tématu [Konfigurace zásady indexování](indexing-policies.md) pro indexování cesty, typy a režimy, a jak by se projevily při provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-232">See [Configuring indexing policy](indexing-policies.md) for indexing paths, kinds, and modes, and how they impact query execution.</span></span> <span data-ttu-id="ad37f-233">Ve výchozím nastavení, zásady indexování používá Hash indexování pro řetězce, který je efektivní dotazy na rovnost, ale ne pro rozsah dotazy nebo order by – dotazy.</span><span class="sxs-lookup"><span data-stu-id="ad37f-233">By default, the indexing policy uses Hash indexing for strings, which is effective for equality queries, but not for range queries/order by queries.</span></span> <span data-ttu-id="ad37f-234">Pokud potřebujete dotazy na rozsah pro řetězce, doporučujeme určení index typu rozsah pro všechny řetězce.</span><span class="sxs-lookup"><span data-stu-id="ad37f-234">If you need range queries for strings, we recommend specifying the Range index type for all strings.</span></span> 

## <a name="query-execution-metrics"></a><span data-ttu-id="ad37f-235">Metriky spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="ad37f-235">Query execution metrics</span></span>
<span data-ttu-id="ad37f-236">Můžete získat podrobné metriky pro spuštění dotazu předáním v nepovinný `x-ms-documentdb-populatequerymetrics` záhlaví (`FeedOptions.PopulateQueryMetrics` sady .NET SDK).</span><span class="sxs-lookup"><span data-stu-id="ad37f-236">You can obtain detailed metrics on query execution by passing in the optional `x-ms-documentdb-populatequerymetrics` header (`FeedOptions.PopulateQueryMetrics` in the .NET SDK).</span></span> <span data-ttu-id="ad37f-237">Hodnota vrácená v `x-ms-documentdb-query-metrics` má následující páry klíč hodnota určená pro pokročilé řešení problémů s spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-237">The value returned in `x-ms-documentdb-query-metrics` has the following key-value pairs meant for advanced troubleshooting of query execution.</span></span> 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| <span data-ttu-id="ad37f-238">Metrika</span><span class="sxs-lookup"><span data-stu-id="ad37f-238">Metric</span></span> | <span data-ttu-id="ad37f-239">Jednotka</span><span class="sxs-lookup"><span data-stu-id="ad37f-239">Unit</span></span> | <span data-ttu-id="ad37f-240">Popis</span><span class="sxs-lookup"><span data-stu-id="ad37f-240">Description</span></span> | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | <span data-ttu-id="ad37f-241">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-241">milliseconds</span></span> | <span data-ttu-id="ad37f-242">Doba provádění dotazu</span><span class="sxs-lookup"><span data-stu-id="ad37f-242">Query execution time</span></span> | 
| `queryCompileTimeInMs` | <span data-ttu-id="ad37f-243">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-243">milliseconds</span></span> | <span data-ttu-id="ad37f-244">Doba kompilace dotazu</span><span class="sxs-lookup"><span data-stu-id="ad37f-244">Query compile time</span></span>  | 
| `queryLogicalPlanBuildTimeInMs` | <span data-ttu-id="ad37f-245">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-245">milliseconds</span></span> | <span data-ttu-id="ad37f-246">Čas vytvořit plán dotazu logické</span><span class="sxs-lookup"><span data-stu-id="ad37f-246">Time to build logical query plan</span></span> | 
| `queryPhysicalPlanBuildTimeInMs` | <span data-ttu-id="ad37f-247">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-247">milliseconds</span></span> | <span data-ttu-id="ad37f-248">Čas vytvořit plán dotazu fyzické</span><span class="sxs-lookup"><span data-stu-id="ad37f-248">Time to build physical query plan</span></span> | 
| `queryOptimizationTimeInMs` | <span data-ttu-id="ad37f-249">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-249">milliseconds</span></span> | <span data-ttu-id="ad37f-250">Čas strávený v optimalizaci dotazu</span><span class="sxs-lookup"><span data-stu-id="ad37f-250">Time spent in optimizing query</span></span> | 
| `VMExecutionTimeInMs` | <span data-ttu-id="ad37f-251">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-251">milliseconds</span></span> | <span data-ttu-id="ad37f-252">Čas strávený v dotazu runtime</span><span class="sxs-lookup"><span data-stu-id="ad37f-252">Time spent in query runtime</span></span> | 
| `indexLookupTimeInMs` | <span data-ttu-id="ad37f-253">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-253">milliseconds</span></span> | <span data-ttu-id="ad37f-254">Čas strávený v indexu fyzické vrstvě</span><span class="sxs-lookup"><span data-stu-id="ad37f-254">Time spent in physical index layer</span></span> | 
| `documentLoadTimeInMs` | <span data-ttu-id="ad37f-255">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-255">milliseconds</span></span> | <span data-ttu-id="ad37f-256">Čas strávený v nahrávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="ad37f-256">Time spent in loading documents</span></span>  | 
| `systemFunctionExecuteTimeInMs` | <span data-ttu-id="ad37f-257">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-257">milliseconds</span></span> | <span data-ttu-id="ad37f-258">Celkový čas strávený provádění (Předdefinované) funkce systému v milisekundách</span><span class="sxs-lookup"><span data-stu-id="ad37f-258">Total time spent executing system (built-in) functions in milliseconds</span></span>  | 
| `userFunctionExecuteTimeInMs` | <span data-ttu-id="ad37f-259">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-259">milliseconds</span></span> | <span data-ttu-id="ad37f-260">Celkový čas strávený spouštění uživatelsky definované funkce v milisekundách</span><span class="sxs-lookup"><span data-stu-id="ad37f-260">Total time spent executing user-defined functions in milliseconds</span></span> | 
| `retrievedDocumentCount` | <span data-ttu-id="ad37f-261">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-261">milliseconds</span></span> | <span data-ttu-id="ad37f-262">Celkový počet načtených dokumentů</span><span class="sxs-lookup"><span data-stu-id="ad37f-262">Total number of retrieved documents</span></span>  | 
| `retrievedDocumentSize` | <span data-ttu-id="ad37f-263">Bajty</span><span class="sxs-lookup"><span data-stu-id="ad37f-263">bytes</span></span> | <span data-ttu-id="ad37f-264">Celková velikost načtené dokumenty v bajtech</span><span class="sxs-lookup"><span data-stu-id="ad37f-264">Total size of retrieved documents in bytes</span></span>  | 
| `outputDocumentCount` | <span data-ttu-id="ad37f-265">Počet</span><span class="sxs-lookup"><span data-stu-id="ad37f-265">count</span></span> | <span data-ttu-id="ad37f-266">Počet výstupních dokumentů</span><span class="sxs-lookup"><span data-stu-id="ad37f-266">Number of output documents</span></span> | 
| `writeOutputTimeInMs` | <span data-ttu-id="ad37f-267">milisekundy</span><span class="sxs-lookup"><span data-stu-id="ad37f-267">milliseconds</span></span> | <span data-ttu-id="ad37f-268">Doba provádění dotazu v milisekundách</span><span class="sxs-lookup"><span data-stu-id="ad37f-268">Query execution time in milliseconds</span></span> | 
| `indexUtilizationRatio` | <span data-ttu-id="ad37f-269">poměr (< = 1)</span><span class="sxs-lookup"><span data-stu-id="ad37f-269">ratio (<=1)</span></span> | <span data-ttu-id="ad37f-270">Načíst poměr počtu dokumenty odpovídala filtr pro počet dokumentů</span><span class="sxs-lookup"><span data-stu-id="ad37f-270">Ratio of number of documents matched by the filter to the number of documents loaded</span></span>  | 

<span data-ttu-id="ad37f-271">Klientské sady SDK může být interně více operací dotazu k obsluze dotaz v rámci každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="ad37f-271">The client SDKs may internally make multiple query operations to serve the query within each partition.</span></span> <span data-ttu-id="ad37f-272">Klient podá více než jedno volání oddílů pokud celkový počet výsledků překročí `x-ms-max-item-count`, pokud dotaz překračuje zřízené propustnosti pro oddíl, datové části dotazu dosáhne maximální velikosti na stránce, nebo pokud dotaz dosáhne systému přidělené časový limit.</span><span class="sxs-lookup"><span data-stu-id="ad37f-272">The client makes more than one call per-partition if the total results exceed `x-ms-max-item-count`, if the query exceeds the provisioned throughput for the partition, or if the query payload reaches the maximum size per page, or if the query reaches the system allocated timeout limit.</span></span> <span data-ttu-id="ad37f-273">Každé spuštění částečné dotaz vrátí `x-ms-documentdb-query-metrics` pro tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="ad37f-273">Each partial query execution returns a `x-ms-documentdb-query-metrics` for that page.</span></span> 

<span data-ttu-id="ad37f-274">Tady jsou některé ukázkové dotazy a jak interpretovat některé z metriky vrácená při provádění dotazu:</span><span class="sxs-lookup"><span data-stu-id="ad37f-274">Here are some sample queries, and how to interpret some of the metrics returned from query execution:</span></span> 

| <span data-ttu-id="ad37f-275">Dotaz</span><span class="sxs-lookup"><span data-stu-id="ad37f-275">Query</span></span> | <span data-ttu-id="ad37f-276">Ukázka metrika</span><span class="sxs-lookup"><span data-stu-id="ad37f-276">Sample Metric</span></span> | <span data-ttu-id="ad37f-277">Popis</span><span class="sxs-lookup"><span data-stu-id="ad37f-277">Description</span></span> | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | <span data-ttu-id="ad37f-278">Počet dokumentů, načíst je 100 + 1 tak, aby odpovídaly klauzule TOP.</span><span class="sxs-lookup"><span data-stu-id="ad37f-278">The number of documents retrieved is 100+1 to match the TOP clause.</span></span> <span data-ttu-id="ad37f-279">Doba dotazu je většinou věnovaný `WriteOutputTime` a `DocumentLoadTime` vzhledem k tomu, že je kontrolu.</span><span class="sxs-lookup"><span data-stu-id="ad37f-279">Query time is mostly spent in `WriteOutputTime` and `DocumentLoadTime` since it is a scan.</span></span> | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | <span data-ttu-id="ad37f-280">RetrievedDocumentCount je nyní vyšší (500 + 1 tak, aby odpovídaly klauzule TOP).</span><span class="sxs-lookup"><span data-stu-id="ad37f-280">RetrievedDocumentCount is now higher (500+1 to match the TOP clause).</span></span> | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | <span data-ttu-id="ad37f-281">O 0.9 ms je věnovaný IndexLookupTime pro vyhledávání klíčů, protože je indexu vyhledávání `/N/?`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-281">About 0.9 ms is spent in IndexLookupTime for a key lookup, because it's an index lookup on `/N/?`.</span></span> | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | <span data-ttu-id="ad37f-282">Něco delší dobu (1.7 ms) věnovaný IndexLookupTime přes rozsah kontroly, protože je indexu vyhledávání `/N/?`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-282">Slightly more time (1.7 ms) spent in IndexLookupTime over a range scan, because it's an index lookup on `/N/?`.</span></span> | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | <span data-ttu-id="ad37f-283">Stejný čas strávený `DocumentLoadTime` jako předchozí dotazy, ale nižší `WriteOutputTime` protože jsme se projekce pouze jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ad37f-283">Same time spent on `DocumentLoadTime` as previous queries, but lower `WriteOutputTime` because we're projecting only one property.</span></span> | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | <span data-ttu-id="ad37f-284">O 213 ms je věnovaný `UserDefinedFunctionExecutionTime` provádění UDF na každou hodnotu `c.N`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-284">About 213 ms is spent in `UserDefinedFunctionExecutionTime` executing the UDF on each value of `c.N`.</span></span> |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | <span data-ttu-id="ad37f-285">Je věnovaný přibližně 0,6 ms `IndexLookupTime` na `/Name/?`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-285">About 0.6 ms is spent in `IndexLookupTime` on `/Name/?`.</span></span> <span data-ttu-id="ad37f-286">Většina doba provádění dotazu (~ 7 ms) v `SystemFunctionExecutionTime`.</span><span class="sxs-lookup"><span data-stu-id="ad37f-286">Most of the query execution time (~7 ms) in `SystemFunctionExecutionTime`.</span></span> |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | <span data-ttu-id="ad37f-287">Dotaz se provádí jako kontrolu, protože používá `LOWER`, a jsou vráceny 500 mimo 2491 načtené dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ad37f-287">Query is performed as a scan because it uses `LOWER`, and 500 out of 2491 retrieved documents are returned.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="ad37f-288">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad37f-288">Next steps</span></span>
* <span data-ttu-id="ad37f-289">Další informace o podporovaných klíčová slova a operátory dotazu SQL najdete v tématu [dotazu SQL](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-289">To learn about the supported SQL query operators and keywords, see [SQL query](documentdb-sql-query.md).</span></span> 
* <span data-ttu-id="ad37f-290">Další informace o jednotkách žádosti, najdete v části [požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="ad37f-290">To learn about request units, see [request units](request-units.md).</span></span>
* <span data-ttu-id="ad37f-291">Další informace o zásady indexování najdete v tématu [indexování zásad](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="ad37f-291">To learn about indexing policy, see [indexing policy](indexing-policies.md)</span></span> 


