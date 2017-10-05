---
title: "Azure Cosmos DB Python rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o rozhraní API pro Python a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi sady Azure Cosmos DB Python SDK."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70d2550f713ff0e9daed235eb8053589b8682633
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a><span data-ttu-id="b44ad-103">Azure Python Cosmos DB SDK: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="b44ad-103">Azure Cosmos DB Python SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b44ad-104">.NET</span><span class="sxs-lookup"><span data-stu-id="b44ad-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="b44ad-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b44ad-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="b44ad-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b44ad-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="b44ad-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="b44ad-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="b44ad-108">Java</span><span class="sxs-lookup"><span data-stu-id="b44ad-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="b44ad-109">Python</span><span class="sxs-lookup"><span data-stu-id="b44ad-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="b44ad-110">REST</span><span class="sxs-lookup"><span data-stu-id="b44ad-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="b44ad-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="b44ad-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="b44ad-112">SQL</span><span class="sxs-lookup"><span data-stu-id="b44ad-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="b44ad-113">**Stáhněte si sadu SDK**</span><span class="sxs-lookup"><span data-stu-id="b44ad-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="b44ad-114">Úložiště PyPI</span><span class="sxs-lookup"><span data-stu-id="b44ad-114">PyPI</span></span>](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td><span data-ttu-id="b44ad-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="b44ad-115">**API documentation**</span></span></td><td>[<span data-ttu-id="b44ad-116">Referenční dokumentace rozhraní API jazyka Python</span><span class="sxs-lookup"><span data-stu-id="b44ad-116">Python API reference documentation</span></span>](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td><span data-ttu-id="b44ad-117">**Pokyny k instalaci sady SDK**</span><span class="sxs-lookup"><span data-stu-id="b44ad-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="b44ad-118">Pokyny k instalaci Python SDK</span><span class="sxs-lookup"><span data-stu-id="b44ad-118">Python SDK installation instructions</span></span>](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td><span data-ttu-id="b44ad-119">**Můžete přispět k sadě SDK**</span><span class="sxs-lookup"><span data-stu-id="b44ad-119">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="b44ad-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="b44ad-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td><span data-ttu-id="b44ad-121">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="b44ad-121">**Get started**</span></span></td><td>[<span data-ttu-id="b44ad-122">Začínáme s Python SDK</span><span class="sxs-lookup"><span data-stu-id="b44ad-122">Get started with the Python SDK</span></span>](documentdb-python-application.md)</td></tr>

<tr><td><span data-ttu-id="b44ad-123">**Aktuální podporované platformy**</span><span class="sxs-lookup"><span data-stu-id="b44ad-123">**Current supported platform**</span></span></td><td><span data-ttu-id="b44ad-124">[Python 2.7](https://www.python.org/downloads/) a [Python 3.5](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="b44ad-124">[Python 2.7](https://www.python.org/downloads/) and [Python 3.5](https://www.python.org/downloads/)</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="b44ad-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="b44ad-125">Release notes</span></span>
### <a name="a-name220220"></a><span data-ttu-id="b44ad-126"><a name="2.2.0"/>2.2.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-126"><a name="2.2.0"/>2.2.0</span></span>
* <span data-ttu-id="b44ad-127">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="b44ad-127">Added support for a new consistency level called ConsistentPrefix.</span></span>


### <a name="a-name210210"></a><span data-ttu-id="b44ad-128"><a name="2.1.0"/>2.1.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-128"><a name="2.1.0"/>2.1.0</span></span>
* <span data-ttu-id="b44ad-129">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="b44ad-129">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="b44ad-130">Přidat možnost pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b44ad-130">Added an option for disabling SSL verification when running against Cosmos DB Emulator.</span></span>
* <span data-ttu-id="b44ad-131">Odebrat závislé požadavky modulu být přesně 2.10.0 omezení.</span><span class="sxs-lookup"><span data-stu-id="b44ad-131">Removed the restriction of dependent requests module to be exactly 2.10.0.</span></span>
* <span data-ttu-id="b44ad-132">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s na 2 500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="b44ad-132">Lowered minimum throughput on partitioned collections from 10,100 RU/s to 2500 RU/s.</span></span>
* <span data-ttu-id="b44ad-133">Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="b44ad-133">Added support for enabling script logging during stored procedure execution.</span></span>
* <span data-ttu-id="b44ad-134">Verze rozhraní REST API bumped k 2017-01-19' v této verzi.</span><span class="sxs-lookup"><span data-stu-id="b44ad-134">REST API version bumped to '2017-01-19' with this release.</span></span>

### <a name="a-name201201"></a><span data-ttu-id="b44ad-135"><a name="2.0.1"/>2.0.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-135"><a name="2.0.1"/>2.0.1</span></span>
* <span data-ttu-id="b44ad-136">Redakční změny provedené v dokumentační komentáře.</span><span class="sxs-lookup"><span data-stu-id="b44ad-136">Made editorial changes to documentation comments.</span></span>

### <a name="a-name200200"></a><span data-ttu-id="b44ad-137"><a name="2.0.0"/>2.0.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-137"><a name="2.0.0"/>2.0.0</span></span>
* <span data-ttu-id="b44ad-138">Přidaná podpora pro Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="b44ad-138">Added support for Python 3.5.</span></span>
* <span data-ttu-id="b44ad-139">Přidaná podpora pro sdružování připojení pomocí modulu požadavky.</span><span class="sxs-lookup"><span data-stu-id="b44ad-139">Added support for connection pooling using a requests module.</span></span>
* <span data-ttu-id="b44ad-140">Přidaná podpora pro konzistence typu relace.</span><span class="sxs-lookup"><span data-stu-id="b44ad-140">Added support for session consistency.</span></span>
* <span data-ttu-id="b44ad-141">Byla přidána podpora pro dotazy na nejvyšší nebo ORDERBY pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="b44ad-141">Added support for TOP/ORDERBY queries for partitioned collections.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="b44ad-142"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-142"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="b44ad-143">Podpora zásad přidané opakování omezenému požadavky.</span><span class="sxs-lookup"><span data-stu-id="b44ad-143">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="b44ad-144">(Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena retryAfter čas v hlavičku odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b44ad-144">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring the retryAfter time in the response header.</span></span> <span data-ttu-id="b44ad-145">Časový interval opakování pevné lze nyní nastavit jako součást RetryOptions vlastnost v objektu ConnectionPolicy Pokud budete chtít ignorovat čas retryAfter vrácená serverem mezi jednotlivými pokusy o odeslání.</span><span class="sxs-lookup"><span data-stu-id="b44ad-145">A fixed retry interval time can now be set as part of the RetryOptions property on the ConnectionPolicy object if you want to ignore the retryAfter time returned by server between the retries.</span></span> <span data-ttu-id="b44ad-146">Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď s kódem chyby 429.</span><span class="sxs-lookup"><span data-stu-id="b44ad-146">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns the response with error code 429.</span></span> <span data-ttu-id="b44ad-147">Tento čas může být také elementem ve vlastnosti RetryOptions ConnectionPolicy objektu.</span><span class="sxs-lookup"><span data-stu-id="b44ad-147">This time can also be overriden in the RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="b44ad-148">Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako opakovat hlavičky odpovědi v každé žádosti k označení omezení počtu a cummulative čas požadavku čekali mezi jednotlivými pokusy o odeslání.</span><span class="sxs-lookup"><span data-stu-id="b44ad-148">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as the response headers in every request to denote the throttle retry count and the cummulative time the request waited between the retries.</span></span>
* <span data-ttu-id="b44ad-149">Odebrat RetryPolicy třídu a vlastnost odpovídající (retry_policy) zveřejněné na třídě document_client a místo toho zavedl třídu RetryOptions vystavení vlastnost RetryOptions u ConnectionPolicy třídy, které je možné přepsat některé výchozí možnosti opakování.</span><span class="sxs-lookup"><span data-stu-id="b44ad-149">Removed the RetryPolicy class and the corresponding property (retry_policy) exposed on the document_client class and instead introduced a RetryOptions class exposing the RetryOptions property on ConnectionPolicy class that can be used to override some of the default retry options.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="b44ad-150"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-150"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="b44ad-151">Přidaná podpora pro účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="b44ad-151">Added the support for multi-region database accounts.</span></span>

### <a name="a-name170170"></a><span data-ttu-id="b44ad-152"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-152"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="b44ad-153">Přidaná podpora pro funkce čas k Live(TTL) pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b44ad-153">Added the support for Time To Live(TTL) feature for documents.</span></span>

### <a name="a-name161161"></a><span data-ttu-id="b44ad-154"><a name="1.6.1"/>1.6.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-154"><a name="1.6.1"/>1.6.1</span></span>
* <span data-ttu-id="b44ad-155">Opravy chyb související s serveru straně vytváření oddílů umožňuje speciální znaky v cestě klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="b44ad-155">Bug fixes related to server side partitioning to allow special characters in partitionkey path.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="b44ad-156"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-156"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="b44ad-157">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="b44ad-157">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span> 

### <a name="a-name150150"></a><span data-ttu-id="b44ad-158"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-158"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="b44ad-159">Přidejte hodnotu Hash p & ro oddílu překladače pomoct s aplikacemi horizontálního dělení napříč více oddílů.</span><span class="sxs-lookup"><span data-stu-id="b44ad-159">Add Hash & Range partition resolvers to assist with sharding applications across multiple partitions.</span></span>

### <a name="a-name142142"></a><span data-ttu-id="b44ad-160"><a name="1.4.2"/>1.4.2</span><span class="sxs-lookup"><span data-stu-id="b44ad-160"><a name="1.4.2"/>1.4.2</span></span>
* <span data-ttu-id="b44ad-161">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="b44ad-161">Implement Upsert.</span></span> <span data-ttu-id="b44ad-162">Přidaná kvůli podpoře funkcí Upsert nové metody UpsertXXX.</span><span class="sxs-lookup"><span data-stu-id="b44ad-162">New UpsertXXX methods added to support Upsert feature.</span></span>
* <span data-ttu-id="b44ad-163">Implementujte, na základě ID směrování.</span><span class="sxs-lookup"><span data-stu-id="b44ad-163">Implement ID Based Routing.</span></span> <span data-ttu-id="b44ad-164">Žádné změny veřejné rozhraní API, všechny změny interní.</span><span class="sxs-lookup"><span data-stu-id="b44ad-164">No public API changes, all changes internal.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="b44ad-165"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-165"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="b44ad-166">Podporuje geoprostorové index.</span><span class="sxs-lookup"><span data-stu-id="b44ad-166">Supports GeoSpatial index.</span></span>
* <span data-ttu-id="b44ad-167">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="b44ad-167">Validates id property for all resources.</span></span> <span data-ttu-id="b44ad-168">Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="b44ad-168">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="b44ad-169">Přidá nové záhlaví "index transformace průběh" ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="b44ad-169">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="b44ad-170"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-170"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="b44ad-171">Implementuje zásady indexování V2.</span><span class="sxs-lookup"><span data-stu-id="b44ad-171">Implements V2 indexing policy.</span></span>

### <a name="a-name101101"></a><span data-ttu-id="b44ad-172"><a name="1.0.1"/>1.0.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-172"><a name="1.0.1"/>1.0.1</span></span>
* <span data-ttu-id="b44ad-173">Podporuje pro připojení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="b44ad-173">Supports proxy connection.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="b44ad-174"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-174"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="b44ad-175">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="b44ad-175">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="b44ad-176">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="b44ad-176">Release & retirement dates</span></span>
<span data-ttu-id="b44ad-177">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** předem vyřazení sady SDK k funkce smooth přechodu na novější nebo podporované verzi.</span><span class="sxs-lookup"><span data-stu-id="b44ad-177">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="b44ad-178">Nové funkce a funkce a optimalizace, jsou přidány pouze v aktuální sadě SDK, jako takový je doporučujeme, aby vždy upgradu na nejnovější verze sady SDK v míře.</span><span class="sxs-lookup"><span data-stu-id="b44ad-178">New features and functionality and optimizations are only added to the current SDK, as such it is  recommend that you always upgrade to the latest SDK version as early as possible.</span></span> 

<span data-ttu-id="b44ad-179">Každá žádost o DB Cosmos pomocí vyřazeno sady SDK budou odmítnuty službou.</span><span class="sxs-lookup"><span data-stu-id="b44ad-179">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

> [!WARNING]
> <span data-ttu-id="b44ad-180">Všechny verze sady Azure DocumentDB SDK pro jazyk Python starší než verze **1.0.0** vyřadí na **29. února 2016**.</span><span class="sxs-lookup"><span data-stu-id="b44ad-180">All versions of the Azure DocumentDB SDK for Python prior to version **1.0.0** will be retired on **February 29, 2016**.</span></span> 
> 
> 

<br/>

| <span data-ttu-id="b44ad-181">Verze</span><span class="sxs-lookup"><span data-stu-id="b44ad-181">Version</span></span> | <span data-ttu-id="b44ad-182">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="b44ad-182">Release Date</span></span> | <span data-ttu-id="b44ad-183">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="b44ad-183">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="b44ad-184">2.2.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-184">2.2.0</span></span>](#2.2.0) |<span data-ttu-id="b44ad-185">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="b44ad-185">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="b44ad-186">2.1.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-186">2.1.0</span></span>](#2.1.0) |<span data-ttu-id="b44ad-187">01 může 2017</span><span class="sxs-lookup"><span data-stu-id="b44ad-187">May 01, 2017</span></span> |--- |
| [<span data-ttu-id="b44ad-188">2.0.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-188">2.0.1</span></span>](#2.0.1) |<span data-ttu-id="b44ad-189">30. října 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-189">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-190">2.0.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-190">2.0.0</span></span>](#2.0.0) |<span data-ttu-id="b44ad-191">29. září 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-191">September 29, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-192">1.9.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-192">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="b44ad-193">07 července 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-193">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-194">1.8.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-194">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="b44ad-195">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-195">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-196">1.7.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-196">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="b44ad-197">26. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-197">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-198">1.6.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-198">1.6.1</span></span>](#1.6.1) |<span data-ttu-id="b44ad-199">08. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-199">April 08, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-200">1.6.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-200">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="b44ad-201">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-201">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-202">1.5.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-202">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="b44ad-203">03 leden 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-203">January 03, 2016</span></span> |--- |
| [<span data-ttu-id="b44ad-204">1.4.2</span><span class="sxs-lookup"><span data-stu-id="b44ad-204">1.4.2</span></span>](#1.4.2) |<span data-ttu-id="b44ad-205">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-205">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="b44ad-206">1.4.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-206">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="b44ad-207">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-207">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="b44ad-208">1.2.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-208">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="b44ad-209">06 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-209">August 06, 2015</span></span> |--- |
| [<span data-ttu-id="b44ad-210">1.1.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-210">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="b44ad-211">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-211">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="b44ad-212">1.0.1</span><span class="sxs-lookup"><span data-stu-id="b44ad-212">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="b44ad-213">25 květen 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-213">May 25, 2015</span></span> |--- |
| [<span data-ttu-id="b44ad-214">1.0.0</span><span class="sxs-lookup"><span data-stu-id="b44ad-214">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="b44ad-215">07 duben 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-215">April 07, 2015</span></span> |--- |
| <span data-ttu-id="b44ad-216">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="b44ad-216">0.9.4-prelease</span></span> |<span data-ttu-id="b44ad-217">14 leden 2015</span><span class="sxs-lookup"><span data-stu-id="b44ad-217">January 14, 2015</span></span> |<span data-ttu-id="b44ad-218">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-218">February 29, 2016</span></span> |
| <span data-ttu-id="b44ad-219">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="b44ad-219">0.9.3-prelease</span></span> |<span data-ttu-id="b44ad-220">09 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="b44ad-220">December 09, 2014</span></span> |<span data-ttu-id="b44ad-221">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-221">February 29, 2016</span></span> |
| <span data-ttu-id="b44ad-222">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="b44ad-222">0.9.2-prelease</span></span> |<span data-ttu-id="b44ad-223">25 listopadu 2014</span><span class="sxs-lookup"><span data-stu-id="b44ad-223">November 25, 2014</span></span> |<span data-ttu-id="b44ad-224">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-224">February 29, 2016</span></span> |
| <span data-ttu-id="b44ad-225">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="b44ad-225">0.9.1-prelease</span></span> |<span data-ttu-id="b44ad-226">23. září 2014</span><span class="sxs-lookup"><span data-stu-id="b44ad-226">September 23, 2014</span></span> |<span data-ttu-id="b44ad-227">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-227">February 29, 2016</span></span> |
| <span data-ttu-id="b44ad-228">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="b44ad-228">0.9.0-prelease</span></span> |<span data-ttu-id="b44ad-229">21 srpen 2014</span><span class="sxs-lookup"><span data-stu-id="b44ad-229">August 21, 2014</span></span> |<span data-ttu-id="b44ad-230">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="b44ad-230">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="b44ad-231">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="b44ad-231">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="b44ad-232">Viz také</span><span class="sxs-lookup"><span data-stu-id="b44ad-232">See also</span></span>
<span data-ttu-id="b44ad-233">Další informace o Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="b44ad-233">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

