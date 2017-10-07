---
title: "aaaAzure Cosmos DB Python rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello rozhraní API jazyka Python a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB Python SDK."
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
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a><span data-ttu-id="9720a-103">Azure Python Cosmos DB SDK: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="9720a-103">Azure Cosmos DB Python SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9720a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="9720a-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="9720a-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9720a-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="9720a-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="9720a-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="9720a-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="9720a-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="9720a-108">Java</span><span class="sxs-lookup"><span data-stu-id="9720a-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="9720a-109">Python</span><span class="sxs-lookup"><span data-stu-id="9720a-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="9720a-110">REST</span><span class="sxs-lookup"><span data-stu-id="9720a-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="9720a-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="9720a-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="9720a-112">SQL</span><span class="sxs-lookup"><span data-stu-id="9720a-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="9720a-113">**Stáhněte si sadu SDK**</span><span class="sxs-lookup"><span data-stu-id="9720a-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="9720a-114">Úložiště PyPI</span><span class="sxs-lookup"><span data-stu-id="9720a-114">PyPI</span></span>](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td><span data-ttu-id="9720a-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="9720a-115">**API documentation**</span></span></td><td>[<span data-ttu-id="9720a-116">Referenční dokumentace rozhraní API jazyka Python</span><span class="sxs-lookup"><span data-stu-id="9720a-116">Python API reference documentation</span></span>](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td><span data-ttu-id="9720a-117">**Pokyny k instalaci sady SDK**</span><span class="sxs-lookup"><span data-stu-id="9720a-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="9720a-118">Pokyny k instalaci Python SDK</span><span class="sxs-lookup"><span data-stu-id="9720a-118">Python SDK installation instructions</span></span>](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td><span data-ttu-id="9720a-119">**Přispívat tooSDK**</span><span class="sxs-lookup"><span data-stu-id="9720a-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="9720a-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="9720a-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td><span data-ttu-id="9720a-121">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="9720a-121">**Get started**</span></span></td><td>[<span data-ttu-id="9720a-122">Začínáme s hello Python SDK</span><span class="sxs-lookup"><span data-stu-id="9720a-122">Get started with hello Python SDK</span></span>](documentdb-python-application.md)</td></tr>

<tr><td><span data-ttu-id="9720a-123">**Aktuální podporované platformy**</span><span class="sxs-lookup"><span data-stu-id="9720a-123">**Current supported platform**</span></span></td><td><span data-ttu-id="9720a-124">[Python 2.7](https://www.python.org/downloads/) a [Python 3.5](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="9720a-124">[Python 2.7](https://www.python.org/downloads/) and [Python 3.5](https://www.python.org/downloads/)</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="9720a-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="9720a-125">Release notes</span></span>
### <a name="a-name220220"></a><span data-ttu-id="9720a-126"><a name="2.2.0"/>2.2.0</span><span class="sxs-lookup"><span data-stu-id="9720a-126"><a name="2.2.0"/>2.2.0</span></span>
* <span data-ttu-id="9720a-127">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="9720a-127">Added support for a new consistency level called ConsistentPrefix.</span></span>


### <a name="a-name210210"></a><span data-ttu-id="9720a-128"><a name="2.1.0"/>2.1.0</span><span class="sxs-lookup"><span data-stu-id="9720a-128"><a name="2.1.0"/>2.1.0</span></span>
* <span data-ttu-id="9720a-129">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="9720a-129">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="9720a-130">Přidat možnost pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="9720a-130">Added an option for disabling SSL verification when running against Cosmos DB Emulator.</span></span>
* <span data-ttu-id="9720a-131">Odebrat závislé požadavky modulu toobe přesně 2.10.0 hello omezením.</span><span class="sxs-lookup"><span data-stu-id="9720a-131">Removed hello restriction of dependent requests module toobe exactly 2.10.0.</span></span>
* <span data-ttu-id="9720a-132">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="9720a-132">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="9720a-133">Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="9720a-133">Added support for enabling script logging during stored procedure execution.</span></span>
* <span data-ttu-id="9720a-134">Verze rozhraní REST API bumped příliš ' 2017-01-19' v této verzi.</span><span class="sxs-lookup"><span data-stu-id="9720a-134">REST API version bumped too'2017-01-19' with this release.</span></span>

### <a name="a-name201201"></a><span data-ttu-id="9720a-135"><a name="2.0.1"/>2.0.1</span><span class="sxs-lookup"><span data-stu-id="9720a-135"><a name="2.0.1"/>2.0.1</span></span>
* <span data-ttu-id="9720a-136">Redakční změny provedené toodocumentation komentáře.</span><span class="sxs-lookup"><span data-stu-id="9720a-136">Made editorial changes toodocumentation comments.</span></span>

### <a name="a-name200200"></a><span data-ttu-id="9720a-137"><a name="2.0.0"/>2.0.0</span><span class="sxs-lookup"><span data-stu-id="9720a-137"><a name="2.0.0"/>2.0.0</span></span>
* <span data-ttu-id="9720a-138">Přidaná podpora pro Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="9720a-138">Added support for Python 3.5.</span></span>
* <span data-ttu-id="9720a-139">Přidaná podpora pro sdružování připojení pomocí modulu požadavky.</span><span class="sxs-lookup"><span data-stu-id="9720a-139">Added support for connection pooling using a requests module.</span></span>
* <span data-ttu-id="9720a-140">Přidaná podpora pro konzistence typu relace.</span><span class="sxs-lookup"><span data-stu-id="9720a-140">Added support for session consistency.</span></span>
* <span data-ttu-id="9720a-141">Byla přidána podpora pro dotazy na nejvyšší nebo ORDERBY pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="9720a-141">Added support for TOP/ORDERBY queries for partitioned collections.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="9720a-142"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="9720a-142"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="9720a-143">Podpora zásad přidané opakování omezenému požadavky.</span><span class="sxs-lookup"><span data-stu-id="9720a-143">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="9720a-144">(Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena hello retryAfter čas v hlavičku odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="9720a-144">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="9720a-145">Časový interval opakování pevné lze nyní nastavit jako součást hello RetryOptions vlastnost u objektu ConnectionPolicy hello Pokud chcete, aby tooignore hello retryAfter čas vrácená serverem mezi opakovanými pokusy hello.</span><span class="sxs-lookup"><span data-stu-id="9720a-145">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="9720a-146">Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď hello s kódem chyby 429.</span><span class="sxs-lookup"><span data-stu-id="9720a-146">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="9720a-147">Tento čas může být také elementem v hello RetryOptions vlastnost ConnectionPolicy objektu.</span><span class="sxs-lookup"><span data-stu-id="9720a-147">This time can also be overriden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="9720a-148">Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako hello hlavičky odpovědi v každé žádosti toodenote hello omezení počtu a hello cummulative čas požadavku hello čekali mezi opakovanými pokusy hello zkuste provést znovu.</span><span class="sxs-lookup"><span data-stu-id="9720a-148">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cummulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="9720a-149">Odebrané hello RetryPolicy třídy a odpovídající vlastnosti hello (retry_policy) vystavený pro třídu document_client hello a místo toho zavedl třídu RetryOptions vystavení hello RetryOptions vlastnost ConnectionPolicy třídu, která lze použít toooverride Některé hello výchozí možnosti opakování.</span><span class="sxs-lookup"><span data-stu-id="9720a-149">Removed hello RetryPolicy class and hello corresponding property (retry_policy) exposed on hello document_client class and instead introduced a RetryOptions class exposing hello RetryOptions property on ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="9720a-150"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="9720a-150"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="9720a-151">Podpora přidání hello účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="9720a-151">Added hello support for multi-region database accounts.</span></span>

### <a name="a-name170170"></a><span data-ttu-id="9720a-152"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="9720a-152"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="9720a-153">Přidání hello podporu pro funkci tooLive(TTL) čas pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="9720a-153">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <a name="a-name161161"></a><span data-ttu-id="9720a-154"><a name="1.6.1"/>1.6.1</span><span class="sxs-lookup"><span data-stu-id="9720a-154"><a name="1.6.1"/>1.6.1</span></span>
* <span data-ttu-id="9720a-155">Opravy chyb souvisejících s tooserver straně dělení tooallow speciální znaky v cestě klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="9720a-155">Bug fixes related tooserver side partitioning tooallow special characters in partitionkey path.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="9720a-156"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="9720a-156"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="9720a-157">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="9720a-157">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span> 

### <a name="a-name150150"></a><span data-ttu-id="9720a-158"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="9720a-158"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="9720a-159">Přidejte hodnotu Hash p & ro tooassist překladače oddílu s horizontálního dělení aplikacemi v rámci více oddílů.</span><span class="sxs-lookup"><span data-stu-id="9720a-159">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name142142"></a><span data-ttu-id="9720a-160"><a name="1.4.2"/>1.4.2</span><span class="sxs-lookup"><span data-stu-id="9720a-160"><a name="1.4.2"/>1.4.2</span></span>
* <span data-ttu-id="9720a-161">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="9720a-161">Implement Upsert.</span></span> <span data-ttu-id="9720a-162">Přidat nové metody UpsertXXX toosupport Upsert funkce.</span><span class="sxs-lookup"><span data-stu-id="9720a-162">New UpsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="9720a-163">Implementujte, na základě ID směrování.</span><span class="sxs-lookup"><span data-stu-id="9720a-163">Implement ID Based Routing.</span></span> <span data-ttu-id="9720a-164">Žádné změny veřejné rozhraní API, všechny změny interní.</span><span class="sxs-lookup"><span data-stu-id="9720a-164">No public API changes, all changes internal.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="9720a-165"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="9720a-165"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="9720a-166">Podporuje geoprostorové index.</span><span class="sxs-lookup"><span data-stu-id="9720a-166">Supports GeoSpatial index.</span></span>
* <span data-ttu-id="9720a-167">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9720a-167">Validates id property for all resources.</span></span> <span data-ttu-id="9720a-168">Identifikátory prostředků nesmí obsahovat?, /, #, \, znaků nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="9720a-168">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="9720a-169">Přidá nový tooResourceResponse "průběh transformace index" záhlaví.</span><span class="sxs-lookup"><span data-stu-id="9720a-169">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="9720a-170"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="9720a-170"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="9720a-171">Implementuje zásady indexování V2.</span><span class="sxs-lookup"><span data-stu-id="9720a-171">Implements V2 indexing policy.</span></span>

### <a name="a-name101101"></a><span data-ttu-id="9720a-172"><a name="1.0.1"/>1.0.1</span><span class="sxs-lookup"><span data-stu-id="9720a-172"><a name="1.0.1"/>1.0.1</span></span>
* <span data-ttu-id="9720a-173">Podporuje pro připojení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="9720a-173">Supports proxy connection.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="9720a-174"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="9720a-174"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="9720a-175">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="9720a-175">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="9720a-176">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="9720a-176">Release & retirement dates</span></span>
<span data-ttu-id="9720a-177">Microsoft bude poskytovat oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="9720a-177">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="9720a-178">Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK a jako takový je, že jste vždy upgradu toohello nejnovější verze sady SDK, abyste co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="9720a-178">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="9720a-179">Všechny žádosti o tooCosmos DB pomocí vyřazeno sady SDK budou odmítnuty službou hello.</span><span class="sxs-lookup"><span data-stu-id="9720a-179">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="9720a-180">Všechny verze hello Azure DocumentDB SDK pro Python předchozí tooversion **1.0.0** vyřadí na **29. února 2016**.</span><span class="sxs-lookup"><span data-stu-id="9720a-180">All versions of hello Azure DocumentDB SDK for Python prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span> 
> 
> 

<br/>

| <span data-ttu-id="9720a-181">Verze</span><span class="sxs-lookup"><span data-stu-id="9720a-181">Version</span></span> | <span data-ttu-id="9720a-182">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="9720a-182">Release Date</span></span> | <span data-ttu-id="9720a-183">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="9720a-183">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="9720a-184">2.2.0</span><span class="sxs-lookup"><span data-stu-id="9720a-184">2.2.0</span></span>](#2.2.0) |<span data-ttu-id="9720a-185">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="9720a-185">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="9720a-186">2.1.0</span><span class="sxs-lookup"><span data-stu-id="9720a-186">2.1.0</span></span>](#2.1.0) |<span data-ttu-id="9720a-187">01 může 2017</span><span class="sxs-lookup"><span data-stu-id="9720a-187">May 01, 2017</span></span> |--- |
| [<span data-ttu-id="9720a-188">2.0.1</span><span class="sxs-lookup"><span data-stu-id="9720a-188">2.0.1</span></span>](#2.0.1) |<span data-ttu-id="9720a-189">30. října 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-189">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-190">2.0.0</span><span class="sxs-lookup"><span data-stu-id="9720a-190">2.0.0</span></span>](#2.0.0) |<span data-ttu-id="9720a-191">29. září 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-191">September 29, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-192">1.9.0</span><span class="sxs-lookup"><span data-stu-id="9720a-192">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="9720a-193">07 července 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-193">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-194">1.8.0</span><span class="sxs-lookup"><span data-stu-id="9720a-194">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="9720a-195">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-195">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-196">1.7.0</span><span class="sxs-lookup"><span data-stu-id="9720a-196">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="9720a-197">26. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-197">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-198">1.6.1</span><span class="sxs-lookup"><span data-stu-id="9720a-198">1.6.1</span></span>](#1.6.1) |<span data-ttu-id="9720a-199">08. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-199">April 08, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-200">1.6.0</span><span class="sxs-lookup"><span data-stu-id="9720a-200">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="9720a-201">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-201">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-202">1.5.0</span><span class="sxs-lookup"><span data-stu-id="9720a-202">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="9720a-203">03 leden 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-203">January 03, 2016</span></span> |--- |
| [<span data-ttu-id="9720a-204">1.4.2</span><span class="sxs-lookup"><span data-stu-id="9720a-204">1.4.2</span></span>](#1.4.2) |<span data-ttu-id="9720a-205">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-205">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="9720a-206">1.4.1</span><span class="sxs-lookup"><span data-stu-id="9720a-206">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="9720a-207">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-207">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="9720a-208">1.2.0</span><span class="sxs-lookup"><span data-stu-id="9720a-208">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="9720a-209">06 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-209">August 06, 2015</span></span> |--- |
| [<span data-ttu-id="9720a-210">1.1.0</span><span class="sxs-lookup"><span data-stu-id="9720a-210">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="9720a-211">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-211">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="9720a-212">1.0.1</span><span class="sxs-lookup"><span data-stu-id="9720a-212">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="9720a-213">25 květen 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-213">May 25, 2015</span></span> |--- |
| [<span data-ttu-id="9720a-214">1.0.0</span><span class="sxs-lookup"><span data-stu-id="9720a-214">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="9720a-215">07 duben 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-215">April 07, 2015</span></span> |--- |
| <span data-ttu-id="9720a-216">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="9720a-216">0.9.4-prelease</span></span> |<span data-ttu-id="9720a-217">14 leden 2015</span><span class="sxs-lookup"><span data-stu-id="9720a-217">January 14, 2015</span></span> |<span data-ttu-id="9720a-218">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-218">February 29, 2016</span></span> |
| <span data-ttu-id="9720a-219">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="9720a-219">0.9.3-prelease</span></span> |<span data-ttu-id="9720a-220">09 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="9720a-220">December 09, 2014</span></span> |<span data-ttu-id="9720a-221">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-221">February 29, 2016</span></span> |
| <span data-ttu-id="9720a-222">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="9720a-222">0.9.2-prelease</span></span> |<span data-ttu-id="9720a-223">25 listopadu 2014</span><span class="sxs-lookup"><span data-stu-id="9720a-223">November 25, 2014</span></span> |<span data-ttu-id="9720a-224">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-224">February 29, 2016</span></span> |
| <span data-ttu-id="9720a-225">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="9720a-225">0.9.1-prelease</span></span> |<span data-ttu-id="9720a-226">23. září 2014</span><span class="sxs-lookup"><span data-stu-id="9720a-226">September 23, 2014</span></span> |<span data-ttu-id="9720a-227">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-227">February 29, 2016</span></span> |
| <span data-ttu-id="9720a-228">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="9720a-228">0.9.0-prelease</span></span> |<span data-ttu-id="9720a-229">21 srpen 2014</span><span class="sxs-lookup"><span data-stu-id="9720a-229">August 21, 2014</span></span> |<span data-ttu-id="9720a-230">29. února 2016</span><span class="sxs-lookup"><span data-stu-id="9720a-230">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="9720a-231">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="9720a-231">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="9720a-232">Viz také</span><span class="sxs-lookup"><span data-stu-id="9720a-232">See also</span></span>
<span data-ttu-id="9720a-233">toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="9720a-233">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

