---
title: "Azure Cosmos DB: DocumentDB Java rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o rozhraní API Java a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi Azure Cosmos databáze DocumentDB Java SDK."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15e3f7ef3bfd6b1f61fe6081a378bdb29e0a1aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a><span data-ttu-id="0b928-103">Azure Cosmos DB: DocumentDB Java SDK poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="0b928-103">Azure Cosmos DB: DocumentDB Java SDK release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b928-104">.NET</span><span class="sxs-lookup"><span data-stu-id="0b928-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="0b928-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="0b928-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="0b928-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b928-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="0b928-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="0b928-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="0b928-108">Java</span><span class="sxs-lookup"><span data-stu-id="0b928-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="0b928-109">Python</span><span class="sxs-lookup"><span data-stu-id="0b928-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="0b928-110">REST</span><span class="sxs-lookup"><span data-stu-id="0b928-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="0b928-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="0b928-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="0b928-112">SQL</span><span class="sxs-lookup"><span data-stu-id="0b928-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="0b928-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="0b928-113">**SDK Download**</span></span></td><td>[<span data-ttu-id="0b928-114">Maven</span><span class="sxs-lookup"><span data-stu-id="0b928-114">Maven</span></span>](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td><span data-ttu-id="0b928-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="0b928-115">**API documentation**</span></span></td><td>[<span data-ttu-id="0b928-116">Referenční dokumentace rozhraní API Java</span><span class="sxs-lookup"><span data-stu-id="0b928-116">Java API reference documentation</span></span>](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td><span data-ttu-id="0b928-117">**Můžete přispět k sadě SDK**</span><span class="sxs-lookup"><span data-stu-id="0b928-117">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="0b928-118">GitHub</span><span class="sxs-lookup"><span data-stu-id="0b928-118">GitHub</span></span>](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td><span data-ttu-id="0b928-119">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="0b928-119">**Get started**</span></span></td><td>[<span data-ttu-id="0b928-120">Začínáme s sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="0b928-120">Get started with the Java SDK</span></span>](documentdb-java-get-started.md)</td></tr>

<tr><td><span data-ttu-id="0b928-121">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="0b928-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="0b928-122">Vývoj webových aplikací s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0b928-122">Web application development with Azure Cosmos DB</span></span>](documentdb-java-application.md)</td></tr>

<tr><td><span data-ttu-id="0b928-123">**Aktuální podporovaný modul runtime**</span><span class="sxs-lookup"><span data-stu-id="0b928-123">**Current supported runtime**</span></span></td><td>[<span data-ttu-id="0b928-124">JDK 7</span><span class="sxs-lookup"><span data-stu-id="0b928-124">JDK 7</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="0b928-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="0b928-125">Release Notes</span></span>

### <a name="a-name11201120"></a><span data-ttu-id="0b928-126"><a name="1.12.0"/>1.12.0</span><span class="sxs-lookup"><span data-stu-id="0b928-126"><a name="1.12.0"/>1.12.0</span></span>
* <span data-ttu-id="0b928-127">Důležité opravy chyb pro zpracování během rozdělení oddílů žádosti.</span><span class="sxs-lookup"><span data-stu-id="0b928-127">Critical bug fixes to request processing during partition splits.</span></span>
* <span data-ttu-id="0b928-128">Oprava problému s úrovní konzistence silné a BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="0b928-128">Fixed an issue with the Strong and BoundedStaleness consistency levels.</span></span>

### <a name="a-name11101110"></a><span data-ttu-id="0b928-129"><a name="1.11.0"/>1.11.0</span><span class="sxs-lookup"><span data-stu-id="0b928-129"><a name="1.11.0"/>1.11.0</span></span>
* <span data-ttu-id="0b928-130">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="0b928-130">Added support for a new consistency level called ConsistentPrefix.</span></span>
* <span data-ttu-id="0b928-131">Opravit chyby při čtení kolekce v režimu relace.</span><span class="sxs-lookup"><span data-stu-id="0b928-131">Fixed a bug in reading collection in session mode.</span></span>

### <a name="a-name11001100"></a><span data-ttu-id="0b928-132"><a name="1.10.0"/>1.10.0</span><span class="sxs-lookup"><span data-stu-id="0b928-132"><a name="1.10.0"/>1.10.0</span></span>
* <span data-ttu-id="0b928-133">Povolená podpora dělenou kolekci s jako málo jako odpovídající 2500 RU za sekundu a škálování v přírůstcích po 100 RU za sekundu.</span><span class="sxs-lookup"><span data-stu-id="0b928-133">Enabled support for partitioned collection with as low as 2,500 RU/sec and scale in increments of 100 RU/sec.</span></span>
* <span data-ttu-id="0b928-134">Chyby opraveny v nativní sestavení, které může způsobit výjimku NullRef v některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="0b928-134">Fixed a bug in the native assembly which can cause NullRef exception in some queries.</span></span>

### <a name="a-name196196"></a><span data-ttu-id="0b928-135"><a name="1.9.6"/>1.9.6</span><span class="sxs-lookup"><span data-stu-id="0b928-135"><a name="1.9.6"/>1.9.6</span></span>
* <span data-ttu-id="0b928-136">Vyřešený chyby v konfiguraci modulu dotazu, který může způsobit, že výjimky pro dotazy v režimu brány.</span><span class="sxs-lookup"><span data-stu-id="0b928-136">Fixed a bug in the query engine configuration that may cause exceptions for queries in Gateway mode.</span></span>
* <span data-ttu-id="0b928-137">Vyřešili několik chyb v kontejneru relace, který může způsobit výjimku "Vlastník prostředek nebyl nalezen" pro požadavky okamžitě po vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="0b928-137">Fixed a few bugs in the session container that may cause an "Owner resource not found" exception for requests immediately after collection creation.</span></span>

### <a name="a-name195195"></a><span data-ttu-id="0b928-138"><a name="1.9.5"/>1.9.5</span><span class="sxs-lookup"><span data-stu-id="0b928-138"><a name="1.9.5"/>1.9.5</span></span>
* <span data-ttu-id="0b928-139">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="0b928-139">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="0b928-140">V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="0b928-140">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="0b928-141">Přidaná podpora pro změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="0b928-141">Added support for change feed.</span></span>
* <span data-ttu-id="0b928-142">Byla přidána podpora pro informace o kvótě kolekce prostřednictvím RequestOptions.setPopulateQuotaInfo.</span><span class="sxs-lookup"><span data-stu-id="0b928-142">Added support for collection quota information through RequestOptions.setPopulateQuotaInfo.</span></span>
* <span data-ttu-id="0b928-143">Byla přidána podpora pro uloženou proceduru skriptu protokolování prostřednictvím RequestOptions.setScriptLoggingEnabled.</span><span class="sxs-lookup"><span data-stu-id="0b928-143">Added support for stored procedure script logging through RequestOptions.setScriptLoggingEnabled.</span></span>
* <span data-ttu-id="0b928-144">Opravit chyby, kde dotazu v režimu DirectHttps může přestat reagovat při zjištění chyby omezení.</span><span class="sxs-lookup"><span data-stu-id="0b928-144">Fixed a bug where query in DirectHttps mode may hang when encountering throttle failures.</span></span>
* <span data-ttu-id="0b928-145">Vyřešený chyby v režimu konzistence relace.</span><span class="sxs-lookup"><span data-stu-id="0b928-145">Fixed a bug in session consistency mode.</span></span>
* <span data-ttu-id="0b928-146">Opravit chyby, které můžou způsobit výjimky NullReferenceException v HttpContext, když je vysoká rychlost požadavků.</span><span class="sxs-lookup"><span data-stu-id="0b928-146">Fixed a bug which may cause NullReferenceException in HttpContext when request rate is high.</span></span>
* <span data-ttu-id="0b928-147">Vylepšený výkon DirectHttps režimu.</span><span class="sxs-lookup"><span data-stu-id="0b928-147">Improved performance of DirectHttps mode.</span></span>

### <a name="a-name194194"></a><span data-ttu-id="0b928-148"><a name="1.9.4"/>1.9.4</span><span class="sxs-lookup"><span data-stu-id="0b928-148"><a name="1.9.4"/>1.9.4</span></span>
* <span data-ttu-id="0b928-149">Podpora přidání jednoduchý klient založený na instancích proxy s rozhraním API ConnectionPolicy.setProxy().</span><span class="sxs-lookup"><span data-stu-id="0b928-149">Added simple client instance-based proxy support with ConnectionPolicy.setProxy() API.</span></span>
* <span data-ttu-id="0b928-150">Přidání rozhraní API DocumentClient.close() správně instance DocumentClient vypnutí.</span><span class="sxs-lookup"><span data-stu-id="0b928-150">Added DocumentClient.close() API to properly shutdown DocumentClient instance.</span></span>
* <span data-ttu-id="0b928-151">Zvýšení výkonu dotazů v režimu přímého připojení odvozením plán dotazu z nativní sestavení místo brány.</span><span class="sxs-lookup"><span data-stu-id="0b928-151">Improved query performance in direct connectivity mode by deriving the query plan from the native assembly instead of the Gateway.</span></span>
* <span data-ttu-id="0b928-152">Nastavit FAIL_ON_UNKNOWN_PROPERTIES = false, takže uživatelé nepotřebují k definování JsonIgnoreProperties v jejich POJO.</span><span class="sxs-lookup"><span data-stu-id="0b928-152">Set FAIL_ON_UNKNOWN_PROPERTIES = false so users don't need to define JsonIgnoreProperties in their POJO.</span></span>
* <span data-ttu-id="0b928-153">Rozdělili protokolování použití SLF4J.</span><span class="sxs-lookup"><span data-stu-id="0b928-153">Refactored logging to use SLF4J.</span></span>
* <span data-ttu-id="0b928-154">Vyřešili několik dalších chyb ve čtecím modulu konzistence.</span><span class="sxs-lookup"><span data-stu-id="0b928-154">Fixed a few other bugs in consistency reader.</span></span>

### <a name="a-name193193"></a><span data-ttu-id="0b928-155"><a name="1.9.3"/>1.9.3</span><span class="sxs-lookup"><span data-stu-id="0b928-155"><a name="1.9.3"/>1.9.3</span></span>
* <span data-ttu-id="0b928-156">Pevné chyby ve správě připojení k prevence úniků připojení v režimu přímého připojení.</span><span class="sxs-lookup"><span data-stu-id="0b928-156">Fixed a bug in the connection management to prevent connection leaks in direct connectivity mode.</span></span>
* <span data-ttu-id="0b928-157">Vyřešený chyby v dotazu TOP, kde se může vyvolat NullReferenece výjimka.</span><span class="sxs-lookup"><span data-stu-id="0b928-157">Fixed a bug in the TOP query where it may throw NullReferenece exception.</span></span>
* <span data-ttu-id="0b928-158">Lepší výkon snížením počtu volání sítě pro vnitřní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0b928-158">Improved performance by reducing the number of network call for the internal caches.</span></span>
* <span data-ttu-id="0b928-159">Přidání stavový kód, aktivity a URI požadavku v DocumentClientException pro lepší řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0b928-159">Added status code, ActivityID and Request URI in DocumentClientException for better troubleshooting.</span></span>

### <a name="a-name192192"></a><span data-ttu-id="0b928-160"><a name="1.9.2"/>1.9.2</span><span class="sxs-lookup"><span data-stu-id="0b928-160"><a name="1.9.2"/>1.9.2</span></span>
* <span data-ttu-id="0b928-161">Ve správě připojení pro stabilitu byl opraven problém.</span><span class="sxs-lookup"><span data-stu-id="0b928-161">Fixed an issue in the connection management for stability.</span></span>

### <a name="a-name191191"></a><span data-ttu-id="0b928-162"><a name="1.9.1"/>1.9.1</span><span class="sxs-lookup"><span data-stu-id="0b928-162"><a name="1.9.1"/>1.9.1</span></span>
* <span data-ttu-id="0b928-163">Přidaná podpora pro úrovně konzistence BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="0b928-163">Added support for BoundedStaleness consistency level.</span></span>
* <span data-ttu-id="0b928-164">Byla přidána podpora pro přímé připojení k síti pro operace CRUD pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="0b928-164">Added support for direct connectivity for CRUD operations for partitioned collections.</span></span>
* <span data-ttu-id="0b928-165">Opravit chyby v dotazování databáze s SQL.</span><span class="sxs-lookup"><span data-stu-id="0b928-165">Fixed a bug in querying a database with SQL.</span></span>
* <span data-ttu-id="0b928-166">Chyby opraveny v mezipaměti relace, kde token relace může být nastaveny nesprávně.</span><span class="sxs-lookup"><span data-stu-id="0b928-166">Fixed a bug in the session cache where session token may be set incorrectly.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="0b928-167"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="0b928-167"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="0b928-168">Přidaná podpora pro křížové paralelní dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="0b928-168">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="0b928-169">Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="0b928-169">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>
* <span data-ttu-id="0b928-170">Přidaná podpora pro silnou konzistenci.</span><span class="sxs-lookup"><span data-stu-id="0b928-170">Added support for strong consistency.</span></span>
* <span data-ttu-id="0b928-171">Byla přidána podpora pro název na základě požadavky při použití přímé připojení.</span><span class="sxs-lookup"><span data-stu-id="0b928-171">Added support for name based requests when using direct connectivity.</span></span>
* <span data-ttu-id="0b928-172">Pevné aby zůstat konzistentní napříč všechny žádosti o opakování aktivity.</span><span class="sxs-lookup"><span data-stu-id="0b928-172">Fixed to make ActivityId stay consistent across all request retries.</span></span>
* <span data-ttu-id="0b928-173">Opravit chyby související s mezipaměti relace při opětovném vytváření kolekce se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="0b928-173">Fixed a bug related to the session cache when recreating a collection with the same name.</span></span>
* <span data-ttu-id="0b928-174">Přidání mnohoúhelníku a datové typy LineString při zadávání kolekce indexování zásady pro geografického vymezení prostorových dotazů.</span><span class="sxs-lookup"><span data-stu-id="0b928-174">Added Polygon and LineString DataTypes while specifying collection indexing policy for geo-fencing spatial queries.</span></span>
* <span data-ttu-id="0b928-175">Opravené problémy s Java dokumentace pro jazyk Java 1.8.</span><span class="sxs-lookup"><span data-stu-id="0b928-175">Fixed issues with Java Doc for Java 1.8.</span></span>

### <a name="a-name181181"></a><span data-ttu-id="0b928-176"><a name="1.8.1"/>1.8.1</span><span class="sxs-lookup"><span data-stu-id="0b928-176"><a name="1.8.1"/>1.8.1</span></span>
* <span data-ttu-id="0b928-177">Pevná chyby ve PartitionKeyDefinitionMap ukládat do mezipaměti kolekce tvořené jedním oddílem a zajistěte, aby nebyly navíc načtení oddílu o klíč.</span><span class="sxs-lookup"><span data-stu-id="0b928-177">Fixed a bug in PartitionKeyDefinitionMap to cache single partition collections and not make extra fetch partition key requests.</span></span>
* <span data-ttu-id="0b928-178">Opravit chyby není opakovat, je-li hodnotu klíče oddílu nesprávné.</span><span class="sxs-lookup"><span data-stu-id="0b928-178">Fixed a bug to not retry when an incorrect partition key value is provided.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="0b928-179"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="0b928-179"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="0b928-180">Přidaná podpora pro účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="0b928-180">Added the support for multi-region database accounts.</span></span>
* <span data-ttu-id="0b928-181">Přidaná podpora pro automatické opakování u omezenému požadavků s možností přizpůsobení maximální počet opakovaných pokusů a doba čekání na maximální počet opakování.</span><span class="sxs-lookup"><span data-stu-id="0b928-181">Added support for automatic retry on throttled requests with options to customize the max retry attempts and max retry wait time.</span></span>  <span data-ttu-id="0b928-182">Zobrazit RetryOptions a ConnectionPolicy.getRetryOptions().</span><span class="sxs-lookup"><span data-stu-id="0b928-182">See RetryOptions and ConnectionPolicy.getRetryOptions().</span></span>
* <span data-ttu-id="0b928-183">Nepoužívané IPartitionResolver na základě vlastního rozdělení kódu.</span><span class="sxs-lookup"><span data-stu-id="0b928-183">Deprecated IPartitionResolver based custom partitioning code.</span></span> <span data-ttu-id="0b928-184">Použijte dělené kolekce pro vyšší propustnost a úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b928-184">Please use partitioned collections for higher storage and throughput.</span></span>

### <a name="a-name171171"></a><span data-ttu-id="0b928-185"><a name="1.7.1"/>1.7.1</span><span class="sxs-lookup"><span data-stu-id="0b928-185"><a name="1.7.1"/>1.7.1</span></span>
* <span data-ttu-id="0b928-186">Podpora zásad přidané opakování omezení.</span><span class="sxs-lookup"><span data-stu-id="0b928-186">Added retry policy support for throttling.</span></span>  

### <a name="a-name170170"></a><span data-ttu-id="0b928-187"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="0b928-187"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="0b928-188">Další čas live (TTL) podpory pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="0b928-188">Added time to live (TTL) support for documents.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="0b928-189"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="0b928-189"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="0b928-190">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="0b928-190">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <a name="a-name151151"></a><span data-ttu-id="0b928-191"><a name="1.5.1"/>1.5.1</span><span class="sxs-lookup"><span data-stu-id="0b928-191"><a name="1.5.1"/>1.5.1</span></span>
* <span data-ttu-id="0b928-192">Pevná chyby ve HashPartitionResolver ke generování hodnoty hash v little endian, aby byla konzistentní se dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="0b928-192">Fixed a bug in HashPartitionResolver to generate hash values in little-endian to be consistent with other SDKs.</span></span>

### <a name="a-name150150"></a><span data-ttu-id="0b928-193"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="0b928-193"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="0b928-194">Přidejte hodnotu Hash p & ro oddílu překladače pomoct s aplikacemi horizontálního dělení napříč více oddílů.</span><span class="sxs-lookup"><span data-stu-id="0b928-194">Add Hash & Range partition resolvers to assist with sharding applications across multiple partitions.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="0b928-195"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="0b928-195"><a name="1.4.0"/>1.4.0</span></span>
* <span data-ttu-id="0b928-196">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="0b928-196">Implement Upsert.</span></span> <span data-ttu-id="0b928-197">Přidaná kvůli podpoře funkcí Upsert nové metody upsertXXX.</span><span class="sxs-lookup"><span data-stu-id="0b928-197">New upsertXXX methods added to support Upsert feature.</span></span>
* <span data-ttu-id="0b928-198">Implementujte, na základě ID směrování.</span><span class="sxs-lookup"><span data-stu-id="0b928-198">Implement ID Based Routing.</span></span> <span data-ttu-id="0b928-199">Žádné změny veřejné rozhraní API, všechny změny interní.</span><span class="sxs-lookup"><span data-stu-id="0b928-199">No public API changes, all changes internal.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="0b928-200"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="0b928-200"><a name="1.3.0"/>1.3.0</span></span>
* <span data-ttu-id="0b928-201">Verze přeskočen mají být předány číslo verze zarovnání s dalších sadách SDK</span><span class="sxs-lookup"><span data-stu-id="0b928-201">Release skipped to bring version number in alignment with other SDKs</span></span>

### <a name="a-name120120"></a><span data-ttu-id="0b928-202"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="0b928-202"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="0b928-203">Podporuje geoprostorové indexu</span><span class="sxs-lookup"><span data-stu-id="0b928-203">Supports GeoSpatial Index</span></span>
* <span data-ttu-id="0b928-204">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0b928-204">Validates id property for all resources.</span></span> <span data-ttu-id="0b928-205">Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="0b928-205">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="0b928-206">Přidá nové záhlaví "index transformace průběh" ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="0b928-206">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="0b928-207"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="0b928-207"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="0b928-208">Implementuje zásady indexování V2</span><span class="sxs-lookup"><span data-stu-id="0b928-208">Implements V2 indexing policy</span></span>

### <a name="a-name100100"></a><span data-ttu-id="0b928-209"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="0b928-209"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="0b928-210">GA SDK</span><span class="sxs-lookup"><span data-stu-id="0b928-210">GA SDK</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="0b928-211">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="0b928-211">Release & Retirement Dates</span></span>
<span data-ttu-id="0b928-212">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** předem vyřazení sady SDK k funkce smooth přechodu na novější nebo podporované verzi.</span><span class="sxs-lookup"><span data-stu-id="0b928-212">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="0b928-213">Nové funkce a funkce a optimalizace, jsou přidány pouze v aktuální sadě SDK, jako takový je doporučujeme, aby vždy upgradu na nejnovější verze sady SDK v míře.</span><span class="sxs-lookup"><span data-stu-id="0b928-213">New features and functionality and optimizations are only added to the current SDK, as such it is  recommend that you always upgrade to the latest SDK version as early as possible.</span></span>

<span data-ttu-id="0b928-214">Každá žádost o DB Cosmos pomocí vyřazeno sady SDK budou odmítnuty službou.</span><span class="sxs-lookup"><span data-stu-id="0b928-214">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

> [!WARNING]
> <span data-ttu-id="0b928-215">Všechny verze sady SDK DocumentDB pro jazyk Java starší než verze **1.0.0** vyřadí na **29. února 2016**.</span><span class="sxs-lookup"><span data-stu-id="0b928-215">All versions of the DocumentDB SDK for Java prior to version **1.0.0** will be retired on **February 29, 2016**.</span></span>
> 
> 

<br/>

| <span data-ttu-id="0b928-216">Verze</span><span class="sxs-lookup"><span data-stu-id="0b928-216">Version</span></span> | <span data-ttu-id="0b928-217">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="0b928-217">Release Date</span></span> | <span data-ttu-id="0b928-218">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="0b928-218">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="0b928-219">1.12.0</span><span class="sxs-lookup"><span data-stu-id="0b928-219">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="0b928-220">11 července 2017</span><span class="sxs-lookup"><span data-stu-id="0b928-220">July 11, 2017</span></span> |--- |
| [<span data-ttu-id="0b928-221">1.11.0</span><span class="sxs-lookup"><span data-stu-id="0b928-221">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="0b928-222">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="0b928-222">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="0b928-223">1.10.0</span><span class="sxs-lookup"><span data-stu-id="0b928-223">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="0b928-224">11 března 2017</span><span class="sxs-lookup"><span data-stu-id="0b928-224">March 11, 2017</span></span> |--- |
| [<span data-ttu-id="0b928-225">1.9.6</span><span class="sxs-lookup"><span data-stu-id="0b928-225">1.9.6</span></span>](#1.9.6) |<span data-ttu-id="0b928-226">21. února 2017</span><span class="sxs-lookup"><span data-stu-id="0b928-226">February 21, 2017</span></span> |--- |
| [<span data-ttu-id="0b928-227">1.9.5</span><span class="sxs-lookup"><span data-stu-id="0b928-227">1.9.5</span></span>](#1.9.5) |<span data-ttu-id="0b928-228">31. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="0b928-228">January 31, 2017</span></span> |--- |
| [<span data-ttu-id="0b928-229">1.9.4</span><span class="sxs-lookup"><span data-stu-id="0b928-229">1.9.4</span></span>](#1.9.4) |<span data-ttu-id="0b928-230">24 od listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-230">November 24, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-231">1.9.3</span><span class="sxs-lookup"><span data-stu-id="0b928-231">1.9.3</span></span>](#1.9.3) |<span data-ttu-id="0b928-232">30. října 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-232">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-233">1.9.2</span><span class="sxs-lookup"><span data-stu-id="0b928-233">1.9.2</span></span>](#1.9.2) |<span data-ttu-id="0b928-234">28. října 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-234">October 28, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-235">1.9.1</span><span class="sxs-lookup"><span data-stu-id="0b928-235">1.9.1</span></span>](#1.9.1) |<span data-ttu-id="0b928-236">26 října 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-236">October 26, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-237">1.9.0</span><span class="sxs-lookup"><span data-stu-id="0b928-237">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="0b928-238">03. října 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-238">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-239">1.8.1</span><span class="sxs-lookup"><span data-stu-id="0b928-239">1.8.1</span></span>](#1.8.1) |<span data-ttu-id="0b928-240">30. června 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-240">June 30, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-241">1.8.0</span><span class="sxs-lookup"><span data-stu-id="0b928-241">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="0b928-242">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-242">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-243">1.7.1</span><span class="sxs-lookup"><span data-stu-id="0b928-243">1.7.1</span></span>](#1.7.1) |<span data-ttu-id="0b928-244">30. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-244">April 30, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-245">1.7.0</span><span class="sxs-lookup"><span data-stu-id="0b928-245">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="0b928-246">27. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-246">April 27, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-247">1.6.0</span><span class="sxs-lookup"><span data-stu-id="0b928-247">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="0b928-248">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-248">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="0b928-249">1.5.1</span><span class="sxs-lookup"><span data-stu-id="0b928-249">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="0b928-250">31. prosince 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-250">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-251">1.5.0</span><span class="sxs-lookup"><span data-stu-id="0b928-251">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="0b928-252">04 prosince 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-252">December 04, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-253">1.4.0</span><span class="sxs-lookup"><span data-stu-id="0b928-253">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="0b928-254">05 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-254">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-255">1.3.0</span><span class="sxs-lookup"><span data-stu-id="0b928-255">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="0b928-256">05 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-256">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-257">1.2.0</span><span class="sxs-lookup"><span data-stu-id="0b928-257">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="0b928-258">05 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-258">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-259">1.1.0</span><span class="sxs-lookup"><span data-stu-id="0b928-259">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="0b928-260">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-260">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-261">1.0.1</span><span class="sxs-lookup"><span data-stu-id="0b928-261">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="0b928-262">12 květen 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-262">May 12, 2015</span></span> |--- |
| [<span data-ttu-id="0b928-263">1.0.0</span><span class="sxs-lookup"><span data-stu-id="0b928-263">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="0b928-264">07 duben 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-264">April 07, 2015</span></span> |--- |
| <span data-ttu-id="0b928-265">0.9.5-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-265">0.9.5-prelease</span></span> |<span data-ttu-id="0b928-266">09 března 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-266">Mar 09, 2015</span></span> |<span data-ttu-id="0b928-267">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-267">February 29, 2016</span></span> |
| <span data-ttu-id="0b928-268">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-268">0.9.4-prelease</span></span> |<span data-ttu-id="0b928-269">17 únor 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-269">February 17, 2015</span></span> |<span data-ttu-id="0b928-270">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-270">February 29, 2016</span></span> |
| <span data-ttu-id="0b928-271">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-271">0.9.3-prelease</span></span> |<span data-ttu-id="0b928-272">13. ledna 2015</span><span class="sxs-lookup"><span data-stu-id="0b928-272">January 13, 2015</span></span> |<span data-ttu-id="0b928-273">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-273">February 29, 2016</span></span> |
| <span data-ttu-id="0b928-274">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-274">0.9.2-prelease</span></span> |<span data-ttu-id="0b928-275">19 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="0b928-275">December 19, 2014</span></span> |<span data-ttu-id="0b928-276">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-276">February 29, 2016</span></span> |
| <span data-ttu-id="0b928-277">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-277">0.9.1-prelease</span></span> |<span data-ttu-id="0b928-278">19 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="0b928-278">December 19, 2014</span></span> |<span data-ttu-id="0b928-279">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-279">February 29, 2016</span></span> |
| <span data-ttu-id="0b928-280">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="0b928-280">0.9.0-prelease</span></span> |<span data-ttu-id="0b928-281">10. prosince 2014</span><span class="sxs-lookup"><span data-stu-id="0b928-281">December 10, 2014</span></span> |<span data-ttu-id="0b928-282">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="0b928-282">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="0b928-283">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="0b928-283">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="0b928-284">Viz také</span><span class="sxs-lookup"><span data-stu-id="0b928-284">See Also</span></span>
<span data-ttu-id="0b928-285">Další informace o Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="0b928-285">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

