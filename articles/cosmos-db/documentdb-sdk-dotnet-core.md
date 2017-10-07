---
title: "aaaAzure Cosmos DB .NET Core API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o hello .NET Core API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi hello Azure Cosmos DB .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a><span data-ttu-id="97ebb-103">Azure Cosmos DB .NET Core SDK: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="97ebb-103">Azure Cosmos DB .NET Core SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="97ebb-104">.NET</span><span class="sxs-lookup"><span data-stu-id="97ebb-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="97ebb-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="97ebb-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="97ebb-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="97ebb-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="97ebb-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="97ebb-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="97ebb-108">Java</span><span class="sxs-lookup"><span data-stu-id="97ebb-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="97ebb-109">Python</span><span class="sxs-lookup"><span data-stu-id="97ebb-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="97ebb-110">REST</span><span class="sxs-lookup"><span data-stu-id="97ebb-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="97ebb-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="97ebb-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="97ebb-112">SQL</span><span class="sxs-lookup"><span data-stu-id="97ebb-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="97ebb-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="97ebb-113">**SDK download**</span></span></td><td>[<span data-ttu-id="97ebb-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="97ebb-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td><span data-ttu-id="97ebb-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="97ebb-115">**API documentation**</span></span></td><td>[<span data-ttu-id="97ebb-116">Referenční dokumentace rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="97ebb-116">.NET API reference documentation</span></span>](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="97ebb-117">**Ukázky**</span><span class="sxs-lookup"><span data-stu-id="97ebb-117">**Samples**</span></span></td><td>[<span data-ttu-id="97ebb-118">Ukázky kódu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="97ebb-118">.NET code samples</span></span>](documentdb-dotnet-samples.md)</td></tr>

<tr><td><span data-ttu-id="97ebb-119">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="97ebb-119">**Get started**</span></span></td><td>[<span data-ttu-id="97ebb-120">Začínáme s Azure Cosmos DB .NET Core SDK hello</span><span class="sxs-lookup"><span data-stu-id="97ebb-120">Get started with hello Azure Cosmos DB .NET Core SDK</span></span>](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td><span data-ttu-id="97ebb-121">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="97ebb-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="97ebb-122">Vývoj webových aplikací s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="97ebb-122">Web application development with Azure Cosmos DB</span></span>](documentdb-dotnet-application.md)</td></tr>

<tr><td><span data-ttu-id="97ebb-123">**Aktuální podporovaných prostředí**</span><span class="sxs-lookup"><span data-stu-id="97ebb-123">**Current supported framework**</span></span></td><td>[<span data-ttu-id="97ebb-124">Rozhraní .NET 1.6 standardní a .NET standardní 1.5</span><span class="sxs-lookup"><span data-stu-id="97ebb-124">.NET Standard 1.6 and .NET Standard 1.5</span></span>](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="97ebb-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="97ebb-125">Release Notes</span></span>

<span data-ttu-id="97ebb-126">Hello Azure Cosmos DB .NET Core SDK má parity funkcí s nejnovější verzí hello hello [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="97ebb-126">hello Azure Cosmos DB .NET Core SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="97ebb-127">Hello Azure Cosmos DB .NET Core SDK ještě není kompatibilní s aplikací pro univerzální platformu Windows (UWP).</span><span class="sxs-lookup"><span data-stu-id="97ebb-127">hello Azure Cosmos DB .NET Core SDK is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="97ebb-128">Pokud vás zajímá hello .NET Core SDK, který podporuje aplikace UWP odeslání e-mailu příliš[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="97ebb-128">If you are interested in hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

### <a name="a-name150150"></a><span data-ttu-id="97ebb-129"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-129"><a name="1.5.0"/>1.5.0</span></span> 

* <span data-ttu-id="97ebb-130">Přidaná podpora pro PartitionKeyRangeId jako FeedOption pro vymezení hodnotu klíče rozsahu konkrétního oddílu tooa výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="97ebb-130">Added support for PartitionKeyRangeId as a FeedOption for scoping query results tooa specific partition key range value.</span></span> 
* <span data-ttu-id="97ebb-131">Přidaná podpora pro StartTime jako ChangeFeedOption toostart, hledá změny hello po uplynutí této doby.</span><span class="sxs-lookup"><span data-stu-id="97ebb-131">Added support for StartTime as a ChangeFeedOption toostart looking for hello changes after that time.</span></span> 

### <a name="a-name141141"></a><span data-ttu-id="97ebb-132"><a name="1.4.1"/>1.4.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-132"><a name="1.4.1"/>1.4.1</span></span>

*   <span data-ttu-id="97ebb-133">Byl opraven problém v hello JsonSerializable třídu, která může způsobit výjimce přetečení zásobníku.</span><span class="sxs-lookup"><span data-stu-id="97ebb-133">Fixed an issue in hello JsonSerializable class that may cause a stack overflow exception.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="97ebb-134"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-134"><a name="1.4.0"/>1.4.0</span></span>

*   <span data-ttu-id="97ebb-135">Přidaná podpora pro zadání vlastní JsonSerializerSettings při vytváření instancí [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span><span class="sxs-lookup"><span data-stu-id="97ebb-135">Added support for specifying custom JsonSerializerSettings while instantiating a [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span></span>

### <a name="a-name132132"></a><span data-ttu-id="97ebb-136"><a name="1.3.2"/>1.3.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-136"><a name="1.3.2"/>1.3.2</span></span>

*   <span data-ttu-id="97ebb-137">Podpora rozhraní .NET standardní 1.5 jako jeden z hello cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="97ebb-137">Supporting .NET Standard 1.5 as one of hello target frameworks.</span></span>

### <a name="a-name131131"></a><span data-ttu-id="97ebb-138"><a name="1.3.1"/>1.3.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-138"><a name="1.3.1"/>1.3.1</span></span>

*   <span data-ttu-id="97ebb-139">Byl opraven problém ovlivňující x64 počítače, které nemají podporu SSE4 instrukce a throw SEHException – při spuštění dotazů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="97ebb-139">Fixed an issue that affected x64 machines that don’t support SSE4 instruction and throw SEHException when running Azure Cosmos DB queries.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="97ebb-140"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-140"><a name="1.3.0"/>1.3.0</span></span>

*   <span data-ttu-id="97ebb-141">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="97ebb-141">Added support for a new consistency level called ConsistentPrefix.</span></span>
*   <span data-ttu-id="97ebb-142">Přidaná podpora pro dotaz metriky pro jednotlivé oddíly.</span><span class="sxs-lookup"><span data-stu-id="97ebb-142">Added support for query metrics for individual partitions.</span></span>
*   <span data-ttu-id="97ebb-143">Přidaná podpora pro omezení velikosti hello hello token pokračování pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="97ebb-143">Added support for limiting hello size of hello continuation token for queries.</span></span>
*   <span data-ttu-id="97ebb-144">Přidaná podpora pro podrobnější trasování pro chybné žádosti.</span><span class="sxs-lookup"><span data-stu-id="97ebb-144">Added support for more detailed tracing for failed requests.</span></span>
*   <span data-ttu-id="97ebb-145">Provádí některé vylepšení výkonu v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="97ebb-145">Made some performance improvements in hello SDK.</span></span>

### <a name="a-name122122"></a><span data-ttu-id="97ebb-146"><a name="1.2.2"/>1.2.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-146"><a name="1.2.2"/>1.2.2</span></span>

* <span data-ttu-id="97ebb-147">Opravit problém, který ignoruje hello PartitionKey hodnota zadaná v FeedOptions pro agregační dotazy.</span><span class="sxs-lookup"><span data-stu-id="97ebb-147">Fixed an issue that ignored hello PartitionKey value provided in FeedOptions for aggregate queries.</span></span>
* <span data-ttu-id="97ebb-148">Byl opraven problém v transparentní zpracování oddílu správy během letu střední cross-partition Order By dotazu provádění.</span><span class="sxs-lookup"><span data-stu-id="97ebb-148">Fixed an issue in transparent handling of partition management during mid-flight cross-partition Order By query execution.</span></span>

### <a name="a-name121121"></a><span data-ttu-id="97ebb-149"><a name="1.2.1"/>1.2.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-149"><a name="1.2.1"/>1.2.1</span></span>

* <span data-ttu-id="97ebb-150">Byl opraven problém, který chybu způsobil zablokování v některých hello asynchronní rozhraní API, pokud se používá v kontextu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="97ebb-150">Fixed an issue which caused deadlocks in some of hello async APIs when used inside ASP.NET context.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="97ebb-151"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-151"><a name="1.2.0"/>1.2.0</span></span>

* <span data-ttu-id="97ebb-152">Opravy toomake SDK více odolné tooautomatic převzetí služeb při selhání za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="97ebb-152">Fixes toomake SDK more resilient tooautomatic failover under certain conditions.</span></span>

### <a name="a-name112112"></a><span data-ttu-id="97ebb-153"><a name="1.1.2"/>1.1.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-153"><a name="1.1.2"/>1.1.2</span></span>

* <span data-ttu-id="97ebb-154">Opravte pro problém způsobující příležitostně o výjimku WebException: hello vzdálený název nelze rozpoznat.</span><span class="sxs-lookup"><span data-stu-id="97ebb-154">Fix for an issue that occasionally causes a WebException: hello remote name could not be resolved.</span></span>
* <span data-ttu-id="97ebb-155">Přidání hello podporu pro přímo čtení typu dokumentu přidáním nové rozhraní API tooReadDocumentAsync přetížení.</span><span class="sxs-lookup"><span data-stu-id="97ebb-155">Added hello support for directly reading a typed document by adding new overloads tooReadDocumentAsync API.</span></span>

### <a name="a-name111111"></a><span data-ttu-id="97ebb-156"><a name="1.1.1"/>1.1.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-156"><a name="1.1.1"/>1.1.1</span></span>

* <span data-ttu-id="97ebb-157">Byla přidána podpora LINQ pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="97ebb-157">Added LINQ support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="97ebb-158">Opravte pro problém nevracení paměti pro objekt ConnectionPolicy hello způsobené hello použití obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="97ebb-158">Fix for a memory leak issue for hello ConnectionPolicy object caused by hello use of event handler.</span></span>
* <span data-ttu-id="97ebb-159">Oprava problému, ve kterém nebyl UpsertAttachmentAsync pracovat, když byla použita značka ETag.</span><span class="sxs-lookup"><span data-stu-id="97ebb-159">Fix for an issue wherein UpsertAttachmentAsync was not working when ETag was used.</span></span>
* <span data-ttu-id="97ebb-160">Oprava problému, ve kterém nebyl křížové oddílu klauzule order by dotazu pokračování práce při řazení na pole řetězce.</span><span class="sxs-lookup"><span data-stu-id="97ebb-160">Fix for an issue wherein cross partition order-by query continuation was not working when sorting on string field.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="97ebb-161"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-161"><a name="1.1.0"/>1.1.0</span></span>

* <span data-ttu-id="97ebb-162">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="97ebb-162">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="97ebb-163">V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="97ebb-163">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="97ebb-164">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="97ebb-164">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="97ebb-165"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-165"><a name="1.0.0"/>1.0.0</span></span>

<span data-ttu-id="97ebb-166">Hello Azure Cosmos DB .NET Core SDK vám umožní toobuild rychlá napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) toorun aplikace v systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="97ebb-166">hello Azure Cosmos DB .NET Core SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span> <span data-ttu-id="97ebb-167">Hello nejnovější verzi hello Azure Cosmos DB .NET Core SDK je plně [Xamarin](https://www.xamarin.com) kompatibilní a použít toobuild aplikacemi, které cílí na iOS, Android a Mono (Linux).</span><span class="sxs-lookup"><span data-stu-id="97ebb-167">hello latest release of hello Azure Cosmos DB .NET Core SDK is fully [Xamarin](https://www.xamarin.com) compatible and be used toobuild applications that target iOS, Android, and Mono (Linux).</span></span>  

### <a name="a-name010-preview010-preview"></a><span data-ttu-id="97ebb-168"><a name="0.1.0-preview"/>0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="97ebb-168"><a name="0.1.0-preview"/>0.1.0-preview</span></span>

<span data-ttu-id="97ebb-169">Hello Azure Cosmos DB .NET Core Preview SDK vám umožní toobuild rychlá napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) toorun aplikace v systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="97ebb-169">hello Azure Cosmos DB .NET Core Preview SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span>

<span data-ttu-id="97ebb-170">Hello Azure Cosmos DB .NET Core Preview SDK má parity funkcí s nejnovější verzí hello hello [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) a podporuje následující hello:</span><span class="sxs-lookup"><span data-stu-id="97ebb-170">hello Azure Cosmos DB .NET Core Preview SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) and supports hello following:</span></span>
* <span data-ttu-id="97ebb-171">Všechny [připojení režimy](performance-tips.md#networking): režim brány, přímé TCP a přímé HTTPs.</span><span class="sxs-lookup"><span data-stu-id="97ebb-171">All [connection modes](performance-tips.md#networking): Gateway mode, Direct TCP, and Direct HTTPs.</span></span> 
* <span data-ttu-id="97ebb-172">Všechny [úrovně konzistence](consistency-levels.md): silným, relace, typu s ohraničenou Prošlostí a Eventual.</span><span class="sxs-lookup"><span data-stu-id="97ebb-172">All [consistency levels](consistency-levels.md): Strong, Session, Bounded Staleness, and Eventual.</span></span>
* <span data-ttu-id="97ebb-173">[Oddíly kolekce](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="97ebb-173">[Partitioned collections](partition-data.md).</span></span> 
* <span data-ttu-id="97ebb-174">[Účty databáze více oblasti a geografická replikace](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="97ebb-174">[Multi-region database accounts and geo-replication](distribute-data-globally.md).</span></span>

<span data-ttu-id="97ebb-175">Pokud máte otázky související toothis SDK, post příliš[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), nebo soubor problém v hello [úložiště github](https://github.com/Azure/azure-documentdb-dotnet/issues).</span><span class="sxs-lookup"><span data-stu-id="97ebb-175">If you have questions related toothis SDK, post too[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), or file an issue in hello [github repository](https://github.com/Azure/azure-documentdb-dotnet/issues).</span></span> 

## <a name="release--retirement-dates"></a><span data-ttu-id="97ebb-176">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="97ebb-176">Release & Retirement Dates</span></span>

| <span data-ttu-id="97ebb-177">Verze</span><span class="sxs-lookup"><span data-stu-id="97ebb-177">Version</span></span> | <span data-ttu-id="97ebb-178">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="97ebb-178">Release Date</span></span> | <span data-ttu-id="97ebb-179">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="97ebb-179">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="97ebb-180">1.5.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-180">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="97ebb-181">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-181">August 10, 2017</span></span> |--- | 
| [<span data-ttu-id="97ebb-182">1.4.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-182">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="97ebb-183">07 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-183">August 07, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-184">1.4.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-184">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="97ebb-185">02 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-185">August 02, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-186">1.3.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-186">1.3.2</span></span>](#1.3.2) |<span data-ttu-id="97ebb-187">12. června 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-187">June 12, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-188">1.3.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-188">1.3.1</span></span>](#1.3.1) |<span data-ttu-id="97ebb-189">23 může 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-189">May 23, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-190">1.3.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-190">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="97ebb-191">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-191">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-192">1.2.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-192">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="97ebb-193">19. dubna 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-193">April 19, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-194">1.2.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-194">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="97ebb-195">29. března 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-195">March 29, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-196">1.2.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-196">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="97ebb-197">25. března 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-197">March 25, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-198">1.1.2</span><span class="sxs-lookup"><span data-stu-id="97ebb-198">1.1.2</span></span>](#1.1.2) |<span data-ttu-id="97ebb-199">20. března 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-199">March 20, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-200">1.1.1</span><span class="sxs-lookup"><span data-stu-id="97ebb-200">1.1.1</span></span>](#1.1.1) |<span data-ttu-id="97ebb-201">14. března 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-201">March 14, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-202">1.1.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-202">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="97ebb-203">16 února 2017</span><span class="sxs-lookup"><span data-stu-id="97ebb-203">February 16, 2017</span></span> |--- |
| [<span data-ttu-id="97ebb-204">1.0.0</span><span class="sxs-lookup"><span data-stu-id="97ebb-204">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="97ebb-205">21 prosince 2016</span><span class="sxs-lookup"><span data-stu-id="97ebb-205">December 21, 2016</span></span> |--- |
| [<span data-ttu-id="97ebb-206">0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="97ebb-206">0.1.0-preview</span></span>](#0.1.0-preview) |<span data-ttu-id="97ebb-207">15. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="97ebb-207">November 15, 2016</span></span> |<span data-ttu-id="97ebb-208">31. prosinci 2016</span><span class="sxs-lookup"><span data-stu-id="97ebb-208">December 31, 2016</span></span> |

## <a name="see-also"></a><span data-ttu-id="97ebb-209">Viz také</span><span class="sxs-lookup"><span data-stu-id="97ebb-209">See Also</span></span>
<span data-ttu-id="97ebb-210">toolearn Další informace o Cosmos databáze, najdete v části [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="97ebb-210">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

