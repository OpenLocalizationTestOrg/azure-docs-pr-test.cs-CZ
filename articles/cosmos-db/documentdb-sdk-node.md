---
title: "Azure Cosmos DB Node.js API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o rozhraní API Node.js a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi sady Azure Cosmos DB Node.js SDK."
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
ms.openlocfilehash: 4376a5c07b5f00311ce0fe3c0056efdf79c273f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a><span data-ttu-id="a5d62-103">Azure SDK Cosmos DB Node.js: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="a5d62-103">Azure Cosmos DB Node.js SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5d62-104">.NET</span><span class="sxs-lookup"><span data-stu-id="a5d62-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="a5d62-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a5d62-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="a5d62-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5d62-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="a5d62-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="a5d62-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="a5d62-108">Java</span><span class="sxs-lookup"><span data-stu-id="a5d62-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="a5d62-109">Python</span><span class="sxs-lookup"><span data-stu-id="a5d62-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="a5d62-110">REST</span><span class="sxs-lookup"><span data-stu-id="a5d62-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="a5d62-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="a5d62-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="a5d62-112">SQL</span><span class="sxs-lookup"><span data-stu-id="a5d62-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="a5d62-113">**Stáhněte si sadu SDK**</span><span class="sxs-lookup"><span data-stu-id="a5d62-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="a5d62-114">NPM</span><span class="sxs-lookup"><span data-stu-id="a5d62-114">NPM</span></span>](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td><span data-ttu-id="a5d62-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="a5d62-115">**API documentation**</span></span></td><td>[<span data-ttu-id="a5d62-116">Referenční dokumentace rozhraní API Node.js</span><span class="sxs-lookup"><span data-stu-id="a5d62-116">Node.js API reference documentation</span></span>](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td><span data-ttu-id="a5d62-117">**Pokyny k instalaci sady SDK**</span><span class="sxs-lookup"><span data-stu-id="a5d62-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="a5d62-118">Pokyny k instalaci</span><span class="sxs-lookup"><span data-stu-id="a5d62-118">Installation instructions</span></span>](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td><span data-ttu-id="a5d62-119">**Můžete přispět k sadě SDK**</span><span class="sxs-lookup"><span data-stu-id="a5d62-119">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="a5d62-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="a5d62-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td><span data-ttu-id="a5d62-121">**Ukázky**</span><span class="sxs-lookup"><span data-stu-id="a5d62-121">**Samples**</span></span></td><td>[<span data-ttu-id="a5d62-122">Ukázky kódu Node.js</span><span class="sxs-lookup"><span data-stu-id="a5d62-122">Node.js code samples</span></span>](documentdb-nodejs-samples.md)</td></tr>

<tr><td><span data-ttu-id="a5d62-123">**Kurz Začínáme**</span><span class="sxs-lookup"><span data-stu-id="a5d62-123">**Get started tutorial**</span></span></td><td>[<span data-ttu-id="a5d62-124">Začínáme s Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="a5d62-124">Get started with the Node.js SDK</span></span>](documentdb-nodejs-get-started.md)</td></tr>

<tr><td><span data-ttu-id="a5d62-125">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="a5d62-125">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="a5d62-126">Vytvoření webové aplikace Node.js pomocí Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a5d62-126">Build a Node.js web application using Azure Cosmos DB</span></span>](documentdb-nodejs-application.md)</td></tr>

<tr><td><span data-ttu-id="a5d62-127">**Aktuální podporované platformy**</span><span class="sxs-lookup"><span data-stu-id="a5d62-127">**Current supported platform**</span></span></td><td> 
[<span data-ttu-id="a5d62-128">Verze 6.x Node.js</span><span class="sxs-lookup"><span data-stu-id="a5d62-128">Node.js v6.x</span></span>](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[<span data-ttu-id="a5d62-129">Node.js v4.2.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-129">Node.js v4.2.0</span></span>](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[<span data-ttu-id="a5d62-130">Node.js v0.12</span><span class="sxs-lookup"><span data-stu-id="a5d62-130">Node.js v0.12</span></span>](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
<span data-ttu-id="a5d62-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span><span class="sxs-lookup"><span data-stu-id="a5d62-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="a5d62-132">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="a5d62-132">Release notes</span></span>

### <span data-ttu-id="a5d62-133"><a name="1.12.2"/>1.12.2</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-133"><a name="1.12.2"/>1.12.2</a></span></span>
*   <span data-ttu-id="a5d62-134">dokumentace npm pevné.</span><span class="sxs-lookup"><span data-stu-id="a5d62-134">npm documentation fixed.</span></span>

### <span data-ttu-id="a5d62-135"><a name="1.12.1"/>1.12.1</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-135"><a name="1.12.1"/>1.12.1</a></span></span>
* <span data-ttu-id="a5d62-136">Pevná chyby ve executeStoredProcedure kde dokumentů, které měl speciální znaky znakové sady Unicode (LS, PS).</span><span class="sxs-lookup"><span data-stu-id="a5d62-136">Fixed a bug in executeStoredProcedure where documents involved had special Unicode characters (LS, PS).</span></span>
* <span data-ttu-id="a5d62-137">Opravit chyby při zpracování dokumentů s znaky kódování Unicode v klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-137">Fixed a bug in handling documents with Unicode characters in the partition key.</span></span>
* <span data-ttu-id="a5d62-138">Opravené podpora pro vytváření kolekcí s názvem média.</span><span class="sxs-lookup"><span data-stu-id="a5d62-138">Fixed support for creating collections with the name media.</span></span> <span data-ttu-id="a5d62-139">Problém Githubu #114.</span><span class="sxs-lookup"><span data-stu-id="a5d62-139">Github issue #114.</span></span>
* <span data-ttu-id="a5d62-140">Opravené podpora oprávnění autorizační token.</span><span class="sxs-lookup"><span data-stu-id="a5d62-140">Fixed support for permission authorization token.</span></span> <span data-ttu-id="a5d62-141">Problém Githubu #178.</span><span class="sxs-lookup"><span data-stu-id="a5d62-141">Github issue #178.</span></span>

### <span data-ttu-id="a5d62-142"><a name="1.12.0"/>1.12.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-142"><a name="1.12.0"/>1.12.0</a></span></span>
* <span data-ttu-id="a5d62-143">Přidaná podpora pro novou [úroveň konzistence](consistency-levels.md) názvem ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="a5d62-143">Added support for a new [consistency level](consistency-levels.md) called ConsistentPrefix.</span></span>
* <span data-ttu-id="a5d62-144">Přidaná podpora pro UriFactory.</span><span class="sxs-lookup"><span data-stu-id="a5d62-144">Added support for UriFactory.</span></span>
* <span data-ttu-id="a5d62-145">Opravit chyby Podpora kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="a5d62-145">Fixed a Unicode support bug.</span></span> <span data-ttu-id="a5d62-146">#171 potíže Githubu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-146">GitHub issue #171.</span></span>

### <span data-ttu-id="a5d62-147"><a name="1.11.0"/>1.11.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-147"><a name="1.11.0"/>1.11.0</a></span></span>
* <span data-ttu-id="a5d62-148">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="a5d62-148">Added the support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="a5d62-149">Přidání možnosti pro řízení stupně paralelního zpracování pro různé dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-149">Added the option for controlling degree of parallelism for cross partition queries.</span></span>
* <span data-ttu-id="a5d62-150">Přidání možnosti pro zákaz protokolu SSL ověření při spuštění emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="a5d62-150">Added the option for disabling SSL verification when running against Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="a5d62-151">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s na 2 500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="a5d62-151">Lowered minimum throughput on partitioned collections from 10,100 RU/s to 2500 RU/s.</span></span>
* <span data-ttu-id="a5d62-152">Opravit chyby token pokračování pro kolekce tvořené jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="a5d62-152">Fixed the continuation token bug for single partition collection.</span></span> <span data-ttu-id="a5d62-153">Problém Githubu #107.</span><span class="sxs-lookup"><span data-stu-id="a5d62-153">Github issue #107.</span></span>
* <span data-ttu-id="a5d62-154">Opravit chyby executeStoredProcedure při zpracování 0 jako jeden param.</span><span class="sxs-lookup"><span data-stu-id="a5d62-154">Fixed the executeStoredProcedure bug in handling 0 as single param.</span></span> <span data-ttu-id="a5d62-155">Problém Githubu #155.</span><span class="sxs-lookup"><span data-stu-id="a5d62-155">Github issue #155.</span></span>

### <span data-ttu-id="a5d62-156"><a name="1.10.2"/>1.10.2</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-156"><a name="1.10.2"/>1.10.2</a></span></span>
* <span data-ttu-id="a5d62-157">Záhlaví pevné user-agent zahrnout verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="a5d62-157">Fixed user-agent header to include the SDK version.</span></span>
* <span data-ttu-id="a5d62-158">Méně závažné kód čištění.</span><span class="sxs-lookup"><span data-stu-id="a5d62-158">Minor code cleanup.</span></span>

### <span data-ttu-id="a5d62-159"><a name="1.10.1"/>1.10.1</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-159"><a name="1.10.1"/>1.10.1</a></span></span>
* <span data-ttu-id="a5d62-160">Zakázání ověřování SSL, když pomocí sady SDK cílit emulator(hostname=localhost).</span><span class="sxs-lookup"><span data-stu-id="a5d62-160">Disabling SSL verification when using the SDK to target the emulator(hostname=localhost).</span></span>
* <span data-ttu-id="a5d62-161">Přidaná podpora pro povolení protokolování skriptu během provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="a5d62-161">Added support for enabling script logging during stored procedure execution.</span></span>

### <span data-ttu-id="a5d62-162"><a name="1.10.0"/>1.10.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-162"><a name="1.10.0"/>1.10.0</a></span></span>
* <span data-ttu-id="a5d62-163">Přidaná podpora pro křížové paralelní dotazy oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-163">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="a5d62-164">Byla přidána podpora pro horní nebo ORDER BY a dotazy pro dělené kolekce.</span><span class="sxs-lookup"><span data-stu-id="a5d62-164">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>

### <span data-ttu-id="a5d62-165"><a name="1.9.0"/>1.9.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-165"><a name="1.9.0"/>1.9.0</a></span></span>
* <span data-ttu-id="a5d62-166">Podpora zásad přidané opakování omezenému požadavky.</span><span class="sxs-lookup"><span data-stu-id="a5d62-166">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="a5d62-167">(Omezenému požadavky obdrží žádost o míra příliš velký výjimka, kód chyby 429.) Ve výchozím nastavení Azure Cosmos DB opakuje devětkrát pro každý požadavek vyskytne kód chyby 429, aby byla dodržena retryAfter čas v hlavičku odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a5d62-167">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring the retryAfter time in the response header.</span></span> <span data-ttu-id="a5d62-168">Časový interval opakování pevné lze nyní nastavit jako součást RetryOptions vlastnost v objektu ConnectionPolicy Pokud budete chtít ignorovat čas retryAfter vrácená serverem mezi jednotlivými pokusy o odeslání.</span><span class="sxs-lookup"><span data-stu-id="a5d62-168">A fixed retry interval time can now be set as part of the RetryOptions property on the ConnectionPolicy object if you want to ignore the retryAfter time returned by server between the retries.</span></span> <span data-ttu-id="a5d62-169">Azure Cosmos DB nyní čeká maximálně 30 sekund pro každý požadavek, který je omezené (bez ohledu na počet opakování) a vrátí odpověď s kódem chyby 429.</span><span class="sxs-lookup"><span data-stu-id="a5d62-169">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns the response with error code 429.</span></span> <span data-ttu-id="a5d62-170">Nyní lze přepsat také ve vlastnosti RetryOptions ConnectionPolicy objektu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-170">This time can also be overridden in the RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="a5d62-171">Cosmos DB nyní vrátí x-ms omezení--počet opakování a x-ms-throttle-retry-wait-time-ms jako opakovat hlavičky odpovědi v každé žádosti k označení omezení počtu a kumulativní čas požadavku čekali mezi jednotlivými pokusy o odeslání.</span><span class="sxs-lookup"><span data-stu-id="a5d62-171">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as the response headers in every request to denote the throttle retry count and the cumulative time the request waited between the retries.</span></span>
* <span data-ttu-id="a5d62-172">Třída RetryOptions byla přidána vystavení vlastnost RetryOptions na ConnectionPolicy třídu, která slouží k některé z možností opakování výchozí přepsat.</span><span class="sxs-lookup"><span data-stu-id="a5d62-172">The RetryOptions class was added, exposing the RetryOptions property on the ConnectionPolicy class that can be used to override some of the default retry options.</span></span>

### <span data-ttu-id="a5d62-173"><a name="1.8.0"/>1.8.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-173"><a name="1.8.0"/>1.8.0</a></span></span>
* <span data-ttu-id="a5d62-174">Přidaná podpora pro účty databáze více oblast.</span><span class="sxs-lookup"><span data-stu-id="a5d62-174">Added the support for multi-region database accounts.</span></span>

### <span data-ttu-id="a5d62-175"><a name="1.7.0"/>1.7.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-175"><a name="1.7.0"/>1.7.0</a></span></span>
* <span data-ttu-id="a5d62-176">Přidaná podpora pro funkce čas k Live(TTL) pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="a5d62-176">Added the support for Time To Live(TTL) feature for documents.</span></span>

### <span data-ttu-id="a5d62-177"><a name="1.6.0"/>1.6.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-177"><a name="1.6.0"/>1.6.0</a></span></span>
* <span data-ttu-id="a5d62-178">Implementovat [oddíly kolekce](partition-data.md) a [úrovně výkonu uživatelem definované](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="a5d62-178">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <span data-ttu-id="a5d62-179"><a name="1.5.6"/>1.5.6</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-179"><a name="1.5.6"/>1.5.6</a></span></span>
* <span data-ttu-id="a5d62-180">Opravené chyby RangePartitionResolver.resolveForRead, kde ji nebyl vrácení odkazy z důvodu chybné concat výsledků.</span><span class="sxs-lookup"><span data-stu-id="a5d62-180">Fixed RangePartitionResolver.resolveForRead bug where it was not returning links due to a bad concat of results.</span></span>

### <span data-ttu-id="a5d62-181"><a name="1.5.5"/>1.5.5</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-181"><a name="1.5.5"/>1.5.5</a></span></span>
* <span data-ttu-id="a5d62-182">Pevné hashParitionResolver resolveForRead(): žádné předaný klíč oddílu byla při vyvolání výjimky, místo vrací seznam všech registrovaných odkazů.</span><span class="sxs-lookup"><span data-stu-id="a5d62-182">Fixed hashParitionResolver resolveForRead(): When no partition key supplied was throwing exception, instead of returning a list of all registered links.</span></span>

### <span data-ttu-id="a5d62-183"><a name="1.5.4"/>1.5.4</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-183"><a name="1.5.4"/>1.5.4</a></span></span>
* <span data-ttu-id="a5d62-184">Řeší problém [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -vyhrazené agenta HTTPS: neměli upravovat globální agenta pro účely Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a5d62-184">Fixes issue [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Dedicated HTTPS Agent: Avoid modifying the global agent for Azure Cosmos DB purposes.</span></span> <span data-ttu-id="a5d62-185">Použijte vyhrazenou agenta pro všechny požadavky lib.</span><span class="sxs-lookup"><span data-stu-id="a5d62-185">Use a dedicated agent for all of the lib’s requests.</span></span>

### <span data-ttu-id="a5d62-186"><a name="1.5.3"/>1.5.3</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-186"><a name="1.5.3"/>1.5.3</a></span></span>
* <span data-ttu-id="a5d62-187">Řeší problém [#81](https://github.com/Azure/azure-documentdb-node/issues/81) – správně zpracovat pomlčky v ID média.</span><span class="sxs-lookup"><span data-stu-id="a5d62-187">Fixes issue [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - Properly handle dashes in media ids.</span></span>

### <span data-ttu-id="a5d62-188"><a name="1.5.2"/>1.5.2</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-188"><a name="1.5.2"/>1.5.2</a></span></span>
* <span data-ttu-id="a5d62-189">Řeší problém [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -naslouchací proces EventEmitter úniku upozornění.</span><span class="sxs-lookup"><span data-stu-id="a5d62-189">Fixes issue [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter listener leak warning.</span></span>

### <span data-ttu-id="a5d62-190"><a name="1.5.1"/>1.5.1</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-190"><a name="1.5.1"/>1.5.1</a></span></span>
* <span data-ttu-id="a5d62-191">Řeší problém [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -přejmenovat složku Hash na hodnotu hash pro systémy malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="a5d62-191">Fixes issue [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - rename folder Hash to hash for case-sensitive systems.</span></span>

### <span data-ttu-id="a5d62-192"><a name="1.5.0"/>1.5.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-192"><a name="1.5.0"/>1.5.0</a></span></span>
* <span data-ttu-id="a5d62-193">Podpora horizontálního dělení implementace přidáním hash p & ro překladače oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-193">Implement sharding support by adding hash & range partition resolvers.</span></span>

### <span data-ttu-id="a5d62-194"><a name="1.4.0"/>1.4.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-194"><a name="1.4.0"/>1.4.0</a></span></span>
* <span data-ttu-id="a5d62-195">Implementujte Upsert.</span><span class="sxs-lookup"><span data-stu-id="a5d62-195">Implement Upsert.</span></span> <span data-ttu-id="a5d62-196">Nové metody upsertXXX na documentClient.</span><span class="sxs-lookup"><span data-stu-id="a5d62-196">New upsertXXX methods on documentClient.</span></span>

### <span data-ttu-id="a5d62-197"><a name="1.3.0"/>1.3.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-197"><a name="1.3.0"/>1.3.0</a></span></span>
* <span data-ttu-id="a5d62-198">Přeskočeno mají být předány čísla verzí zarovnání s dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="a5d62-198">Skipped to bring version numbers in alignment with other SDKs.</span></span>

### <span data-ttu-id="a5d62-199"><a name="1.2.2"/>1.2.2</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-199"><a name="1.2.2"/>1.2.2</a></span></span>
* <span data-ttu-id="a5d62-200">Rozdělení Q nabízí obálku do nového úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5d62-200">Split Q promises wrapper to new repository.</span></span>
* <span data-ttu-id="a5d62-201">Aktualizace k souboru balíčku pro npm registru.</span><span class="sxs-lookup"><span data-stu-id="a5d62-201">Update to package file for npm registry.</span></span>

### <span data-ttu-id="a5d62-202"><a name="1.2.1"/>1.2.1</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-202"><a name="1.2.1"/>1.2.1</a></span></span>
* <span data-ttu-id="a5d62-203">Implementuje ID založené na směrování.</span><span class="sxs-lookup"><span data-stu-id="a5d62-203">Implements ID Based Routing.</span></span>
* <span data-ttu-id="a5d62-204">Řeší problém [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuální vlastnost je v konfliktu s má objekt current() metoda.</span><span class="sxs-lookup"><span data-stu-id="a5d62-204">Fixes Issue [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - current property conflicts with method current().</span></span>

### <span data-ttu-id="a5d62-205"><a name="1.2.0"/>1.2.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-205"><a name="1.2.0"/>1.2.0</a></span></span>
* <span data-ttu-id="a5d62-206">Byla přidána podpora pro geoprostorové index.</span><span class="sxs-lookup"><span data-stu-id="a5d62-206">Added support for GeoSpatial index.</span></span>
* <span data-ttu-id="a5d62-207">Ověří vlastnost id pro všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="a5d62-207">Validates id property for all resources.</span></span> <span data-ttu-id="a5d62-208">Identifikátory prostředků nesmí obsahovat?, /, # &#47; &#47; znaky nebo končit mezerou.</span><span class="sxs-lookup"><span data-stu-id="a5d62-208">Ids for resources cannot contain ?, /, #, &#47;&#47;, characters or end with a space.</span></span>
* <span data-ttu-id="a5d62-209">Přidá nové záhlaví "index transformace průběh" ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="a5d62-209">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <span data-ttu-id="a5d62-210"><a name="1.1.0"/>1.1.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-210"><a name="1.1.0"/>1.1.0</a></span></span>
* <span data-ttu-id="a5d62-211">Implementuje zásady indexování V2.</span><span class="sxs-lookup"><span data-stu-id="a5d62-211">Implements V2 indexing policy.</span></span>

### <span data-ttu-id="a5d62-212"><a name="1.0.3"/>1.0.3</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-212"><a name="1.0.3"/>1.0.3</a></span></span>
* <span data-ttu-id="a5d62-213">Problém [#40](https://github.com/Azure/azure-documentdb-node/issues/40) – implementována eslint grunt konfigurace v základní a promise SDK.</span><span class="sxs-lookup"><span data-stu-id="a5d62-213">Issue [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Implemented eslint and grunt configurations in the core and promise SDK.</span></span>

### <span data-ttu-id="a5d62-214"><a name="1.0.2"/>1.0.2</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-214"><a name="1.0.2"/>1.0.2</a></span></span>
* <span data-ttu-id="a5d62-215">Problém [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -lišící obálku nezahrnuje hlavičky s chybou.</span><span class="sxs-lookup"><span data-stu-id="a5d62-215">Issue [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises wrapper does not include header with error.</span></span>

### <span data-ttu-id="a5d62-216"><a name="1.0.1"/>1.0.1</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-216"><a name="1.0.1"/>1.0.1</a></span></span>
* <span data-ttu-id="a5d62-217">Implementovaná možnost dotazu pro přidáním readConflicts, readConflictAsync a queryConflicts je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="a5d62-217">Implemented ability to query for conflicts by adding readConflicts, readConflictAsync, and queryConflicts.</span></span>
* <span data-ttu-id="a5d62-218">Aktualizované dokumentace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5d62-218">Updated API documentation.</span></span>
* <span data-ttu-id="a5d62-219">Problém [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync chyby.</span><span class="sxs-lookup"><span data-stu-id="a5d62-219">Issue [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync error.</span></span>

### <span data-ttu-id="a5d62-220"><a name="1.0.0"/>1.0.0</a></span><span class="sxs-lookup"><span data-stu-id="a5d62-220"><a name="1.0.0"/>1.0.0</a></span></span>
* <span data-ttu-id="a5d62-221">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="a5d62-221">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="a5d62-222">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="a5d62-222">Release & Retirement Dates</span></span>
<span data-ttu-id="a5d62-223">Společnost Microsoft poskytuje oznámení alespoň **dobu 12 měsíců** předem vyřazení sady SDK k funkce smooth přechodu na novější nebo podporované verzi.</span><span class="sxs-lookup"><span data-stu-id="a5d62-223">Microsoft provides notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="a5d62-224">Nové funkce a funkce a optimalizace, jsou přidány pouze v aktuální sadě SDK, jako takový se doporučuje, aby vždy upgradu na nejnovější verze sady SDK v míře.</span><span class="sxs-lookup"><span data-stu-id="a5d62-224">New features and functionality and optimizations are only added to the current SDK, as such it is  recommended that you always upgrade to the latest SDK version as early as possible.</span></span>

<span data-ttu-id="a5d62-225">Každá žádost o pomocí Cosmos DB, že je vyřazeno SDK odmítnuta službou.</span><span class="sxs-lookup"><span data-stu-id="a5d62-225">Any request to Cosmos DB using a retired SDK is be rejected by the service.</span></span>

<br/>

| <span data-ttu-id="a5d62-226">Verze</span><span class="sxs-lookup"><span data-stu-id="a5d62-226">Version</span></span> | <span data-ttu-id="a5d62-227">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="a5d62-227">Release Date</span></span> | <span data-ttu-id="a5d62-228">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="a5d62-228">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="a5d62-229">1.12.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-229">1.12.2</span></span>](#1.12.2) |<span data-ttu-id="a5d62-230">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="a5d62-230">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="a5d62-231">1.12.1</span><span class="sxs-lookup"><span data-stu-id="a5d62-231">1.12.1</span></span>](#1.12.1) |<span data-ttu-id="a5d62-232">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="a5d62-232">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="a5d62-233">1.12.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-233">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="a5d62-234">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="a5d62-234">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="a5d62-235">1.11.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-235">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="a5d62-236">16 března 2017</span><span class="sxs-lookup"><span data-stu-id="a5d62-236">March 16, 2017</span></span> |--- |
| [<span data-ttu-id="a5d62-237">1.10.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-237">1.10.2</span></span>](#1.10.2) |<span data-ttu-id="a5d62-238">27. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="a5d62-238">January 27, 2017</span></span> |--- |
| [<span data-ttu-id="a5d62-239">1.10.1</span><span class="sxs-lookup"><span data-stu-id="a5d62-239">1.10.1</span></span>](#1.10.1) |<span data-ttu-id="a5d62-240">22. prosinci 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-240">December 22, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-241">1.10.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-241">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="a5d62-242">03. října 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-242">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-243">1.9.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-243">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="a5d62-244">07 července 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-244">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-245">1.8.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-245">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="a5d62-246">14. června 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-246">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-247">1.7.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-247">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="a5d62-248">26. dubna 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-248">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-249">1.6.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-249">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="a5d62-250">29. března 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-250">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-251">1.5.6</span><span class="sxs-lookup"><span data-stu-id="a5d62-251">1.5.6</span></span>](#1.5.6) |<span data-ttu-id="a5d62-252">08 března 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-252">March 08, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-253">1.5.5</span><span class="sxs-lookup"><span data-stu-id="a5d62-253">1.5.5</span></span>](#1.5.5) |<span data-ttu-id="a5d62-254">02. února 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-254">February 02, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-255">1.5.4</span><span class="sxs-lookup"><span data-stu-id="a5d62-255">1.5.4</span></span>](#1.5.4) |<span data-ttu-id="a5d62-256">01. února 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-256">February 01, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-257">1.5.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-257">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="a5d62-258">26 leden 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-258">January 26, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-259">1.5.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-259">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="a5d62-260">22. ledna 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-260">January 22, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-261">1.5.1</span><span class="sxs-lookup"><span data-stu-id="a5d62-261">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="a5d62-262">4 leden 2016</span><span class="sxs-lookup"><span data-stu-id="a5d62-262">January 4, 2016</span></span> |--- |
| [<span data-ttu-id="a5d62-263">1.5.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-263">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="a5d62-264">31. prosince 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-264">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-265">1.4.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-265">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="a5d62-266">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-266">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-267">1.3.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-267">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="a5d62-268">06 říjen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-268">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-269">1.2.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-269">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="a5d62-270">10 září 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-270">September 10, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-271">1.2.1</span><span class="sxs-lookup"><span data-stu-id="a5d62-271">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="a5d62-272">15 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-272">August 15, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-273">1.2.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-273">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="a5d62-274">05 srpen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-274">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-275">1.1.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-275">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="a5d62-276">09 července 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-276">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-277">1.0.3</span><span class="sxs-lookup"><span data-stu-id="a5d62-277">1.0.3</span></span>](#1.0.3) |<span data-ttu-id="a5d62-278">04 červen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-278">June 04, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-279">1.0.2</span><span class="sxs-lookup"><span data-stu-id="a5d62-279">1.0.2</span></span>](#1.0.2) |<span data-ttu-id="a5d62-280">23 květen 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-280">May 23, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-281">1.0.1</span><span class="sxs-lookup"><span data-stu-id="a5d62-281">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="a5d62-282">15. května 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-282">May 15, 2015</span></span> |--- |
| [<span data-ttu-id="a5d62-283">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a5d62-283">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="a5d62-284">08 duben 2015</span><span class="sxs-lookup"><span data-stu-id="a5d62-284">April 08, 2015</span></span> |--- |

## <a name="faq"></a><span data-ttu-id="a5d62-285">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a5d62-285">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="a5d62-286">Viz také</span><span class="sxs-lookup"><span data-stu-id="a5d62-286">See also</span></span>
<span data-ttu-id="a5d62-287">Další informace o Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="a5d62-287">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

