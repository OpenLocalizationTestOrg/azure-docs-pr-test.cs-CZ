---
title: "aaaAzure Cosmos DB Node.js API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello rozhraní API Node.js a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB Node.js SDK."
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d450b9a9ea7b0f4717ddae8940121fc458ea3744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a><span data-ttu-id="3503a-103">Azure SDK Cosmos DB Node.js: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="3503a-103">Azure Cosmos DB Node.js SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3503a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="3503a-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="3503a-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3503a-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="3503a-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="3503a-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="3503a-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="3503a-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="3503a-108">Java</span><span class="sxs-lookup"><span data-stu-id="3503a-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="3503a-109">Python</span><span class="sxs-lookup"><span data-stu-id="3503a-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="3503a-110">REST</span><span class="sxs-lookup"><span data-stu-id="3503a-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="3503a-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="3503a-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="3503a-112">SQL</span><span class="sxs-lookup"><span data-stu-id="3503a-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="3503a-113">**Stáhněte si sadu SDK**</span><span class="sxs-lookup"><span data-stu-id="3503a-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="3503a-114">NPM</span><span class="sxs-lookup"><span data-stu-id="3503a-114">NPM</span></span>](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td><span data-ttu-id="3503a-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="3503a-115">**API documentation**</span></span></td><td>[<span data-ttu-id="3503a-116">Referenční dokumentace rozhraní API Node.js</span><span class="sxs-lookup"><span data-stu-id="3503a-116">Node.js API reference documentation</span></span>](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td><span data-ttu-id="3503a-117">**Pokyny k instalaci sady SDK**</span><span class="sxs-lookup"><span data-stu-id="3503a-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="3503a-118">Pokyny k instalaci</span><span class="sxs-lookup"><span data-stu-id="3503a-118">Installation instructions</span></span>](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td><span data-ttu-id="3503a-119">**Přispívat tooSDK**</span><span class="sxs-lookup"><span data-stu-id="3503a-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="3503a-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="3503a-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td><span data-ttu-id="3503a-121">**Ukázky**</span><span class="sxs-lookup"><span data-stu-id="3503a-121">**Samples**</span></span></td><td>[<span data-ttu-id="3503a-122">Ukázky kódu Node.js</span><span class="sxs-lookup"><span data-stu-id="3503a-122">Node.js code samples</span></span>](documentdb-nodejs-samples.md)</td></tr>

<tr><td><span data-ttu-id="3503a-123">**Kurz Začínáme**</span><span class="sxs-lookup"><span data-stu-id="3503a-123">**Get started tutorial**</span></span></td><td>[<span data-ttu-id="3503a-124">Začínáme s hello Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="3503a-124">Get started with hello Node.js SDK</span></span>](documentdb-nodejs-get-started.md)</td></tr>

<tr><td><span data-ttu-id="3503a-125">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="3503a-125">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="3503a-126">Vytvoření webové aplikace Node.js pomocí Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3503a-126">Build a Node.js web application using Azure Cosmos DB</span></span>](documentdb-nodejs-application.md)</td></tr>

<tr><td><span data-ttu-id="3503a-127">**Aktuální podporované platformy**</span><span class="sxs-lookup"><span data-stu-id="3503a-127">**Current supported platform**</span></span></td><td> 
[<span data-ttu-id="3503a-128">Verze 6.x Node.js</span><span class="sxs-lookup"><span data-stu-id="3503a-128">Node.js v6.x</span></span>](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[<span data-ttu-id="3503a-129">Node.js v4.2.0</span><span class="sxs-lookup"><span data-stu-id="3503a-129">Node.js v4.2.0</span></span>](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[<span data-ttu-id="3503a-130">Node.js v0.12</span><span class="sxs-lookup"><span data-stu-id="3503a-130">Node.js v0.12</span></span>](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
<span data-ttu-id="3503a-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span><span class="sxs-lookup"><span data-stu-id="3503a-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="3503a-132">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="3503a-132">Release notes</span></span>

### <span data-ttu-id="3503a-133"><a name="1.12.2"/>1.12.2</a></span><span class="sxs-lookup"><span data-stu-id="3503a-133"><a name="1.12.2"/>1.12.2</a></span></span>
*   <span data-ttu-id="3503a-134">dokumentace npm pevné.</span><span class="sxs-lookup"><span data-stu-id="3503a-134">npm documentation fixed.</span></span>

### <span data-ttu-id="3503a-135"><a name="1.12.1"/>1.12.1</a></span><span class="sxs-lookup"><span data-stu-id="3503a-135"><a name="1.12.1"/>1.12.1</a></span></span>
* <span data-ttu-id="3503a-136">Pevná chyby ve executeStoredProcedure kde dokumentů, které měl speciální znaky znakové sady Unicode (LS, PS).</span><span class="sxs-lookup"><span data-stu-id="3503a-136">Fixed a bug in executeStoredProcedure where documents involved had special Unicode characters (LS, PS).</span></span>
* <span data-ttu-id="3503a-137">Opravit chyby při zpracování dokumentů s znaky kódování Unicode v hello klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="3503a-137">Fixed a bug in handling documents with Unicode characters in hello partition key.</span></span>
* <span data-ttu-id="3503a-138">Opravené podpora pro vytváření kolekcí s hello název média.</span><span class="sxs-lookup"><span data-stu-id="3503a-138">Fixed support for creating collections with hello name media.</span></span> <span data-ttu-id="3503a-139">Problém Githubu #114.</span><span class="sxs-lookup"><span data-stu-id="3503a-139">Github issue #114.</span></span>
* <span data-ttu-id="3503a-140">Opravené podpora oprávnění autorizační token.</span><span class="sxs-lookup"><span data-stu-id="3503a-140">Fixed support for permission authorization token.</span></span> <span data-ttu-id="3503a-141">Problém Githubu #178.</span><span class="sxs-lookup"><span data-stu-id="3503a-141">Github issue #178.</span></span>

### <span data-ttu-id="3503a-142"><a name="1.12.0"/>1.12.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-142"><a name="1.12.0"/>1.12.0</a></span></span>
* <span data-ttu-id="3503a-143">Přidaná podpora pro novou [úroveň konzistence](consistency-levels.md) názvem ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="3503a-143">Added support for a new [consistency level](consistency-levels.md) called ConsistentPrefix.</span></span>
* <span data-ttu-id="3503a-144">Přidaná podpora pro UriFactory.</span><span class="sxs-lookup"><span data-stu-id="3503a-144">Added support for UriFactory.</span></span>
* <span data-ttu-id="3503a-145">Opravit chyby Podpora kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="3503a-145">Fixed a Unicode support bug.</span></span> <span data-ttu-id="3503a-146">#171 potíže Githubu.</span><span class="sxs-lookup"><span data-stu-id="3503a-146">GitHub issue #171.</span></span>

### <span data-ttu-id="3503a-147"><a name="1.11.0"/>1.11.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-147"><a name="1.11.0"/>1.11.0</a></span></span>
* <span data-ttu-id="3503a-148">Podpora přidání hello dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="3503a-148">Added hello support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="3503a-149">Přidání hello možnost řízení stupně paralelního zpracování pro různé dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="3503a-149">Added hello option for controlling degree of parallelism for cross partition queries.</span></span>
* <span data-ttu-id="3503a-150">Přidání hello možnost pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="3503a-150">Added hello option for disabling SSL verification when running against Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="3503a-151">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="3503a-151">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="3503a-152">Chyby tokenu pevné hello pokračování pro kolekce tvořené jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="3503a-152">Fixed hello continuation token bug for single partition collection.</span></span> <span data-ttu-id="3503a-153">Problém Githubu #107.</span><span class="sxs-lookup"><span data-stu-id="3503a-153">Github issue #107.</span></span>
* <span data-ttu-id="3503a-154">Chyby opravené hello executeStoredProcedure při zpracování 0 jako jeden param.</span><span class="sxs-lookup"><span data-stu-id="3503a-154">Fixed hello executeStoredProcedure bug in handling 0 as single param.</span></span> <span data-ttu-id="3503a-155">Problém Githubu #155.</span><span class="sxs-lookup"><span data-stu-id="3503a-155">Github issue #155.</span></span>

### <span data-ttu-id="3503a-156"><a name="1.10.2"/>1.10.2</a></span><span class="sxs-lookup"><span data-stu-id="3503a-156"><a name="1.10.2"/>1.10.2</a></span></span>
* <span data-ttu-id="3503a-157">Opravené identifikační hlavička tooinclude hello SDK verze.</span><span class="sxs-lookup"><span data-stu-id="3503a-157">Fixed user-agent header tooinclude hello SDK version.</span></span>
* <span data-ttu-id="3503a-158">Méně závažné kód čištění.</span><span class="sxs-lookup"><span data-stu-id="3503a-158">Minor code cleanup.</span></span>

### <span data-ttu-id="3503a-159"><a name="1.10.1"/>1.10.1</a></span><span class="sxs-lookup"><span data-stu-id="3503a-159"><a name="1.10.1"/>1.10.1</a></span></span>
* <span data-ttu-id="3503a-160">Při použití hello SDK tootarget hello emulator(hostname=localhost) se zakazuje ověření protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="3503a-160">Disabling SSL verification when using hello SDK tootarget hello emulator(hostname=localhost).</span></span>
* <span data-ttu-id="3503a-161">Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3503a-161">Added support for enabling script logging during stored procedure execution.</span></span>

### <span data-ttu-id="3503a-162"><a name="1.10.0"/>1.10.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-162"><a name="1.10.0"/>1.10.0</a></span></span>
* <span data-ttu-id="3503a-163">Přidaná podpora pro křížové paralelní dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="3503a-163">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="3503a-164">Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="3503a-164">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>

### <span data-ttu-id="3503a-165"><a name="1.9.0"/>1.9.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-165"><a name="1.9.0"/>1.9.0</a></span></span>
* <span data-ttu-id="3503a-166">Podpora zásad přidané opakování omezenému požadavky.</span><span class="sxs-lookup"><span data-stu-id="3503a-166">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="3503a-167">(Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena hello retryAfter čas v hlavičku odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="3503a-167">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="3503a-168">Časový interval opakování pevné lze nyní nastavit jako součást hello RetryOptions vlastnost u objektu ConnectionPolicy hello Pokud chcete, aby tooignore hello retryAfter čas vrácená serverem mezi opakovanými pokusy hello.</span><span class="sxs-lookup"><span data-stu-id="3503a-168">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="3503a-169">Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď hello s kódem chyby 429.</span><span class="sxs-lookup"><span data-stu-id="3503a-169">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="3503a-170">Tentokrát lze také ve hello RetryOptions vlastnosti v objektu ConnectionPolicy přepsat.</span><span class="sxs-lookup"><span data-stu-id="3503a-170">This time can also be overridden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="3503a-171">Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako hello hlavičky odpovědi v každé žádosti toodenote hello omezení počtu a hello kumulativní čas požadavku hello čekali mezi opakovanými pokusy hello zkuste provést znovu.</span><span class="sxs-lookup"><span data-stu-id="3503a-171">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cumulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="3503a-172">byla přidána Hello RetryOptions třída vystavení hello RetryOptions vlastnost u hello ConnectionPolicy třídy, které můžou být použité toooverride některé hello výchozí možnosti opakování.</span><span class="sxs-lookup"><span data-stu-id="3503a-172">hello RetryOptions class was added, exposing hello RetryOptions property on hello ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <span data-ttu-id="3503a-173"><a name="1.8.0"/>1.8.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-173"><a name="1.8.0"/>1.8.0</a></span></span>
* <span data-ttu-id="3503a-174">Podpora přidání hello účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="3503a-174">Added hello support for multi-region database accounts.</span></span>

### <span data-ttu-id="3503a-175"><a name="1.7.0"/>1.7.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-175"><a name="1.7.0"/>1.7.0</a></span></span>
* <span data-ttu-id="3503a-176">Přidání hello podporu pro funkci tooLive(TTL) čas pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="3503a-176">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <span data-ttu-id="3503a-177"><a name="1.6.0"/>1.6.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-177"><a name="1.6.0"/>1.6.0</a></span></span>
* <span data-ttu-id="3503a-178">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="3503a-178">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <span data-ttu-id="3503a-179"><a name="1.5.6"/>1.5.6</a></span><span class="sxs-lookup"><span data-stu-id="3503a-179"><a name="1.5.6"/>1.5.6</a></span></span>
* <span data-ttu-id="3503a-180">Opravené chyby RangePartitionResolver.resolveForRead, kde ji nebyl vrácení odkazy z důvodu chybné concat tooa výsledků.</span><span class="sxs-lookup"><span data-stu-id="3503a-180">Fixed RangePartitionResolver.resolveForRead bug where it was not returning links due tooa bad concat of results.</span></span>

### <span data-ttu-id="3503a-181"><a name="1.5.5"/>1.5.5</a></span><span class="sxs-lookup"><span data-stu-id="3503a-181"><a name="1.5.5"/>1.5.5</a></span></span>
* <span data-ttu-id="3503a-182">Pevné hashParitionResolver resolveForRead(): žádné předaný klíč oddílu byla při vyvolání výjimky, místo vrací seznam všech registrovaných odkazů.</span><span class="sxs-lookup"><span data-stu-id="3503a-182">Fixed hashParitionResolver resolveForRead(): When no partition key supplied was throwing exception, instead of returning a list of all registered links.</span></span>

### <span data-ttu-id="3503a-183"><a name="1.5.4"/>1.5.4</a></span><span class="sxs-lookup"><span data-stu-id="3503a-183"><a name="1.5.4"/>1.5.4</a></span></span>
* <span data-ttu-id="3503a-184">Řeší problém [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -vyhrazené agenta HTTPS: neměli upravovat hello globální agenta pro účely Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3503a-184">Fixes issue [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Dedicated HTTPS Agent: Avoid modifying hello global agent for Azure Cosmos DB purposes.</span></span> <span data-ttu-id="3503a-185">Použijte vyhrazenou agenta pro všechny požadavky hello lib.</span><span class="sxs-lookup"><span data-stu-id="3503a-185">Use a dedicated agent for all of hello lib’s requests.</span></span>

### <span data-ttu-id="3503a-186"><a name="1.5.3"/>1.5.3</a></span><span class="sxs-lookup"><span data-stu-id="3503a-186"><a name="1.5.3"/>1.5.3</a></span></span>
* <span data-ttu-id="3503a-187">Řeší problém [#81](https://github.com/Azure/azure-documentdb-node/issues/81) – správně zpracovat pomlčky v ID média.</span><span class="sxs-lookup"><span data-stu-id="3503a-187">Fixes issue [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - Properly handle dashes in media ids.</span></span>

### <span data-ttu-id="3503a-188"><a name="1.5.2"/>1.5.2</a></span><span class="sxs-lookup"><span data-stu-id="3503a-188"><a name="1.5.2"/>1.5.2</a></span></span>
* <span data-ttu-id="3503a-189">Řeší problém [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -naslouchací proces EventEmitter úniku upozornění.</span><span class="sxs-lookup"><span data-stu-id="3503a-189">Fixes issue [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter listener leak warning.</span></span>

### <span data-ttu-id="3503a-190"><a name="1.5.1"/>1.5.1</a></span><span class="sxs-lookup"><span data-stu-id="3503a-190"><a name="1.5.1"/>1.5.1</a></span></span>
* <span data-ttu-id="3503a-191">Řeší problém [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -přejmenovat složku Hash toohash systémů malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3503a-191">Fixes issue [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - rename folder Hash toohash for case-sensitive systems.</span></span>

### <span data-ttu-id="3503a-192"><a name="1.5.0"/>1.5.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-192"><a name="1.5.0"/>1.5.0</a></span></span>
* <span data-ttu-id="3503a-193">Podpora horizontálního dělení implementace přidáním hash p & ro překladače oddílu.</span><span class="sxs-lookup"><span data-stu-id="3503a-193">Implement sharding support by adding hash & range partition resolvers.</span></span>

### <span data-ttu-id="3503a-194"><a name="1.4.0"/>1.4.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-194"><a name="1.4.0"/>1.4.0</a></span></span>
* <span data-ttu-id="3503a-195">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="3503a-195">Implement Upsert.</span></span> <span data-ttu-id="3503a-196">Nové metody upsertXXX na documentClient.</span><span class="sxs-lookup"><span data-stu-id="3503a-196">New upsertXXX methods on documentClient.</span></span>

### <span data-ttu-id="3503a-197"><a name="1.3.0"/>1.3.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-197"><a name="1.3.0"/>1.3.0</a></span></span>
* <span data-ttu-id="3503a-198">Přeskočené toobring čísla verze v zarovnání s dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="3503a-198">Skipped toobring version numbers in alignment with other SDKs.</span></span>

### <span data-ttu-id="3503a-199"><a name="1.2.2"/>1.2.2</a></span><span class="sxs-lookup"><span data-stu-id="3503a-199"><a name="1.2.2"/>1.2.2</a></span></span>
* <span data-ttu-id="3503a-200">Rozdělení Q nabízí obálku toonew úložiště.</span><span class="sxs-lookup"><span data-stu-id="3503a-200">Split Q promises wrapper toonew repository.</span></span>
* <span data-ttu-id="3503a-201">Aktualizujte soubor toopackage pro npm registru.</span><span class="sxs-lookup"><span data-stu-id="3503a-201">Update toopackage file for npm registry.</span></span>

### <span data-ttu-id="3503a-202"><a name="1.2.1"/>1.2.1</a></span><span class="sxs-lookup"><span data-stu-id="3503a-202"><a name="1.2.1"/>1.2.1</a></span></span>
* <span data-ttu-id="3503a-203">Implementuje ID založené na směrování.</span><span class="sxs-lookup"><span data-stu-id="3503a-203">Implements ID Based Routing.</span></span>
* <span data-ttu-id="3503a-204">Řeší problém [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuální vlastnost je v konfliktu s má objekt current() metoda.</span><span class="sxs-lookup"><span data-stu-id="3503a-204">Fixes Issue [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - current property conflicts with method current().</span></span>

### <span data-ttu-id="3503a-205"><a name="1.2.0"/>1.2.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-205"><a name="1.2.0"/>1.2.0</a></span></span>
* <span data-ttu-id="3503a-206">Byla přidána podpora pro geoprostorové index.</span><span class="sxs-lookup"><span data-stu-id="3503a-206">Added support for GeoSpatial index.</span></span>
* <span data-ttu-id="3503a-207">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3503a-207">Validates id property for all resources.</span></span> <span data-ttu-id="3503a-208">Identifikátory prostředků nesmí obsahovat?, /, # &#47; &#47; znaky nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="3503a-208">Ids for resources cannot contain ?, /, #, &#47;&#47;, characters or end with a space.</span></span>
* <span data-ttu-id="3503a-209">Přidá nový tooResourceResponse "průběh transformace index" záhlaví.</span><span class="sxs-lookup"><span data-stu-id="3503a-209">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <span data-ttu-id="3503a-210"><a name="1.1.0"/>1.1.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-210"><a name="1.1.0"/>1.1.0</a></span></span>
* <span data-ttu-id="3503a-211">Implementuje zásady indexování V2.</span><span class="sxs-lookup"><span data-stu-id="3503a-211">Implements V2 indexing policy.</span></span>

### <span data-ttu-id="3503a-212"><a name="1.0.3"/>1.0.3</a></span><span class="sxs-lookup"><span data-stu-id="3503a-212"><a name="1.0.3"/>1.0.3</a></span></span>
* <span data-ttu-id="3503a-213">Problém [#40](https://github.com/Azure/azure-documentdb-node/issues/40) – implementována eslint grunt konfigurace v hello jádra a promise SDK.</span><span class="sxs-lookup"><span data-stu-id="3503a-213">Issue [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Implemented eslint and grunt configurations in hello core and promise SDK.</span></span>

### <span data-ttu-id="3503a-214"><a name="1.0.2"/>1.0.2</a></span><span class="sxs-lookup"><span data-stu-id="3503a-214"><a name="1.0.2"/>1.0.2</a></span></span>
* <span data-ttu-id="3503a-215">Problém [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -lišící obálku nezahrnuje hlavičky s chybou.</span><span class="sxs-lookup"><span data-stu-id="3503a-215">Issue [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises wrapper does not include header with error.</span></span>

### <span data-ttu-id="3503a-216"><a name="1.0.1"/>1.0.1</a></span><span class="sxs-lookup"><span data-stu-id="3503a-216"><a name="1.0.1"/>1.0.1</a></span></span>
* <span data-ttu-id="3503a-217">Přidání readConflicts, readConflictAsync a queryConflicts implementují možnost tooquery konflikty.</span><span class="sxs-lookup"><span data-stu-id="3503a-217">Implemented ability tooquery for conflicts by adding readConflicts, readConflictAsync, and queryConflicts.</span></span>
* <span data-ttu-id="3503a-218">Aktualizované dokumentace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3503a-218">Updated API documentation.</span></span>
* <span data-ttu-id="3503a-219">Problém [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync chyby.</span><span class="sxs-lookup"><span data-stu-id="3503a-219">Issue [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync error.</span></span>

### <span data-ttu-id="3503a-220"><a name="1.0.0"/>1.0.0</a></span><span class="sxs-lookup"><span data-stu-id="3503a-220"><a name="1.0.0"/>1.0.0</a></span></span>
* <span data-ttu-id="3503a-221">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="3503a-221">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="3503a-222">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="3503a-222">Release & Retirement Dates</span></span>
<span data-ttu-id="3503a-223">Společnost Microsoft poskytuje oznámení alespoň **dobu 12 měsíců** před vyřazením z provozu v pořadí toosmooth hello přechod tooa novější nebo v nepodporované verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="3503a-223">Microsoft provides notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="3503a-224">Nové funkce a funkce a optimalizace, jsou přidány pouze aktuální toohello SDK, jako například se doporučuje tento můžete vždy upgradu toohello nejnovější verze sady SDK co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="3503a-224">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommended that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="3503a-225">Každá žádost tooCosmos DB pomocí vyřazeno sady SDK je odmítnuta službou hello.</span><span class="sxs-lookup"><span data-stu-id="3503a-225">Any request tooCosmos DB using a retired SDK is be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="3503a-226">Verze</span><span class="sxs-lookup"><span data-stu-id="3503a-226">Version</span></span> | <span data-ttu-id="3503a-227">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="3503a-227">Release Date</span></span> | <span data-ttu-id="3503a-228">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="3503a-228">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="3503a-229">1.12.2</span><span class="sxs-lookup"><span data-stu-id="3503a-229">1.12.2</span></span>](#1.12.2) |<span data-ttu-id="3503a-230">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="3503a-230">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="3503a-231">1.12.1</span><span class="sxs-lookup"><span data-stu-id="3503a-231">1.12.1</span></span>](#1.12.1) |<span data-ttu-id="3503a-232">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="3503a-232">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="3503a-233">1.12.0</span><span class="sxs-lookup"><span data-stu-id="3503a-233">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="3503a-234">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="3503a-234">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="3503a-235">1.11.0</span><span class="sxs-lookup"><span data-stu-id="3503a-235">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="3503a-236">16 března 2017</span><span class="sxs-lookup"><span data-stu-id="3503a-236">March 16, 2017</span></span> |--- |
| [<span data-ttu-id="3503a-237">1.10.2</span><span class="sxs-lookup"><span data-stu-id="3503a-237">1.10.2</span></span>](#1.10.2) |<span data-ttu-id="3503a-238">27. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="3503a-238">January 27, 2017</span></span> |--- |
| [<span data-ttu-id="3503a-239">1.10.1</span><span class="sxs-lookup"><span data-stu-id="3503a-239">1.10.1</span></span>](#1.10.1) |<span data-ttu-id="3503a-240">22. prosinci 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-240">December 22, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-241">1.10.0</span><span class="sxs-lookup"><span data-stu-id="3503a-241">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="3503a-242">03. října 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-242">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-243">1.9.0</span><span class="sxs-lookup"><span data-stu-id="3503a-243">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="3503a-244">07 července 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-244">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-245">1.8.0</span><span class="sxs-lookup"><span data-stu-id="3503a-245">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="3503a-246">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-246">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-247">1.7.0</span><span class="sxs-lookup"><span data-stu-id="3503a-247">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="3503a-248">26. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-248">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-249">1.6.0</span><span class="sxs-lookup"><span data-stu-id="3503a-249">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="3503a-250">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-250">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-251">1.5.6</span><span class="sxs-lookup"><span data-stu-id="3503a-251">1.5.6</span></span>](#1.5.6) |<span data-ttu-id="3503a-252">08 března 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-252">March 08, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-253">1.5.5</span><span class="sxs-lookup"><span data-stu-id="3503a-253">1.5.5</span></span>](#1.5.5) |<span data-ttu-id="3503a-254">02. února 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-254">February 02, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-255">1.5.4</span><span class="sxs-lookup"><span data-stu-id="3503a-255">1.5.4</span></span>](#1.5.4) |<span data-ttu-id="3503a-256">01. února 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-256">February 01, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-257">1.5.2</span><span class="sxs-lookup"><span data-stu-id="3503a-257">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="3503a-258">26 leden 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-258">January 26, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-259">1.5.2</span><span class="sxs-lookup"><span data-stu-id="3503a-259">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="3503a-260">22. ledna 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-260">January 22, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-261">1.5.1</span><span class="sxs-lookup"><span data-stu-id="3503a-261">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="3503a-262">4 leden 2016</span><span class="sxs-lookup"><span data-stu-id="3503a-262">January 4, 2016</span></span> |--- |
| [<span data-ttu-id="3503a-263">1.5.0</span><span class="sxs-lookup"><span data-stu-id="3503a-263">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="3503a-264">31. prosince 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-264">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-265">1.4.0</span><span class="sxs-lookup"><span data-stu-id="3503a-265">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="3503a-266">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-266">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-267">1.3.0</span><span class="sxs-lookup"><span data-stu-id="3503a-267">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="3503a-268">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-268">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-269">1.2.2</span><span class="sxs-lookup"><span data-stu-id="3503a-269">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="3503a-270">10 září 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-270">September 10, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-271">1.2.1</span><span class="sxs-lookup"><span data-stu-id="3503a-271">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="3503a-272">15 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-272">August 15, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-273">1.2.0</span><span class="sxs-lookup"><span data-stu-id="3503a-273">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="3503a-274">05 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-274">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-275">1.1.0</span><span class="sxs-lookup"><span data-stu-id="3503a-275">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="3503a-276">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-276">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-277">1.0.3</span><span class="sxs-lookup"><span data-stu-id="3503a-277">1.0.3</span></span>](#1.0.3) |<span data-ttu-id="3503a-278">04 červen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-278">June 04, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-279">1.0.2</span><span class="sxs-lookup"><span data-stu-id="3503a-279">1.0.2</span></span>](#1.0.2) |<span data-ttu-id="3503a-280">23 květen 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-280">May 23, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-281">1.0.1</span><span class="sxs-lookup"><span data-stu-id="3503a-281">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="3503a-282">15. května 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-282">May 15, 2015</span></span> |--- |
| [<span data-ttu-id="3503a-283">1.0.0</span><span class="sxs-lookup"><span data-stu-id="3503a-283">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="3503a-284">08 duben 2015</span><span class="sxs-lookup"><span data-stu-id="3503a-284">April 08, 2015</span></span> |--- |

## <a name="faq"></a><span data-ttu-id="3503a-285">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="3503a-285">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="3503a-286">Viz také</span><span class="sxs-lookup"><span data-stu-id="3503a-286">See also</span></span>
<span data-ttu-id="3503a-287">toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="3503a-287">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

