---
title: "Azure Cosmos DB .NET základní rozhraní API, sadu SDK a prostředky | Microsoft Docs"
description: "Další informace o rozhraní API .NET Core a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi Cosmos DB .NET SDK služby Azure jádra."
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
ms.openlocfilehash: a7ce4d771e9c655687f72f4b46c7405cf64aeb74
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a><span data-ttu-id="a5744-103">Azure Cosmos DB .NET Core SDK: Poznámky k verzi a prostředky</span><span class="sxs-lookup"><span data-stu-id="a5744-103">Azure Cosmos DB .NET Core SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5744-104">.NET</span><span class="sxs-lookup"><span data-stu-id="a5744-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="a5744-105">Informační kanál změnu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a5744-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="a5744-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5744-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="a5744-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="a5744-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="a5744-108">Java</span><span class="sxs-lookup"><span data-stu-id="a5744-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="a5744-109">Python</span><span class="sxs-lookup"><span data-stu-id="a5744-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="a5744-110">REST</span><span class="sxs-lookup"><span data-stu-id="a5744-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="a5744-111">Poskytovatel prostředků REST</span><span class="sxs-lookup"><span data-stu-id="a5744-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="a5744-112">SQL</span><span class="sxs-lookup"><span data-stu-id="a5744-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="a5744-113">**Stažení sady SDK**</span><span class="sxs-lookup"><span data-stu-id="a5744-113">**SDK download**</span></span></td><td>[<span data-ttu-id="a5744-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="a5744-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td><span data-ttu-id="a5744-115">**Dokumentaci k rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="a5744-115">**API documentation**</span></span></td><td>[<span data-ttu-id="a5744-116">Referenční dokumentace rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="a5744-116">.NET API reference documentation</span></span>](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="a5744-117">**Ukázky**</span><span class="sxs-lookup"><span data-stu-id="a5744-117">**Samples**</span></span></td><td>[<span data-ttu-id="a5744-118">Ukázky kódu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a5744-118">.NET code samples</span></span>](documentdb-dotnet-samples.md)</td></tr>

<tr><td><span data-ttu-id="a5744-119">**Začínáme**</span><span class="sxs-lookup"><span data-stu-id="a5744-119">**Get started**</span></span></td><td>[<span data-ttu-id="a5744-120">Začínáme s Azure Cosmos DB .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a5744-120">Get started with the Azure Cosmos DB .NET Core SDK</span></span>](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td><span data-ttu-id="a5744-121">**Kurz vývoje webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="a5744-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="a5744-122">Vývoj webových aplikací s Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a5744-122">Web application development with Azure Cosmos DB</span></span>](documentdb-dotnet-application.md)</td></tr>

<tr><td><span data-ttu-id="a5744-123">**Aktuální podporovaných prostředí**</span><span class="sxs-lookup"><span data-stu-id="a5744-123">**Current supported framework**</span></span></td><td>[<span data-ttu-id="a5744-124">Rozhraní .NET 1.6 standardní a .NET standardní 1.5</span><span class="sxs-lookup"><span data-stu-id="a5744-124">.NET Standard 1.6 and .NET Standard 1.5</span></span>](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="a5744-125">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="a5744-125">Release Notes</span></span>

<span data-ttu-id="a5744-126">Cosmos DB .NET SDK služby Azure základní má parity funkcí s nejnovější verzi [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="a5744-126">The Azure Cosmos DB .NET Core SDK has feature parity with the latest version of the [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="a5744-127">Cosmos DB .NET SDK služby Azure jádra není kompatibilní s aplikací pro univerzální platformu Windows (UWP).</span><span class="sxs-lookup"><span data-stu-id="a5744-127">The Azure Cosmos DB .NET Core SDK is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="a5744-128">Pokud vás zajímá .NET Core SDK, který podporuje aplikace UWP odeslat e-mailu [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a5744-128">If you are interested in the .NET Core SDK that does support UWP apps, send email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

### <a name="a-name150150"></a><span data-ttu-id="a5744-129"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="a5744-129"><a name="1.5.0"/>1.5.0</span></span> 

* <span data-ttu-id="a5744-130">Přidaná podpora pro PartitionKeyRangeId jako FeedOption pro vymezení výsledky dotazu a hodnotu klíče rozsahu konkrétního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5744-130">Added support for PartitionKeyRangeId as a FeedOption for scoping query results to a specific partition key range value.</span></span> 
* <span data-ttu-id="a5744-131">Přidaná podpora pro StartTime jako ChangeFeedOption zahájíte hledá změny po uplynutí této doby.</span><span class="sxs-lookup"><span data-stu-id="a5744-131">Added support for StartTime as a ChangeFeedOption to start looking for the changes after that time.</span></span> 

### <a name="a-name141141"></a><span data-ttu-id="a5744-132"><a name="1.4.1"/>1.4.1</span><span class="sxs-lookup"><span data-stu-id="a5744-132"><a name="1.4.1"/>1.4.1</span></span>

*   <span data-ttu-id="a5744-133">Byl opraven problém ve třídě JsonSerializable, která může způsobit výjimce přetečení zásobníku.</span><span class="sxs-lookup"><span data-stu-id="a5744-133">Fixed an issue in the JsonSerializable class that may cause a stack overflow exception.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="a5744-134"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="a5744-134"><a name="1.4.0"/>1.4.0</span></span>

*   <span data-ttu-id="a5744-135">Přidaná podpora pro zadání vlastní JsonSerializerSettings při vytváření instancí [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span><span class="sxs-lookup"><span data-stu-id="a5744-135">Added support for specifying custom JsonSerializerSettings while instantiating a [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span></span>

### <a name="a-name132132"></a><span data-ttu-id="a5744-136"><a name="1.3.2"/>1.3.2</span><span class="sxs-lookup"><span data-stu-id="a5744-136"><a name="1.3.2"/>1.3.2</span></span>

*   <span data-ttu-id="a5744-137">Podpora rozhraní .NET standardní 1.5 jako jeden z cílové architektury.</span><span class="sxs-lookup"><span data-stu-id="a5744-137">Supporting .NET Standard 1.5 as one of the target frameworks.</span></span>

### <a name="a-name131131"></a><span data-ttu-id="a5744-138"><a name="1.3.1"/>1.3.1</span><span class="sxs-lookup"><span data-stu-id="a5744-138"><a name="1.3.1"/>1.3.1</span></span>

*   <span data-ttu-id="a5744-139">Byl opraven problém ovlivňující x64 počítače, které nemají podporu SSE4 instrukce a throw SEHException – při spuštění dotazů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a5744-139">Fixed an issue that affected x64 machines that don’t support SSE4 instruction and throw SEHException when running Azure Cosmos DB queries.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="a5744-140"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="a5744-140"><a name="1.3.0"/>1.3.0</span></span>

*   <span data-ttu-id="a5744-141">Přidaná podpora pro novou úroveň konzistence volá ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="a5744-141">Added support for a new consistency level called ConsistentPrefix.</span></span>
*   <span data-ttu-id="a5744-142">Přidaná podpora pro dotaz metriky pro jednotlivé oddíly.</span><span class="sxs-lookup"><span data-stu-id="a5744-142">Added support for query metrics for individual partitions.</span></span>
*   <span data-ttu-id="a5744-143">Přidaná podpora pro omezení velikosti token pokračování pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="a5744-143">Added support for limiting the size of the continuation token for queries.</span></span>
*   <span data-ttu-id="a5744-144">Přidaná podpora pro podrobnější trasování pro chybné žádosti.</span><span class="sxs-lookup"><span data-stu-id="a5744-144">Added support for more detailed tracing for failed requests.</span></span>
*   <span data-ttu-id="a5744-145">Provedli jsme některé vylepšení výkonu v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="a5744-145">Made some performance improvements in the SDK.</span></span>

### <a name="a-name122122"></a><span data-ttu-id="a5744-146"><a name="1.2.2"/>1.2.2</span><span class="sxs-lookup"><span data-stu-id="a5744-146"><a name="1.2.2"/>1.2.2</span></span>

* <span data-ttu-id="a5744-147">Opravit problém, který ignoruje PartitionKey hodnota zadaná v FeedOptions pro agregační dotazy.</span><span class="sxs-lookup"><span data-stu-id="a5744-147">Fixed an issue that ignored the PartitionKey value provided in FeedOptions for aggregate queries.</span></span>
* <span data-ttu-id="a5744-148">Byl opraven problém v transparentní zpracování oddílu správy během letu střední cross-partition Order By dotazu provádění.</span><span class="sxs-lookup"><span data-stu-id="a5744-148">Fixed an issue in transparent handling of partition management during mid-flight cross-partition Order By query execution.</span></span>

### <a name="a-name121121"></a><span data-ttu-id="a5744-149"><a name="1.2.1"/>1.2.1</span><span class="sxs-lookup"><span data-stu-id="a5744-149"><a name="1.2.1"/>1.2.1</span></span>

* <span data-ttu-id="a5744-150">Byl opraven problém, který chybu způsobil zablokování v některých asynchronní rozhraní API, pokud se používá v kontextu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5744-150">Fixed an issue which caused deadlocks in some of the async APIs when used inside ASP.NET context.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="a5744-151"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="a5744-151"><a name="1.2.0"/>1.2.0</span></span>

* <span data-ttu-id="a5744-152">Opravy aby SDK pružnější automatické převzetí služeb při selhání za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="a5744-152">Fixes to make SDK more resilient to automatic failover under certain conditions.</span></span>

### <a name="a-name112112"></a><span data-ttu-id="a5744-153"><a name="1.1.2"/>1.1.2</span><span class="sxs-lookup"><span data-stu-id="a5744-153"><a name="1.1.2"/>1.1.2</span></span>

* <span data-ttu-id="a5744-154">Opravte pro problém způsobující příležitostně o výjimku WebException: vzdálený název nelze rozpoznat.</span><span class="sxs-lookup"><span data-stu-id="a5744-154">Fix for an issue that occasionally causes a WebException: The remote name could not be resolved.</span></span>
* <span data-ttu-id="a5744-155">Přidaná podpora pro přímo čtení typu dokumentu přidáním nové přetížení ReadDocumentAsync rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5744-155">Added the support for directly reading a typed document by adding new overloads to ReadDocumentAsync API.</span></span>

### <a name="a-name111111"></a><span data-ttu-id="a5744-156"><a name="1.1.1"/>1.1.1</span><span class="sxs-lookup"><span data-stu-id="a5744-156"><a name="1.1.1"/>1.1.1</span></span>

* <span data-ttu-id="a5744-157">Byla přidána podpora LINQ pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="a5744-157">Added LINQ support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="a5744-158">Opravte pro problém nevracení paměti pro objekt ConnectionPolicy způsobené použití obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="a5744-158">Fix for a memory leak issue for the ConnectionPolicy object caused by the use of event handler.</span></span>
* <span data-ttu-id="a5744-159">Oprava problému, ve kterém nebyl UpsertAttachmentAsync pracovat, když byla použita značka ETag.</span><span class="sxs-lookup"><span data-stu-id="a5744-159">Fix for an issue wherein UpsertAttachmentAsync was not working when ETag was used.</span></span>
* <span data-ttu-id="a5744-160">Oprava problému, ve kterém nebyl křížové oddílu klauzule order by dotazu pokračování práce při řazení na pole řetězce.</span><span class="sxs-lookup"><span data-stu-id="a5744-160">Fix for an issue wherein cross partition order-by query continuation was not working when sorting on string field.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="a5744-161"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="a5744-161"><a name="1.1.0"/>1.1.0</span></span>

* <span data-ttu-id="a5744-162">Přidaná podpora pro dotazy agregace (COUNT, MIN, MAX, součet a průměr).</span><span class="sxs-lookup"><span data-stu-id="a5744-162">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="a5744-163">V tématu [podporu agregace](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="a5744-163">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="a5744-164">Snížena minimální propustnosti na dělené kolekce z 10,100 RU/s na 2 500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="a5744-164">Lowered minimum throughput on partitioned collections from 10,100 RU/s to 2500 RU/s.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="a5744-165"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="a5744-165"><a name="1.0.0"/>1.0.0</span></span>

<span data-ttu-id="a5744-166">Azure Cosmos DB .NET Core SDK umožňuje vytvářet rychlé a napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) aplikace běžely na Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="a5744-166">The Azure Cosmos DB .NET Core SDK enables you to build fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps to run on Windows, Mac, and Linux.</span></span> <span data-ttu-id="a5744-167">Nejnovější verzi Azure Cosmos DB .NET Core SDK je plně [Xamarin](https://www.xamarin.com) kompatibilní a umožňuje vytvářet aplikace, které cílí na iOS, Android a Mono (Linux).</span><span class="sxs-lookup"><span data-stu-id="a5744-167">The latest release of the Azure Cosmos DB .NET Core SDK is fully [Xamarin](https://www.xamarin.com) compatible and be used to build applications that target iOS, Android, and Mono (Linux).</span></span>  

### <a name="a-name010-preview010-preview"></a><span data-ttu-id="a5744-168"><a name="0.1.0-preview"/>0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="a5744-168"><a name="0.1.0-preview"/>0.1.0-preview</span></span>

<span data-ttu-id="a5744-169">Azure Cosmos DB .NET Core Preview SDK umožňuje vytvářet rychlé a napříč platformami [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) aplikace běžely na Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="a5744-169">The Azure Cosmos DB .NET Core Preview SDK enables you to build fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps to run on Windows, Mac, and Linux.</span></span>

<span data-ttu-id="a5744-170">Cosmos DB .NET SDK služby Azure základní Preview má parity funkcí s nejnovější verzi [.NET SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) a podporuje následující:</span><span class="sxs-lookup"><span data-stu-id="a5744-170">The Azure Cosmos DB .NET Core Preview SDK has feature parity with the latest version of the [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) and supports the following:</span></span>
* <span data-ttu-id="a5744-171">Všechny [připojení režimy](performance-tips.md#networking): režim brány, přímé TCP a přímé HTTPs.</span><span class="sxs-lookup"><span data-stu-id="a5744-171">All [connection modes](performance-tips.md#networking): Gateway mode, Direct TCP, and Direct HTTPs.</span></span> 
* <span data-ttu-id="a5744-172">Všechny [úrovně konzistence](consistency-levels.md): silným, relace, typu s ohraničenou Prošlostí a Eventual.</span><span class="sxs-lookup"><span data-stu-id="a5744-172">All [consistency levels](consistency-levels.md): Strong, Session, Bounded Staleness, and Eventual.</span></span>
* <span data-ttu-id="a5744-173">[Oddíly kolekce](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="a5744-173">[Partitioned collections](partition-data.md).</span></span> 
* <span data-ttu-id="a5744-174">[Účty databáze více oblasti a geografická replikace](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="a5744-174">[Multi-region database accounts and geo-replication](distribute-data-globally.md).</span></span>

<span data-ttu-id="a5744-175">Pokud máte otázky související s touto sadou SDK, odeslání na [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), nebo problém v souboru [úložiště github](https://github.com/Azure/azure-documentdb-dotnet/issues).</span><span class="sxs-lookup"><span data-stu-id="a5744-175">If you have questions related to this SDK, post to [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), or file an issue in the [github repository](https://github.com/Azure/azure-documentdb-dotnet/issues).</span></span> 

## <a name="release--retirement-dates"></a><span data-ttu-id="a5744-176">Verze & vyřazení kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="a5744-176">Release & Retirement Dates</span></span>

| <span data-ttu-id="a5744-177">Verze</span><span class="sxs-lookup"><span data-stu-id="a5744-177">Version</span></span> | <span data-ttu-id="a5744-178">Datum vydání</span><span class="sxs-lookup"><span data-stu-id="a5744-178">Release Date</span></span> | <span data-ttu-id="a5744-179">Datum vyřazení</span><span class="sxs-lookup"><span data-stu-id="a5744-179">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="a5744-180">1.5.0</span><span class="sxs-lookup"><span data-stu-id="a5744-180">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="a5744-181">10. srpnu 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-181">August 10, 2017</span></span> |--- | 
| [<span data-ttu-id="a5744-182">1.4.1</span><span class="sxs-lookup"><span data-stu-id="a5744-182">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="a5744-183">07 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-183">August 07, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-184">1.4.0</span><span class="sxs-lookup"><span data-stu-id="a5744-184">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="a5744-185">02 srpen 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-185">August 02, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-186">1.3.2</span><span class="sxs-lookup"><span data-stu-id="a5744-186">1.3.2</span></span>](#1.3.2) |<span data-ttu-id="a5744-187">12. června 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-187">June 12, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-188">1.3.1</span><span class="sxs-lookup"><span data-stu-id="a5744-188">1.3.1</span></span>](#1.3.1) |<span data-ttu-id="a5744-189">23 může 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-189">May 23, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-190">1.3.0</span><span class="sxs-lookup"><span data-stu-id="a5744-190">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="a5744-191">10. května 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-191">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-192">1.2.2</span><span class="sxs-lookup"><span data-stu-id="a5744-192">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="a5744-193">19. dubna 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-193">April 19, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-194">1.2.1</span><span class="sxs-lookup"><span data-stu-id="a5744-194">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="a5744-195">29. března 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-195">March 29, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-196">1.2.0</span><span class="sxs-lookup"><span data-stu-id="a5744-196">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="a5744-197">25. března 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-197">March 25, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-198">1.1.2</span><span class="sxs-lookup"><span data-stu-id="a5744-198">1.1.2</span></span>](#1.1.2) |<span data-ttu-id="a5744-199">20. března 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-199">March 20, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-200">1.1.1</span><span class="sxs-lookup"><span data-stu-id="a5744-200">1.1.1</span></span>](#1.1.1) |<span data-ttu-id="a5744-201">14. března 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-201">March 14, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-202">1.1.0</span><span class="sxs-lookup"><span data-stu-id="a5744-202">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="a5744-203">16 února 2017</span><span class="sxs-lookup"><span data-stu-id="a5744-203">February 16, 2017</span></span> |--- |
| [<span data-ttu-id="a5744-204">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a5744-204">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="a5744-205">21 prosince 2016</span><span class="sxs-lookup"><span data-stu-id="a5744-205">December 21, 2016</span></span> |--- |
| [<span data-ttu-id="a5744-206">0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="a5744-206">0.1.0-preview</span></span>](#0.1.0-preview) |<span data-ttu-id="a5744-207">15. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="a5744-207">November 15, 2016</span></span> |<span data-ttu-id="a5744-208">31. prosinci 2016</span><span class="sxs-lookup"><span data-stu-id="a5744-208">December 31, 2016</span></span> |

## <a name="see-also"></a><span data-ttu-id="a5744-209">Viz také</span><span class="sxs-lookup"><span data-stu-id="a5744-209">See Also</span></span>
<span data-ttu-id="a5744-210">Další informace o Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby.</span><span class="sxs-lookup"><span data-stu-id="a5744-210">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

