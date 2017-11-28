---
title: "Azure Cosmos DB: DocumentDB Java rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello Java API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos databáze DocumentDB Java SDK."
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
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a><span data-ttu-id="44cdd-103">Azure Cosmos DB: DocumentDB Java SDK poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="44cdd-103">Azure Cosmos DB: DocumentDB Java SDK release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="44cdd-104">.NET</span><span class="sxs-lookup"><span data-stu-id="44cdd-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="44cdd-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="44cdd-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="44cdd-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cdd-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="44cdd-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="44cdd-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="44cdd-108">Java</span><span class="sxs-lookup"><span data-stu-id="44cdd-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="44cdd-109">Python</span><span class="sxs-lookup"><span data-stu-id="44cdd-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="44cdd-110">REST</span><span class="sxs-lookup"><span data-stu-id="44cdd-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="44cdd-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="44cdd-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="44cdd-112">SQL</span><span class="sxs-lookup"><span data-stu-id="44cdd-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="44cdd-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="44cdd-113">**SDK Download**</span></span></td><td>[<span data-ttu-id="44cdd-114">Maven</span><span class="sxs-lookup"><span data-stu-id="44cdd-114">Maven</span></span>](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td><span data-ttu-id="44cdd-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="44cdd-115">**API documentation**</span></span></td><td>[<span data-ttu-id="44cdd-116">Referenční dokumentace rozhraní API Java</span><span class="sxs-lookup"><span data-stu-id="44cdd-116">Java API reference documentation</span></span>](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td><span data-ttu-id="44cdd-117">**Přispívat tooSDK**</span><span class="sxs-lookup"><span data-stu-id="44cdd-117">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="44cdd-118">GitHub</span><span class="sxs-lookup"><span data-stu-id="44cdd-118">GitHub</span></span>](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td><span data-ttu-id="44cdd-119">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="44cdd-119">**Get started**</span></span></td><td>[<span data-ttu-id="44cdd-120">Začínáme s hello sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="44cdd-120">Get started with hello Java SDK</span></span>](documentdb-java-get-started.md)</td></tr>

<tr><td><span data-ttu-id="44cdd-121">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="44cdd-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="44cdd-122">Vývoj webových aplikací s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="44cdd-122">Web application development with Azure Cosmos DB</span></span>](documentdb-java-application.md)</td></tr>

<tr><td><span data-ttu-id="44cdd-123">**Aktuální podporovaný modul runtime**</span><span class="sxs-lookup"><span data-stu-id="44cdd-123">**Current supported runtime**</span></span></td><td>[<span data-ttu-id="44cdd-124">JDK 7</span><span class="sxs-lookup"><span data-stu-id="44cdd-124">JDK 7</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="44cdd-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="44cdd-125">Release Notes</span></span>

### <a name="a-name11201120"></a><span data-ttu-id="44cdd-126"><a name="1.12.0"/>1.12.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-126"><a name="1.12.0"/>1.12.0</span></span>
* <span data-ttu-id="44cdd-127">Rozdělí toorequest důležitých oprav chyb zpracování během oddílu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-127">Critical bug fixes toorequest processing during partition splits.</span></span>
* <span data-ttu-id="44cdd-128">Oprava problému s hello silné a BoundedStaleness úrovně konzistence.</span><span class="sxs-lookup"><span data-stu-id="44cdd-128">Fixed an issue with hello Strong and BoundedStaleness consistency levels.</span></span>

### <a name="a-name11101110"></a><span data-ttu-id="44cdd-129"><a name="1.11.0"/>1.11.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-129"><a name="1.11.0"/>1.11.0</span></span>
* <span data-ttu-id="44cdd-130">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="44cdd-130">Added support for a new consistency level called ConsistentPrefix.</span></span>
* <span data-ttu-id="44cdd-131">Opravit chyby při čtení kolekce v režimu relace.</span><span class="sxs-lookup"><span data-stu-id="44cdd-131">Fixed a bug in reading collection in session mode.</span></span>

### <a name="a-name11001100"></a><span data-ttu-id="44cdd-132"><a name="1.10.0"/>1.10.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-132"><a name="1.10.0"/>1.10.0</span></span>
* <span data-ttu-id="44cdd-133">Povolená podpora dělenou kolekci s jako málo jako odpovídající 2500 RU za sekundu a škálování v přírůstcích po 100 RU za sekundu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-133">Enabled support for partitioned collection with as low as 2,500 RU/sec and scale in increments of 100 RU/sec.</span></span>
* <span data-ttu-id="44cdd-134">Pevná chyby ve hello nativní sestavení, které může způsobit výjimku NullRef v některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="44cdd-134">Fixed a bug in hello native assembly which can cause NullRef exception in some queries.</span></span>

### <a name="a-name196196"></a><span data-ttu-id="44cdd-135"><a name="1.9.6"/>1.9.6</span><span class="sxs-lookup"><span data-stu-id="44cdd-135"><a name="1.9.6"/>1.9.6</span></span>
* <span data-ttu-id="44cdd-136">Vyřešený chyby v konfiguraci modulu hello dotazu, který může způsobit, že výjimky pro dotazy v režimu brány.</span><span class="sxs-lookup"><span data-stu-id="44cdd-136">Fixed a bug in hello query engine configuration that may cause exceptions for queries in Gateway mode.</span></span>
* <span data-ttu-id="44cdd-137">Vyřešili několik chyb v kontejneru hello relace, který může způsobit výjimku "Vlastník prostředek nebyl nalezen" pro požadavky okamžitě po vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="44cdd-137">Fixed a few bugs in hello session container that may cause an "Owner resource not found" exception for requests immediately after collection creation.</span></span>

### <a name="a-name195195"></a><span data-ttu-id="44cdd-138"><a name="1.9.5"/>1.9.5</span><span class="sxs-lookup"><span data-stu-id="44cdd-138"><a name="1.9.5"/>1.9.5</span></span>
* <span data-ttu-id="44cdd-139">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="44cdd-139">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="44cdd-140">V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="44cdd-140">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="44cdd-141">Přidaná podpora pro změnu informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-141">Added support for change feed.</span></span>
* <span data-ttu-id="44cdd-142">Byla přidána podpora pro informace o kvótě kolekce prostřednictvím RequestOptions.setPopulateQuotaInfo.</span><span class="sxs-lookup"><span data-stu-id="44cdd-142">Added support for collection quota information through RequestOptions.setPopulateQuotaInfo.</span></span>
* <span data-ttu-id="44cdd-143">Byla přidána podpora pro uloženou proceduru skriptu protokolování prostřednictvím RequestOptions.setScriptLoggingEnabled.</span><span class="sxs-lookup"><span data-stu-id="44cdd-143">Added support for stored procedure script logging through RequestOptions.setScriptLoggingEnabled.</span></span>
* <span data-ttu-id="44cdd-144">Opravit chyby, kde dotazu v režimu DirectHttps může přestat reagovat při zjištění chyby omezení.</span><span class="sxs-lookup"><span data-stu-id="44cdd-144">Fixed a bug where query in DirectHttps mode may hang when encountering throttle failures.</span></span>
* <span data-ttu-id="44cdd-145">Vyřešený chyby v režimu konzistence relace.</span><span class="sxs-lookup"><span data-stu-id="44cdd-145">Fixed a bug in session consistency mode.</span></span>
* <span data-ttu-id="44cdd-146">Opravit chyby, které můžou způsobit výjimky NullReferenceException v HttpContext, když je vysoká rychlost požadavků.</span><span class="sxs-lookup"><span data-stu-id="44cdd-146">Fixed a bug which may cause NullReferenceException in HttpContext when request rate is high.</span></span>
* <span data-ttu-id="44cdd-147">Vylepšený výkon DirectHttps režimu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-147">Improved performance of DirectHttps mode.</span></span>

### <a name="a-name194194"></a><span data-ttu-id="44cdd-148"><a name="1.9.4"/>1.9.4</span><span class="sxs-lookup"><span data-stu-id="44cdd-148"><a name="1.9.4"/>1.9.4</span></span>
* <span data-ttu-id="44cdd-149">Podpora přidání jednoduchý klient založený na instancích proxy s rozhraním API ConnectionPolicy.setProxy().</span><span class="sxs-lookup"><span data-stu-id="44cdd-149">Added simple client instance-based proxy support with ConnectionPolicy.setProxy() API.</span></span>
* <span data-ttu-id="44cdd-150">Přidání rozhraní API DocumentClient.close() tooproperly vypnutí DocumentClient instance.</span><span class="sxs-lookup"><span data-stu-id="44cdd-150">Added DocumentClient.close() API tooproperly shutdown DocumentClient instance.</span></span>
* <span data-ttu-id="44cdd-151">Výkon vylepšené dotazů v režimu přímého připojení odvozením hello plán dotazu z nativní sestavení hello místo hello brány.</span><span class="sxs-lookup"><span data-stu-id="44cdd-151">Improved query performance in direct connectivity mode by deriving hello query plan from hello native assembly instead of hello Gateway.</span></span>
* <span data-ttu-id="44cdd-152">Nastavit FAIL_ON_UNKNOWN_PROPERTIES = false, takže uživatelé nepotřebují toodefine JsonIgnoreProperties v jejich POJO.</span><span class="sxs-lookup"><span data-stu-id="44cdd-152">Set FAIL_ON_UNKNOWN_PROPERTIES = false so users don't need toodefine JsonIgnoreProperties in their POJO.</span></span>
* <span data-ttu-id="44cdd-153">Rozdělili protokolování toouse SLF4J.</span><span class="sxs-lookup"><span data-stu-id="44cdd-153">Refactored logging toouse SLF4J.</span></span>
* <span data-ttu-id="44cdd-154">Vyřešili několik dalších chyb ve čtecím modulu konzistence.</span><span class="sxs-lookup"><span data-stu-id="44cdd-154">Fixed a few other bugs in consistency reader.</span></span>

### <a name="a-name193193"></a><span data-ttu-id="44cdd-155"><a name="1.9.3"/>1.9.3</span><span class="sxs-lookup"><span data-stu-id="44cdd-155"><a name="1.9.3"/>1.9.3</span></span>
* <span data-ttu-id="44cdd-156">Pevná chyby ve hello připojení správy tooprevent připojení nevracení v režimu přímého připojení.</span><span class="sxs-lookup"><span data-stu-id="44cdd-156">Fixed a bug in hello connection management tooprevent connection leaks in direct connectivity mode.</span></span>
* <span data-ttu-id="44cdd-157">Vyřešený chyby v dotazu TOP hello, kde se může vyvolat NullReferenece výjimka.</span><span class="sxs-lookup"><span data-stu-id="44cdd-157">Fixed a bug in hello TOP query where it may throw NullReferenece exception.</span></span>
* <span data-ttu-id="44cdd-158">Lepší výkon snížením hello počet volání sítě pro vnitřní mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="44cdd-158">Improved performance by reducing hello number of network call for hello internal caches.</span></span>
* <span data-ttu-id="44cdd-159">Přidání stavový kód, aktivity a URI požadavku v DocumentClientException pro lepší řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="44cdd-159">Added status code, ActivityID and Request URI in DocumentClientException for better troubleshooting.</span></span>

### <a name="a-name192192"></a><span data-ttu-id="44cdd-160"><a name="1.9.2"/>1.9.2</span><span class="sxs-lookup"><span data-stu-id="44cdd-160"><a name="1.9.2"/>1.9.2</span></span>
* <span data-ttu-id="44cdd-161">Ve správě hello připojení pro stabilitu byl opraven problém.</span><span class="sxs-lookup"><span data-stu-id="44cdd-161">Fixed an issue in hello connection management for stability.</span></span>

### <a name="a-name191191"></a><span data-ttu-id="44cdd-162"><a name="1.9.1"/>1.9.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-162"><a name="1.9.1"/>1.9.1</span></span>
* <span data-ttu-id="44cdd-163">Přidaná podpora pro úrovně konzistence BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="44cdd-163">Added support for BoundedStaleness consistency level.</span></span>
* <span data-ttu-id="44cdd-164">Byla přidána podpora pro přímé připojení k síti pro operace CRUD pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="44cdd-164">Added support for direct connectivity for CRUD operations for partitioned collections.</span></span>
* <span data-ttu-id="44cdd-165">Opravit chyby v dotazování databáze s SQL.</span><span class="sxs-lookup"><span data-stu-id="44cdd-165">Fixed a bug in querying a database with SQL.</span></span>
* <span data-ttu-id="44cdd-166">Chyby opraveny v mezipaměti relace hello kde token relace může být nastaveny nesprávně.</span><span class="sxs-lookup"><span data-stu-id="44cdd-166">Fixed a bug in hello session cache where session token may be set incorrectly.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="44cdd-167"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-167"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="44cdd-168">Přidaná podpora pro křížové paralelní dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-168">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="44cdd-169">Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="44cdd-169">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>
* <span data-ttu-id="44cdd-170">Přidaná podpora pro silnou konzistenci.</span><span class="sxs-lookup"><span data-stu-id="44cdd-170">Added support for strong consistency.</span></span>
* <span data-ttu-id="44cdd-171">Byla přidána podpora pro název na základě požadavky při použití přímé připojení.</span><span class="sxs-lookup"><span data-stu-id="44cdd-171">Added support for name based requests when using direct connectivity.</span></span>
* <span data-ttu-id="44cdd-172">Opravené toomake zůstanou ActivityId mezi všechny žádosti o opakování konzistentní.</span><span class="sxs-lookup"><span data-stu-id="44cdd-172">Fixed toomake ActivityId stay consistent across all request retries.</span></span>
* <span data-ttu-id="44cdd-173">Pevné chyby související s toohello mezipaměť relace při opětovném vytváření kolekce se hello stejný název.</span><span class="sxs-lookup"><span data-stu-id="44cdd-173">Fixed a bug related toohello session cache when recreating a collection with hello same name.</span></span>
* <span data-ttu-id="44cdd-174">Přidání mnohoúhelníku a datové typy LineString při zadávání kolekce indexování zásady pro geografického vymezení prostorových dotazů.</span><span class="sxs-lookup"><span data-stu-id="44cdd-174">Added Polygon and LineString DataTypes while specifying collection indexing policy for geo-fencing spatial queries.</span></span>
* <span data-ttu-id="44cdd-175">Opravené problémy s Java dokumentace pro jazyk Java 1.8.</span><span class="sxs-lookup"><span data-stu-id="44cdd-175">Fixed issues with Java Doc for Java 1.8.</span></span>

### <a name="a-name181181"></a><span data-ttu-id="44cdd-176"><a name="1.8.1"/>1.8.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-176"><a name="1.8.1"/>1.8.1</span></span>
* <span data-ttu-id="44cdd-177">Pevná chyby ve PartitionKeyDefinitionMap toocache kolekce tvořené jedním oddílem a není zkontrolujte navíc načíst oddílu o klíč.</span><span class="sxs-lookup"><span data-stu-id="44cdd-177">Fixed a bug in PartitionKeyDefinitionMap toocache single partition collections and not make extra fetch partition key requests.</span></span>
* <span data-ttu-id="44cdd-178">Pevné opakování toonot chyb, je-li hodnotu klíče oddílu nesprávné.</span><span class="sxs-lookup"><span data-stu-id="44cdd-178">Fixed a bug toonot retry when an incorrect partition key value is provided.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="44cdd-179"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-179"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="44cdd-180">Podpora přidání hello účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="44cdd-180">Added hello support for multi-region database accounts.</span></span>
* <span data-ttu-id="44cdd-181">Přidaná podpora pro automatické opakování na omezenému požadavků s možností toocustomize hello maximální pokusy o opakování a maximum opakujte doba čekání.</span><span class="sxs-lookup"><span data-stu-id="44cdd-181">Added support for automatic retry on throttled requests with options toocustomize hello max retry attempts and max retry wait time.</span></span>  <span data-ttu-id="44cdd-182">Zobrazit RetryOptions a ConnectionPolicy.getRetryOptions().</span><span class="sxs-lookup"><span data-stu-id="44cdd-182">See RetryOptions and ConnectionPolicy.getRetryOptions().</span></span>
* <span data-ttu-id="44cdd-183">Nepoužívané IPartitionResolver na základě vlastního rozdělení kódu.</span><span class="sxs-lookup"><span data-stu-id="44cdd-183">Deprecated IPartitionResolver based custom partitioning code.</span></span> <span data-ttu-id="44cdd-184">Použijte dělené kolekce pro vyšší propustnost a úložiště.</span><span class="sxs-lookup"><span data-stu-id="44cdd-184">Please use partitioned collections for higher storage and throughput.</span></span>

### <a name="a-name171171"></a><span data-ttu-id="44cdd-185"><a name="1.7.1"/>1.7.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-185"><a name="1.7.1"/>1.7.1</span></span>
* <span data-ttu-id="44cdd-186">Podpora zásad přidané opakování omezení.</span><span class="sxs-lookup"><span data-stu-id="44cdd-186">Added retry policy support for throttling.</span></span>  

### <a name="a-name170170"></a><span data-ttu-id="44cdd-187"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-187"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="44cdd-188">Přidání čas toolive (TTL) podpory pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="44cdd-188">Added time toolive (TTL) support for documents.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="44cdd-189"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-189"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="44cdd-190">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="44cdd-190">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <a name="a-name151151"></a><span data-ttu-id="44cdd-191"><a name="1.5.1"/>1.5.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-191"><a name="1.5.1"/>1.5.1</span></span>
* <span data-ttu-id="44cdd-192">Pevná chyby ve hodnoty hash toogenerate HashPartitionResolver v little endian toobe konzistentní s dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="44cdd-192">Fixed a bug in HashPartitionResolver toogenerate hash values in little-endian toobe consistent with other SDKs.</span></span>

### <a name="a-name150150"></a><span data-ttu-id="44cdd-193"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-193"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="44cdd-194">Přidejte hodnotu Hash p & ro tooassist překladače oddílu s horizontálního dělení aplikacemi v rámci více oddílů.</span><span class="sxs-lookup"><span data-stu-id="44cdd-194">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="44cdd-195"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-195"><a name="1.4.0"/>1.4.0</span></span>
* <span data-ttu-id="44cdd-196">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="44cdd-196">Implement Upsert.</span></span> <span data-ttu-id="44cdd-197">Přidat nové metody upsertXXX toosupport Upsert funkce.</span><span class="sxs-lookup"><span data-stu-id="44cdd-197">New upsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="44cdd-198">Implementujte, na základě ID směrování.</span><span class="sxs-lookup"><span data-stu-id="44cdd-198">Implement ID Based Routing.</span></span> <span data-ttu-id="44cdd-199">Žádné změny veřejné rozhraní API, všechny změny interní.</span><span class="sxs-lookup"><span data-stu-id="44cdd-199">No public API changes, all changes internal.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="44cdd-200"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-200"><a name="1.3.0"/>1.3.0</span></span>
* <span data-ttu-id="44cdd-201">Verze přeskočen číslo verze toobring v zarovnání s dalších sadách SDK</span><span class="sxs-lookup"><span data-stu-id="44cdd-201">Release skipped toobring version number in alignment with other SDKs</span></span>

### <a name="a-name120120"></a><span data-ttu-id="44cdd-202"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-202"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="44cdd-203">Podporuje geoprostorové indexu</span><span class="sxs-lookup"><span data-stu-id="44cdd-203">Supports GeoSpatial Index</span></span>
* <span data-ttu-id="44cdd-204">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="44cdd-204">Validates id property for all resources.</span></span> <span data-ttu-id="44cdd-205">Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="44cdd-205">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="44cdd-206">Přidá nový tooResourceResponse "průběh transformace index" záhlaví.</span><span class="sxs-lookup"><span data-stu-id="44cdd-206">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="44cdd-207"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-207"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="44cdd-208">Implementuje zásady indexování V2</span><span class="sxs-lookup"><span data-stu-id="44cdd-208">Implements V2 indexing policy</span></span>

### <a name="a-name100100"></a><span data-ttu-id="44cdd-209"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-209"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="44cdd-210">GA SDK</span><span class="sxs-lookup"><span data-stu-id="44cdd-210">GA SDK</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="44cdd-211">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="44cdd-211">Release & Retirement Dates</span></span>
<span data-ttu-id="44cdd-212">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="44cdd-212">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="44cdd-213">Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK a jako takový je, že jste vždy upgradu toohello nejnovější verze sady SDK, abyste co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="44cdd-213">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="44cdd-214">Všechny žádosti o tooCosmos DB pomocí vyřazeno sady SDK budou odmítnuty službou hello.</span><span class="sxs-lookup"><span data-stu-id="44cdd-214">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="44cdd-215">Všechny verze hello DocumentDB SDK pro předchozí tooversion Java **1.0.0** vyřadí na **29. února 2016**.</span><span class="sxs-lookup"><span data-stu-id="44cdd-215">All versions of hello DocumentDB SDK for Java prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span>
> 
> 

<br/>

| <span data-ttu-id="44cdd-216">Verze</span><span class="sxs-lookup"><span data-stu-id="44cdd-216">Version</span></span> | <span data-ttu-id="44cdd-217">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="44cdd-217">Release Date</span></span> | <span data-ttu-id="44cdd-218">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="44cdd-218">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="44cdd-219">1.12.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-219">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="44cdd-220">11 července 2017</span><span class="sxs-lookup"><span data-stu-id="44cdd-220">July 11, 2017</span></span> |--- |
| [<span data-ttu-id="44cdd-221">1.11.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-221">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="44cdd-222">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="44cdd-222">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="44cdd-223">1.10.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-223">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="44cdd-224">11 března 2017</span><span class="sxs-lookup"><span data-stu-id="44cdd-224">March 11, 2017</span></span> |--- |
| [<span data-ttu-id="44cdd-225">1.9.6</span><span class="sxs-lookup"><span data-stu-id="44cdd-225">1.9.6</span></span>](#1.9.6) |<span data-ttu-id="44cdd-226">21. února 2017</span><span class="sxs-lookup"><span data-stu-id="44cdd-226">February 21, 2017</span></span> |--- |
| [<span data-ttu-id="44cdd-227">1.9.5</span><span class="sxs-lookup"><span data-stu-id="44cdd-227">1.9.5</span></span>](#1.9.5) |<span data-ttu-id="44cdd-228">31. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="44cdd-228">January 31, 2017</span></span> |--- |
| [<span data-ttu-id="44cdd-229">1.9.4</span><span class="sxs-lookup"><span data-stu-id="44cdd-229">1.9.4</span></span>](#1.9.4) |<span data-ttu-id="44cdd-230">24 od listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-230">November 24, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-231">1.9.3</span><span class="sxs-lookup"><span data-stu-id="44cdd-231">1.9.3</span></span>](#1.9.3) |<span data-ttu-id="44cdd-232">30. října 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-232">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-233">1.9.2</span><span class="sxs-lookup"><span data-stu-id="44cdd-233">1.9.2</span></span>](#1.9.2) |<span data-ttu-id="44cdd-234">28. října 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-234">October 28, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-235">1.9.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-235">1.9.1</span></span>](#1.9.1) |<span data-ttu-id="44cdd-236">26 října 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-236">October 26, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-237">1.9.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-237">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="44cdd-238">03. října 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-238">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-239">1.8.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-239">1.8.1</span></span>](#1.8.1) |<span data-ttu-id="44cdd-240">30. června 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-240">June 30, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-241">1.8.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-241">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="44cdd-242">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-242">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-243">1.7.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-243">1.7.1</span></span>](#1.7.1) |<span data-ttu-id="44cdd-244">30. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-244">April 30, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-245">1.7.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-245">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="44cdd-246">27. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-246">April 27, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-247">1.6.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-247">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="44cdd-248">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-248">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="44cdd-249">1.5.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-249">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="44cdd-250">31. prosince 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-250">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-251">1.5.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-251">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="44cdd-252">04 prosince 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-252">December 04, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-253">1.4.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-253">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="44cdd-254">05 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-254">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-255">1.3.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-255">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="44cdd-256">05 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-256">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-257">1.2.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-257">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="44cdd-258">05 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-258">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-259">1.1.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-259">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="44cdd-260">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-260">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-261">1.0.1</span><span class="sxs-lookup"><span data-stu-id="44cdd-261">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="44cdd-262">12 květen 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-262">May 12, 2015</span></span> |--- |
| [<span data-ttu-id="44cdd-263">1.0.0</span><span class="sxs-lookup"><span data-stu-id="44cdd-263">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="44cdd-264">07 duben 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-264">April 07, 2015</span></span> |--- |
| <span data-ttu-id="44cdd-265">0.9.5-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-265">0.9.5-prelease</span></span> |<span data-ttu-id="44cdd-266">09 března 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-266">Mar 09, 2015</span></span> |<span data-ttu-id="44cdd-267">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-267">February 29, 2016</span></span> |
| <span data-ttu-id="44cdd-268">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-268">0.9.4-prelease</span></span> |<span data-ttu-id="44cdd-269">17 únor 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-269">February 17, 2015</span></span> |<span data-ttu-id="44cdd-270">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-270">February 29, 2016</span></span> |
| <span data-ttu-id="44cdd-271">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-271">0.9.3-prelease</span></span> |<span data-ttu-id="44cdd-272">13. ledna 2015</span><span class="sxs-lookup"><span data-stu-id="44cdd-272">January 13, 2015</span></span> |<span data-ttu-id="44cdd-273">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-273">February 29, 2016</span></span> |
| <span data-ttu-id="44cdd-274">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-274">0.9.2-prelease</span></span> |<span data-ttu-id="44cdd-275">19 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="44cdd-275">December 19, 2014</span></span> |<span data-ttu-id="44cdd-276">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-276">February 29, 2016</span></span> |
| <span data-ttu-id="44cdd-277">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-277">0.9.1-prelease</span></span> |<span data-ttu-id="44cdd-278">19 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="44cdd-278">December 19, 2014</span></span> |<span data-ttu-id="44cdd-279">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-279">February 29, 2016</span></span> |
| <span data-ttu-id="44cdd-280">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="44cdd-280">0.9.0-prelease</span></span> |<span data-ttu-id="44cdd-281">10. prosince 2014</span><span class="sxs-lookup"><span data-stu-id="44cdd-281">December 10, 2014</span></span> |<span data-ttu-id="44cdd-282">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="44cdd-282">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="44cdd-283">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="44cdd-283">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="44cdd-284">Viz také</span><span class="sxs-lookup"><span data-stu-id="44cdd-284">See Also</span></span>
<span data-ttu-id="44cdd-285">toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="44cdd-285">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

