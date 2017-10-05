---
title: "Požadované jednotky & odhadnout propustnost - Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak porozumět, zadejte a odhadnout požadavky na jednotky žádosti v Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7a4efc0fb9b3855b9dbbe445768ceb2a9940d0b2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="request-units-in-azure-cosmos-db"></a><span data-ttu-id="1f5c3-103">Požadované jednotky v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-103">Request Units in Azure Cosmos DB</span></span>
<span data-ttu-id="1f5c3-104">Nyní k dispozici: Azure Cosmos DB [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-104">Now available: Azure Cosmos DB [request unit calculator](https://www.documentdb.com/capacityplanner).</span></span> <span data-ttu-id="1f5c3-105">Další informace v [odhadnout, musí vaše propustnost](request-units.md#estimating-throughput-needs).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-105">Learn more in [Estimating your throughput needs](request-units.md#estimating-throughput-needs).</span></span>

![Propustnost kalkulačky][5]

## <a name="introduction"></a><span data-ttu-id="1f5c3-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="1f5c3-107">Introduction</span></span>
<span data-ttu-id="1f5c3-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je globálně distribuované databáze více modelu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is Microsoft's globally distributed multi-model database.</span></span> <span data-ttu-id="1f5c3-109">S Azure DB Cosmos nemáte pronajímat virtuálních počítačů, nasazení softwaru nebo monitorování databází.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-109">With Azure Cosmos DB, you don't have to rent virtual machines, deploy software, or monitor databases.</span></span> <span data-ttu-id="1f5c3-110">Azure Cosmos DB je provozována a průběžně monitorovat pomocí Microsoft nejvyšší technici k poskytování world třída data dostupnosti, výkonu a ochrany.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-110">Azure Cosmos DB is operated and continuously monitored by Microsoft top engineers to deliver world class availability, performance, and data protection.</span></span> <span data-ttu-id="1f5c3-111">Přistupujete k datům pomocí rozhraní API podle svého výběru jako [DocumentDB SQL](documentdb-sql-query.md) (dokumentu), MongoDB (dokumentu), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (klíč hodnota), a [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graf) jsou všechny nativně podporuje.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-111">You can access your data using APIs of your choice, as [DocumentDB SQL](documentdb-sql-query.md) (document), MongoDB (document), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (key-value), and [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graph) are all natively supported.</span></span> <span data-ttu-id="1f5c3-112">Měna Azure Cosmos DB je jednotka žádosti (RU).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-112">The currency of Azure Cosmos DB is the Request Unit (RU).</span></span> <span data-ttu-id="1f5c3-113">S RUs není potřeba rezervovat kapacity pro čtení a zápis nebo přidělení procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-113">With RUs, you do not need to reserve read/write capacities or provision CPU, Memory and IOPS.</span></span>

<span data-ttu-id="1f5c3-114">Azure Cosmos DB podporuje několik rozhraní API s různé operace, od jednoduchého čte a zapisuje do grafu komplexní dotazy.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-114">Azure Cosmos DB supports a number of APIs with different operations ranging from simple reads and writes to complex graph queries.</span></span> <span data-ttu-id="1f5c3-115">Vzhledem k tomu, že ne všechny požadavky jsou stejné, jsou přiřazeny normalizovaný objemu **požadované jednotky** založenou na velikosti výpočty potřebné k požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-115">Since not all requests are equal, they are assigned a normalized quantity of **request units** based on the amount of computation required to serve the request.</span></span> <span data-ttu-id="1f5c3-116">Počet jednotek žádosti operace je deterministická, a můžete sledovat počet jednotek žádosti spotřebovávají všechny operace v Azure Cosmos DB prostřednictvím hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-116">The number of request units for an operation is deterministic, and you can track the number of request units consumed by any operation in Azure Cosmos DB via a response header.</span></span> 

<span data-ttu-id="1f5c3-117">Zajistit předvídatelný výkon, budete muset rezervovat propustnost v jednotkách 100 RU za sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-117">To provide predictable performance, you need to reserve throughput in units of 100 RU/second.</span></span> 

<span data-ttu-id="1f5c3-118">Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-118">After reading this article, you'll be able to answer the following questions:</span></span>  

* <span data-ttu-id="1f5c3-119">Jaké jsou požadované jednotky a požádat o poplatky?</span><span class="sxs-lookup"><span data-stu-id="1f5c3-119">What are request units and request charges?</span></span>
* <span data-ttu-id="1f5c3-120">Jak určit kapacitu jednotky žádosti pro kolekci?</span><span class="sxs-lookup"><span data-stu-id="1f5c3-120">How do I specify request unit capacity for a collection?</span></span>
* <span data-ttu-id="1f5c3-121">Jak odhadnout, že je jednotka žádosti Moje aplikace?</span><span class="sxs-lookup"><span data-stu-id="1f5c3-121">How do I estimate my application's request unit needs?</span></span>
* <span data-ttu-id="1f5c3-122">Co se stane, když I překročit kapacitu jednotky žádosti pro kolekci?</span><span class="sxs-lookup"><span data-stu-id="1f5c3-122">What happens if I exceed request unit capacity for a collection?</span></span>

<span data-ttu-id="1f5c3-123">Jak Azure Cosmos DB je více modelu databáze, je důležité si uvědomit, že bude označujeme kolekce či dokumentu pro dokument rozhraní API, grafu nebo uzel pro graph API a tabulka/entity pro rozhraní API tabulky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-123">As Azure Cosmos DB is a multi-model database, it is important to note that we will refer to a collection/document for a document API, a graph/node for a graph API and a table/entity for table API.</span></span> <span data-ttu-id="1f5c3-124">Propustnost tohoto dokumentu jsme se generalize Principy kontejneru nebo položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-124">Throughput this document we will generalize to the concepts of container/item.</span></span>

## <a name="request-units-and-request-charges"></a><span data-ttu-id="1f5c3-125">Jednotek žádosti a poplatky požadavku</span><span class="sxs-lookup"><span data-stu-id="1f5c3-125">Request units and request charges</span></span>
<span data-ttu-id="1f5c3-126">Azure Cosmos DB poskytuje rychlé, předvídatelný výkon pomocí *rezervování* prostředky pro uspokojení musí propustnost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-126">Azure Cosmos DB delivers fast, predictable performance by *reserving* resources to satisfy your application's throughput needs.</span></span>  <span data-ttu-id="1f5c3-127">Vzhledem k aplikaci načíst a přístup k vzory změny v čase, Azure Cosmos DB umožňuje snadno zvýšit nebo snížit množství vyhrazenou propustností, které jsou k dispozici pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-127">Because application load and access patterns change over time, Azure Cosmos DB allows you to easily increase or decrease the amount of reserved throughput available to your application.</span></span>

<span data-ttu-id="1f5c3-128">S Azure Cosmos databáze je zadána vyhrazenou propustností z hlediska jednotek žádosti zpracování za sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-128">With Azure Cosmos DB, reserved throughput is specified in terms of request units processing per second.</span></span> <span data-ttu-id="1f5c3-129">Si můžete představit jednotek žádosti jako měnu propustnost, které jste *rezervovat* množství jednotek zaručenou žádosti, které jsou k dispozici pro aplikaci na základě za sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-129">You can think of request units as throughput currency, whereby you *reserve* an amount of guaranteed request units available to your application on per second basis.</span></span>  <span data-ttu-id="1f5c3-130">Každé operace v Azure DB Cosmos - zápis dokumentu, provádění dotazu, aktualizace dokumentu - spotřebuje procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-130">Each operation in Azure Cosmos DB - writing a document, performing a query, updating a document - consumes CPU, memory, and IOPS.</span></span>  <span data-ttu-id="1f5c3-131">To znamená, každou operaci způsobuje *požadavku poplatků*, vyjádřeného v *požadované jednotky*.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-131">That is, each operation incurs a *request charge*, which is expressed in *request units*.</span></span>  <span data-ttu-id="1f5c3-132">Principy faktory, což ovlivňuje poplatky jednotek žádosti, společně s požadavky na propustnost vaší aplikace, umožňuje aplikaci spustit jako efektivně možné náklady.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-132">Understanding the factors which impact request unit charges, along with your application's throughput requirements, enables you to run your application as cost effectively as possible.</span></span> <span data-ttu-id="1f5c3-133">Průzkumník dotazů je také skvělý nástroj pro testování základní dotazu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-133">The query explorer is also a wonderful tool to test the core of a query.</span></span>

<span data-ttu-id="1f5c3-134">Doporučujeme začít následujícím videem, kde vysvětluje Aravind Ramachandran jednotek žádosti a předvídatelného výkonu s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-134">We recommend getting started by watching the following video, where Aravind Ramachandran explains request units and predictable performance with Azure Cosmos DB.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a><span data-ttu-id="1f5c3-135">Určení požadavku jednotka kapacity v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-135">Specifying request unit capacity in Azure Cosmos DB</span></span>
<span data-ttu-id="1f5c3-136">Při spouštění novou kolekci, tabulka nebo graf, je třeba zadat počet jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-136">When starting a new collection, table or graph, you specify the number of request units per second (RU per second) you want reserved.</span></span> <span data-ttu-id="1f5c3-137">Na základě zřízené propustnosti, Azure Cosmos DB přiděluje fyzické oddíly pro hostování vaší kolekce a rozdělení/rebalances dat napříč oddíly ho s růstem.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-137">Based on the provisioned throughput, Azure Cosmos DB allocates physical partitions to host your collection and splits/rebalances data across partitions as it grows.</span></span>

<span data-ttu-id="1f5c3-138">Azure Cosmos DB vyžaduje klíč oddílu na zadat, když je kolekce s 2 500 jednotek žádosti přiděleným nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-138">Azure Cosmos DB requires a partition key to be specified when a collection is provisioned with 2,500 request units or higher.</span></span> <span data-ttu-id="1f5c3-139">Klíč oddílu je taky požadovat, aby v budoucnu škálování propustnost vaší kolekce nad rámec odpovídající 2500 jednotek žádosti.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-139">A partition key is also required to scale your collection's throughput beyond 2,500 request units in the future.</span></span> <span data-ttu-id="1f5c3-140">Proto důrazně doporučujeme nakonfigurovat [klíč oddílu](partition-data.md) při vytváření kontejneru bez ohledu na počáteční propustnosti.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-140">Therefore, it is highly recommended to configure a [partition key](partition-data.md) when creating a container regardless of your initial throughput.</span></span> <span data-ttu-id="1f5c3-141">Vzhledem k tomu, aby se daly rozdělit mezi více oddílů mohou mít vaše data, je nutné vybrat klíč oddílu, který má vysokou kardinalitou (100 na miliony jedinečných hodnot), aby kolekce, tabulka nebo graf a žádostí je možné rozšířit jednotně pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-141">Since your data might have to be split across multiple partitions, it is necessary to pick a partition key that has a high cardinality (100 to millions of distinct values) so that your collection/table/graph and requests can be scaled uniformly by Azure Cosmos DB.</span></span> 

> [!NOTE]
> <span data-ttu-id="1f5c3-142">Klíč oddílu je logické hranice a není fyzický jeden.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-142">A partition key is a logical boundary, and not a physical one.</span></span> <span data-ttu-id="1f5c3-143">Proto není potřeba omezit počet hodnoty klíče jedinečné oddílu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-143">Therefore, you do not need to limit the number of distinct partition key values.</span></span> <span data-ttu-id="1f5c3-144">Ve skutečnosti je lepší má více jedinečných hodnot klíče oddílu menší, než databázi Cosmos Azure má další možnosti vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-144">It is in fact better to have more distinct partition key values than less, as Azure Cosmos DB has more load balancing options.</span></span>

<span data-ttu-id="1f5c3-145">Zde je fragment kódu pro vytvoření kolekce s 3 000 jednotek žádosti za druhé pomocí sady .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-145">Here is a code snippet for creating a collection with 3,000 request units per second using the .NET SDK:</span></span>

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

<span data-ttu-id="1f5c3-146">Azure Cosmos DB funguje na rezervace modelu na propustnost.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-146">Azure Cosmos DB operates on a reservation model on throughput.</span></span> <span data-ttu-id="1f5c3-147">To znamená, že se účtují pro množství propustnost *vyhrazené*, bez ohledu na to, kolik z této propustnost je aktivně *používá*.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-147">That is, you are billed for the amount of throughput *reserved*, regardless of how much of that throughput is actively *used*.</span></span> <span data-ttu-id="1f5c3-148">Jako vaše aplikace je zatížení, data a využití vzory změnu, je možné snadno škálovat nahoru a dolů množství vyhrazené RUs prostřednictvím sady SDK nebo pomocí [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-148">As your application's load, data, and usage patterns change you can easily scale up and down the amount of reserved RUs through SDKs or using the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="1f5c3-149">Každý kolekce a tabulka/grafika jsou namapované na `Offer` prostředků v Azure DB Cosmos, který má metadata o zřízené propustnosti.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-149">Each collection/table/graph are mapped to an `Offer` resource in Azure Cosmos DB, which has metadata about the provisioned throughput.</span></span> <span data-ttu-id="1f5c3-150">Vyhledávání odpovídající prostředek nabídka pro kontejner a poté aktualizace pomocí novou hodnotu propustnosti, můžete změnit přidělené propustnost.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-150">You can change the allocated throughput by looking up the corresponding offer resource for a container, then updating it with the new throughput value.</span></span> <span data-ttu-id="1f5c3-151">Zde je fragment kódu pro změnu propustnost kolekce do 5 000 jednotek žádosti za druhé pomocí sady .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-151">Here is a code snippet for changing the throughput of a collection to 5,000 request units per second using the .NET SDK:</span></span>

```csharp
// Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="1f5c3-152">Neexistuje žádný vliv na dostupnost vaší kontejneru při změně propustnost.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-152">There is no impact to the availability of your container when you change the throughput.</span></span> <span data-ttu-id="1f5c3-153">Nové vyhrazenou propustností je obvykle efektivní během několika sekund na použití nové propustnost.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-153">Typically the new reserved throughput is effective within seconds on application of the new throughput.</span></span>

## <a name="request-unit-considerations"></a><span data-ttu-id="1f5c3-154">Aspekty jednotek žádosti</span><span class="sxs-lookup"><span data-stu-id="1f5c3-154">Request unit considerations</span></span>
<span data-ttu-id="1f5c3-155">Při odhadování počet jednotek žádosti můžete vyhradit pro váš kontejner Azure Cosmos DB, je důležité vzít v úvahu následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-155">When estimating the number of request units to reserve for your Azure Cosmos DB container, it is important to take the following variables into consideration:</span></span>

* <span data-ttu-id="1f5c3-156">**Velikost položky**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-156">**Item size**.</span></span> <span data-ttu-id="1f5c3-157">Jak roste množství jednotek použití číst nebo zapisovat data také zvýší.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-157">As size increases the units consumed to read or write the data will also increase.</span></span>
* <span data-ttu-id="1f5c3-158">**Počet vlastností položky**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-158">**Item property count**.</span></span> <span data-ttu-id="1f5c3-159">V případě indexování výchozí všech vlastností, jednotek použití k zápisu dokumentu nebo uzel nebo ntity zvýší jako zvyšuje počet vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-159">Assuming default indexing of all properties, the units consumed to write a document/node/ntity will increase as the property count increases.</span></span>
* <span data-ttu-id="1f5c3-160">**Konzistenci dat**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-160">**Data consistency**.</span></span> <span data-ttu-id="1f5c3-161">Při použití úrovně konzistence dat silného nebo typu s ohraničenou Prošlostí, budou další jednotky pro čtení položek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-161">When using data consistency levels of Strong or Bounded Staleness, additional units will be consumed to read items.</span></span>
* <span data-ttu-id="1f5c3-162">**Indexované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-162">**Indexed properties**.</span></span> <span data-ttu-id="1f5c3-163">Zásadu indexu na každý kontejner určuje vlastnosti, které jsou uloženy ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-163">An index policy on each container determines which properties are indexed by default.</span></span> <span data-ttu-id="1f5c3-164">Omezení počtu indexované vlastnosti nebo povolením Opožděné indexování můžete snížit spotřebu jednotky vaší žádosti.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-164">You can reduce your request unit consumption by limiting the number of indexed properties or by enabling lazy indexing.</span></span>
* <span data-ttu-id="1f5c3-165">**Indexování dokumentů**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-165">**Document indexing**.</span></span> <span data-ttu-id="1f5c3-166">Ve výchozím nastavení je každá položka automaticky indexovaný bude využívat méně jednotek žádosti, pokud se rozhodnete indexování některých položek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-166">By default each item is automatically indexed, you will consume fewer request units if you choose not to index some of your items.</span></span>
* <span data-ttu-id="1f5c3-167">**Dotaz vzory**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-167">**Query patterns**.</span></span> <span data-ttu-id="1f5c3-168">Složitost dotazu má dopad na tom, kolik jednotek žádosti se spotřebovávají pro operace.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-168">The complexity of a query impacts how many Request Units are consumed for an operation.</span></span> <span data-ttu-id="1f5c3-169">Počet predikáty, povaha predikáty, projekce, počet UDF a velikost datové sady zdroje, které jsou všechny ovlivnit náklady na operace dotazů.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-169">The number of predicates, nature of the predicates, projections, number of UDFs, and the size of the source data set all influence the cost of query operations.</span></span>
* <span data-ttu-id="1f5c3-170">**Použití skriptu**.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-170">**Script usage**.</span></span>  <span data-ttu-id="1f5c3-171">Stejně jako u dotazů, využívat jednotek žádosti podle složitosti operací během provádění uložené procedury a triggery.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-171">As with queries, stored procedures and triggers consume request units based on the complexity of the operations being performed.</span></span> <span data-ttu-id="1f5c3-172">Když budete vyvíjet aplikace, zkontrolujte hlavičky požadavku poplatků abyste lépe pochopili, jak každou operaci spotřebovává požadavek jednotky kapacity.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-172">As you develop your application, inspect the request charge header to better understand how each operation is consuming request unit capacity.</span></span>

## <a name="estimating-throughput-needs"></a><span data-ttu-id="1f5c3-173">Odhad potřeb propustnost</span><span class="sxs-lookup"><span data-stu-id="1f5c3-173">Estimating throughput needs</span></span>
<span data-ttu-id="1f5c3-174">Jednotka žádosti je normalizovaný míru náklady na zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-174">A request unit is a normalized measure of request processing cost.</span></span> <span data-ttu-id="1f5c3-175">Jednotka jedné žádosti představuje kapacity zpracování požadovaná pro čtení (prostřednictvím id nebo vlastní odkaz) jeden 1KB položky skládající se z 10 jedinečnou vlastnost hodnot (s výjimkou vlastnosti systému).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-175">A single request unit represents the processing capacity required to read (via self link or id) a single 1KB item consisting of 10 unique property values (excluding system properties).</span></span> <span data-ttu-id="1f5c3-176">Požadavek na vytvoření (Vložit), nahraďte nebo odstranění stejnou položku spotřebuje další zpracování ze služby a tím více jednotek žádosti.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-176">A request to create (insert), replace or delete the same item will consume more processing from the service and thereby more request units.</span></span>   

> [!NOTE]
> <span data-ttu-id="1f5c3-177">Směrný plán pro 1KB požadavků 1 jednotka položky odpovídá jednoduché GET vlastní odkaz nebo id položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-177">The baseline of 1 request unit for a 1KB item corresponds to a simple GET by self link or id of the item.</span></span>
> 
> 

<span data-ttu-id="1f5c3-178">Například je zde tabulku, která zobrazí počet jednotek žádosti zřídit na tři různé položky velikosti (1KB, 4KB a 64KB) a na dvou různých výkonu úrovních (500 čtení za sekundu + 100 zápisů za sekundu a 500 čtení za sekundu + 500 zápisů za sekundu).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-178">For example, here's a table that shows how many request units to provision at three different item sizes (1KB, 4KB, and 64KB) and at two different performance levels (500 reads/second + 100 writes/second and 500 reads/second + 500 writes/second).</span></span> <span data-ttu-id="1f5c3-179">Konzistenci dat byl nakonfigurován v relaci a zásady indexování byla nastavena na hodnotu None.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-179">The data consistency was configured at Session, and the indexing policy was set to None.</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-180"><strong>Velikost položky</strong></span><span class="sxs-lookup"><span data-stu-id="1f5c3-180"><strong>Item size</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-181"><strong>Čtení za sekundu</strong></span><span class="sxs-lookup"><span data-stu-id="1f5c3-181"><strong>Reads/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-182"><strong>Zápisů za sekundu</strong></span><span class="sxs-lookup"><span data-stu-id="1f5c3-182"><strong>Writes/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-183"><strong>Jednotky žádostí</strong></span><span class="sxs-lookup"><span data-stu-id="1f5c3-183"><strong>Request units</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-184">1 KB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-184">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-185">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-185">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-186">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-186">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-187">(500 * 1) + (100 * 5) = 1 000 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-187">(500 * 1) + (100 * 5) = 1,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-188">1 KB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-188">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-189">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-189">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-190">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-190">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-191">(500 * 1) + (500 * 5) = 3000 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-191">(500 * 1) + (500 * 5) = 3,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-192">4 KB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-192">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-193">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-193">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-194">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-194">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-195">(500 * 1,3) + (100 * 7) = 1,350 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-196">4 KB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-196">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-197">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-197">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-198">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-198">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-199">(500 * 1,3) + (500 * 7) = 4,150 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-200">64 kB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-200">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-201">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-201">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-202">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-202">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1f5c3-204">64 kB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-204">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-205">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-205">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-206">500</span><span class="sxs-lookup"><span data-stu-id="1f5c3-206">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1f5c3-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span><span class="sxs-lookup"><span data-stu-id="1f5c3-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span></span></p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a><span data-ttu-id="1f5c3-208">Použití kalkulačky jednotek žádosti</span><span class="sxs-lookup"><span data-stu-id="1f5c3-208">Use the request unit calculator</span></span>
<span data-ttu-id="1f5c3-209">Pro zákazníky, dobře ladit jejich odhady propustnost, je web, na základě [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner) ke zjištění přibližné hodnoty požadované žádosti jednotky pro typická operace, včetně:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-209">To help customers fine tune their throughput estimations, there is a web based [request unit calculator](https://www.documentdb.com/capacityplanner) to help estimate the request unit requirements for typical operations, including:</span></span>

* <span data-ttu-id="1f5c3-210">Vytvoří položku (zápisy)</span><span class="sxs-lookup"><span data-stu-id="1f5c3-210">Item creates (writes)</span></span>
* <span data-ttu-id="1f5c3-211">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-211">Item reads</span></span>
* <span data-ttu-id="1f5c3-212">Odstranění položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-212">Item deletes</span></span>
* <span data-ttu-id="1f5c3-213">Aktualizace položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-213">Item updates</span></span>

<span data-ttu-id="1f5c3-214">Nástroj zahrnuje taky podporu odhadnout požadavky na úložiště dat podle ukázkové položky, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-214">The tool also includes support for estimating data storage needs based on the sample items you provide.</span></span>

<span data-ttu-id="1f5c3-215">Pomocí nástroje je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-215">Using the tool is simple:</span></span>

1. <span data-ttu-id="1f5c3-216">Nahrajte jednu nebo více reprezentativní položek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-216">Upload one or more representative items.</span></span>
   
    ![Nahrání položky do kalkulačky jednotek žádosti][2]
2. <span data-ttu-id="1f5c3-218">Chcete-li odhadnout požadavky na úložiště dat, zadejte celkový počet položek, které chcete uložit.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-218">To estimate data storage requirements, enter the total number of items you expect to store.</span></span>
3. <span data-ttu-id="1f5c3-219">Zadejte počet položek, které vytvářet, číst, aktualizovat a odstranit operations vyžadují (na základě za sekundu).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-219">Enter the number of items create, read, update, and delete operations you require (on a per-second basis).</span></span> <span data-ttu-id="1f5c3-220">K zjištění přibližné hodnoty poplatky jednotek žádosti operací aktualizace položky, nahrajte kopii ukázkové položky z kroku 1 výše, zahrnuje typické pole aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-220">To estimate the request unit charges of item update operations, upload a copy of the sample item from step 1 above that includes typical field updates.</span></span>  <span data-ttu-id="1f5c3-221">Například pokud položku aktualizace obvykle upravit dvě vlastnosti s názvem lastLogin a userVisits, pak jednoduše zkopírovat ukázkové položky, aktualizujte hodnoty pro tyto dvě vlastnosti a nahrát kopírovaných položek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-221">For example, if item updates typically modify two properties named lastLogin and userVisits, then simply copy the sample item, update the values for those two properties, and upload the copied item.</span></span>
   
    ![Zadejte požadavky na propustnost v kalkulačky jednotek žádosti][3]
4. <span data-ttu-id="1f5c3-223">Klikněte na tlačítko Vypočítat a podívejte se na výsledky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-223">Click calculate and examine the results.</span></span>
   
    ![Žádosti o výsledky kalkulačky jednotky][4]

> [!NOTE]
> <span data-ttu-id="1f5c3-225">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a počtu indexované vlastnosti, nahrajte vzorek každého *typ* z typických položky do nástroje a potom vypočítat výsledky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-225">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then upload a sample of each *type* of typical item to the tool and then calculate the results.</span></span>
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a><span data-ttu-id="1f5c3-226">Použití hlavičku odpovědi Azure Cosmos DB požadavek zdarma</span><span class="sxs-lookup"><span data-stu-id="1f5c3-226">Use the Azure Cosmos DB request charge response header</span></span>
<span data-ttu-id="1f5c3-227">Každou odpověď ze služby Azure Cosmos DB obsahuje vlastní hlavičky (`x-ms-request-charge`) obsahující jednotek žádosti využité pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-227">Every response from the Azure Cosmos DB service includes a custom header (`x-ms-request-charge`) that contains the request units consumed for the request.</span></span> <span data-ttu-id="1f5c3-228">Tuto hlavičku je také přístupné prostřednictvím sady SDK Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-228">This header is also accessible through the Azure Cosmos DB SDKs.</span></span> <span data-ttu-id="1f5c3-229">V sadě SDK .NET je RequestCharge vlastnost ResourceResponse objektu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-229">In the .NET SDK, RequestCharge is a property of the ResourceResponse object.</span></span>  <span data-ttu-id="1f5c3-230">Pro dotazy Průzkumník Azure Cosmos DB dotazů na portálu Azure poskytuje informace poplatků požadavku pro spuštění dotazů.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-230">For queries, the Azure Cosmos DB Query Explorer in the Azure portal provides request charge information for executed queries.</span></span>

![Zkoumání RU poplatky v Průzkumníku dotazu][1]

<span data-ttu-id="1f5c3-232">Myslete na to je záznam zřizování jednotky žádosti přidružené spuštěná typických operací proti položku reprezentativní používá vaše aplikace a pak odhadnout jedné metody odhadnout velikost vyhrazenou propustností požadované aplikací počet operací, které předpokládáte provádění každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-232">With this in mind, one method for estimating the amount of reserved throughput required by your application is to record the request unit charge associated with running typical operations against a representative item used by your application and then estimating the number of operations you anticipate performing each second.</span></span>  <span data-ttu-id="1f5c3-233">Nezapomeňte měřit a zahrnují typické dotazy a také při použití skriptu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-233">Be sure to measure and include typical queries and Azure Cosmos DB script usage as well.</span></span>

> [!NOTE]
> <span data-ttu-id="1f5c3-234">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a počtu indexované vlastnosti, potom si poznamenejte poplatků jednotek žádosti příslušné operace spojené s každou *typ* typické položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-234">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then record the applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

<span data-ttu-id="1f5c3-235">Například:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-235">For example:</span></span>

1. <span data-ttu-id="1f5c3-236">Zaznamenejte poplatků jednotek žádosti o vytvoření (vkládání) typické položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-236">Record the request unit charge of creating (inserting) a typical item.</span></span> 
2. <span data-ttu-id="1f5c3-237">Záznam poplatků jednotek žádosti o čtení typické položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-237">Record the request unit charge of reading a typical item.</span></span>
3. <span data-ttu-id="1f5c3-238">Záznam poplatků jednotek žádosti aktualizace typické položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-238">Record the request unit charge of updating a typical item.</span></span>
4. <span data-ttu-id="1f5c3-239">Záznam poplatků jednotek žádosti typické, běžné položky dotazů.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-239">Record the request unit charge of typical, common item queries.</span></span>
5. <span data-ttu-id="1f5c3-240">Zaznamenejte poplatků jednotek žádosti vlastních skriptů (uložené procedury, triggery, funkce definované uživatelem), využít aplikací</span><span class="sxs-lookup"><span data-stu-id="1f5c3-240">Record the request unit charge of any custom scripts (stored procedures, triggers, user-defined functions) leveraged by the application</span></span>
6. <span data-ttu-id="1f5c3-241">Vypočítejte jednotky požadované žádosti dané odhadovaný počet operací, které předpokládáte spouštět každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-241">Calculate the required request units given the estimated number of operations you anticipate to run each second.</span></span>

### <span data-ttu-id="1f5c3-242"><a id="GetLastRequestStatistics"></a>Použití rozhraní API pro příkaz GetLastRequestStatistics pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-242"><a id="GetLastRequestStatistics"></a>Use API for MongoDB's GetLastRequestStatistics command</span></span>
<span data-ttu-id="1f5c3-243">Rozhraní API pro MongoDB podporuje vlastního příkazu *getLastRequestStatistics*, pro načítání poplatků požadavku pro zadané operace.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-243">API for MongoDB supports a custom command, *getLastRequestStatistics*, for retrieving the request charge for specified operations.</span></span>

<span data-ttu-id="1f5c3-244">Například v prostředí Mongo provést operaci chcete ověřit žádost zdarma pro.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-244">For example, in the Mongo Shell, execute the operation you want to verify the request charge for.</span></span>
```
> db.sample.find()
```

<span data-ttu-id="1f5c3-245">Potom spusťte příkaz *getLastRequestStatistics*.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-245">Next, execute the command *getLastRequestStatistics*.</span></span>
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

<span data-ttu-id="1f5c3-246">Myslete na to je záznam zřizování jednotky žádosti přidružené spuštěná typických operací proti položku reprezentativní používá vaše aplikace a pak odhadnout jedné metody odhadnout velikost vyhrazenou propustností požadované aplikací počet operací, které předpokládáte provádění každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-246">With this in mind, one method for estimating the amount of reserved throughput required by your application is to record the request unit charge associated with running typical operations against a representative item used by your application and then estimating the number of operations you anticipate performing each second.</span></span>

> [!NOTE]
> <span data-ttu-id="1f5c3-247">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a počtu indexované vlastnosti, potom si poznamenejte poplatků jednotek žádosti příslušné operace spojené s každou *typ* typické položky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-247">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then record the applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a><span data-ttu-id="1f5c3-248">Použít rozhraní API pro portál metriky pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-248">Use API for MongoDB's portal metrics</span></span>
<span data-ttu-id="1f5c3-249">Nejjednodušší způsob, jak získat dobrý odhad požadavku poplatky jednotky pro vaše rozhraní API pro databázi MongoDB má použít [portál Azure](https://portal.azure.com) metriky.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-249">The simplest way to get a good estimation of request unit charges for your API for MongoDB database is to use the [Azure portal](https://portal.azure.com) metrics.</span></span> <span data-ttu-id="1f5c3-250">S *počet požadavků, které* a *požadavek poplatků* grafy, můžete získat odhad, kolik jednotek žádosti ze každý je náročné operace a kolik jednotek žádosti, které budou využívat relativně k jinému.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-250">With the *Number of requests* and *Request Charge* charts, you can get an estimation of how many request units each operation is consuming and how many request units they consume relative to one another.</span></span>

![Rozhraní API pro MongoDB portálu metriky][6]

## <a name="a-request-unit-estimation-example"></a><span data-ttu-id="1f5c3-252">V příkladu odhad jednotek žádosti</span><span class="sxs-lookup"><span data-stu-id="1f5c3-252">A request unit estimation example</span></span>
<span data-ttu-id="1f5c3-253">Vezměte v úvahu následující ~ 1KB dokumentu:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-253">Consider the following ~1KB document:</span></span>

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> <span data-ttu-id="1f5c3-254">Dokumenty jsou minifikovaný v Azure Cosmos DB, takže systém vypočítat velikost dokumentu výše je něco menší než 1 KB.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-254">Documents are minified in Azure Cosmos DB, so the system calculated size of the document above is slightly less than 1KB.</span></span>
> 
> 

<span data-ttu-id="1f5c3-255">Následující tabulka uvádí přibližnou požadavek jednotky poplatky za typických operací u této položky (zdarma jednotky přibližnou požadavek předpokládá, že je úroveň konzistence účtu nastavená na "Relace" a automaticky indexování všech položek):</span><span class="sxs-lookup"><span data-stu-id="1f5c3-255">The following table shows approximate request unit charges for typical operations on this item (the approximate request unit charge assumes that the account consistency level is set to “Session” and that all items are automatically indexed):</span></span>

| <span data-ttu-id="1f5c3-256">Operace</span><span class="sxs-lookup"><span data-stu-id="1f5c3-256">Operation</span></span> | <span data-ttu-id="1f5c3-257">Žádost o jednotky zdarma</span><span class="sxs-lookup"><span data-stu-id="1f5c3-257">Request Unit Charge</span></span> |
| --- | --- |
| <span data-ttu-id="1f5c3-258">Vytvoření položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-258">Create item</span></span> |<span data-ttu-id="1f5c3-259">~ 15 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-259">~15 RU</span></span> |
| <span data-ttu-id="1f5c3-260">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-260">Read item</span></span> |<span data-ttu-id="1f5c3-261">~ 1 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-261">~1 RU</span></span> |
| <span data-ttu-id="1f5c3-262">Dotaz položky podle id</span><span class="sxs-lookup"><span data-stu-id="1f5c3-262">Query item by id</span></span> |<span data-ttu-id="1f5c3-263">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-263">~2.5 RU</span></span> |

<span data-ttu-id="1f5c3-264">Kromě toho tato tabulka ukazuje přibližnou požadavek poplatky jednotky pro typické dotazy použitou v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-264">Additionally, this table shows approximate request unit charges for typical queries used in the application:</span></span>

| <span data-ttu-id="1f5c3-265">Dotaz</span><span class="sxs-lookup"><span data-stu-id="1f5c3-265">Query</span></span> | <span data-ttu-id="1f5c3-266">Žádost o jednotky zdarma</span><span class="sxs-lookup"><span data-stu-id="1f5c3-266">Request Unit Charge</span></span> | <span data-ttu-id="1f5c3-267">počet vrácených položek</span><span class="sxs-lookup"><span data-stu-id="1f5c3-267"># of Returned Items</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1f5c3-268">Vyberte jídlo podle id</span><span class="sxs-lookup"><span data-stu-id="1f5c3-268">Select food by id</span></span> |<span data-ttu-id="1f5c3-269">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-269">~2.5 RU</span></span> |<span data-ttu-id="1f5c3-270">1</span><span class="sxs-lookup"><span data-stu-id="1f5c3-270">1</span></span> |
| <span data-ttu-id="1f5c3-271">Vyberte potravin podle výrobce</span><span class="sxs-lookup"><span data-stu-id="1f5c3-271">Select foods by manufacturer</span></span> |<span data-ttu-id="1f5c3-272">~ 7 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-272">~7 RU</span></span> |<span data-ttu-id="1f5c3-273">7</span><span class="sxs-lookup"><span data-stu-id="1f5c3-273">7</span></span> |
| <span data-ttu-id="1f5c3-274">Vyberte skupiny jídlo a pořadí podle váhy</span><span class="sxs-lookup"><span data-stu-id="1f5c3-274">Select by food group and order by weight</span></span> |<span data-ttu-id="1f5c3-275">~ 70 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-275">~70 RU</span></span> |<span data-ttu-id="1f5c3-276">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-276">100</span></span> |
| <span data-ttu-id="1f5c3-277">Vyberte nejvyšší 10 potravin ve skupině jídlo</span><span class="sxs-lookup"><span data-stu-id="1f5c3-277">Select top 10 foods in a food group</span></span> |<span data-ttu-id="1f5c3-278">~ 10 RU</span><span class="sxs-lookup"><span data-stu-id="1f5c3-278">~10 RU</span></span> |<span data-ttu-id="1f5c3-279">10</span><span class="sxs-lookup"><span data-stu-id="1f5c3-279">10</span></span> |

> [!NOTE]
> <span data-ttu-id="1f5c3-280">Poplatky za RU lišit v závislosti na počet vrácených položek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-280">RU charges vary based on the number of items returned.</span></span>
> 
> 

<span data-ttu-id="1f5c3-281">Pomocí těchto informací jsme odhadnout požadavky na RU pro tuto aplikaci zadaný počet operací a dotazy že Očekáváme, že za sekundu:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-281">With this information, we can estimate the RU requirements for this application given the number of operations and queries we expect per second:</span></span>

| <span data-ttu-id="1f5c3-282">Operace nebo dotazu</span><span class="sxs-lookup"><span data-stu-id="1f5c3-282">Operation/Query</span></span> | <span data-ttu-id="1f5c3-283">Očekávaný počet za sekundu</span><span class="sxs-lookup"><span data-stu-id="1f5c3-283">Estimated number per second</span></span> | <span data-ttu-id="1f5c3-284">Požadované RUs</span><span class="sxs-lookup"><span data-stu-id="1f5c3-284">Required RUs</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1f5c3-285">Vytvoření položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-285">Create item</span></span> |<span data-ttu-id="1f5c3-286">10</span><span class="sxs-lookup"><span data-stu-id="1f5c3-286">10</span></span> |<span data-ttu-id="1f5c3-287">150</span><span class="sxs-lookup"><span data-stu-id="1f5c3-287">150</span></span> |
| <span data-ttu-id="1f5c3-288">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-288">Read item</span></span> |<span data-ttu-id="1f5c3-289">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-289">100</span></span> |<span data-ttu-id="1f5c3-290">100</span><span class="sxs-lookup"><span data-stu-id="1f5c3-290">100</span></span> |
| <span data-ttu-id="1f5c3-291">Vyberte potravin podle výrobce</span><span class="sxs-lookup"><span data-stu-id="1f5c3-291">Select foods by manufacturer</span></span> |<span data-ttu-id="1f5c3-292">25</span><span class="sxs-lookup"><span data-stu-id="1f5c3-292">25</span></span> |<span data-ttu-id="1f5c3-293">175</span><span class="sxs-lookup"><span data-stu-id="1f5c3-293">175</span></span> |
| <span data-ttu-id="1f5c3-294">Vyberte jídlo skupinou</span><span class="sxs-lookup"><span data-stu-id="1f5c3-294">Select by food group</span></span> |<span data-ttu-id="1f5c3-295">10</span><span class="sxs-lookup"><span data-stu-id="1f5c3-295">10</span></span> |<span data-ttu-id="1f5c3-296">700</span><span class="sxs-lookup"><span data-stu-id="1f5c3-296">700</span></span> |
| <span data-ttu-id="1f5c3-297">Vyberte nejvyšší 10</span><span class="sxs-lookup"><span data-stu-id="1f5c3-297">Select top 10</span></span> |<span data-ttu-id="1f5c3-298">15</span><span class="sxs-lookup"><span data-stu-id="1f5c3-298">15</span></span> |<span data-ttu-id="1f5c3-299">Celkem 150</span><span class="sxs-lookup"><span data-stu-id="1f5c3-299">150 Total</span></span> |

<span data-ttu-id="1f5c3-300">V takovém případě Očekáváme, že požadavek průměrnou propustností 1,275 RU/s.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-300">In this case, we expect an average throughput requirement of 1,275 RU/s.</span></span>  <span data-ttu-id="1f5c3-301">Zaokrouhlení až nejbližší 100 jsme by zřídit 1 300 RU/s pro kolekci této aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-301">Rounding up to the nearest 100, we would provision 1,300 RU/s for this application's collection.</span></span>

## <span data-ttu-id="1f5c3-302"><a id="RequestRateTooLarge"></a>Překročení omezení vyhrazenou propustností v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-302"><a id="RequestRateTooLarge"></a> Exceeding reserved throughput limits in Azure Cosmos DB</span></span>
<span data-ttu-id="1f5c3-303">Odvolat, že spotřeba jednotek žádosti budou vyhodnocené jako za sekundu Pokud rozpočtu je prázdný.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-303">Recall that request unit consumption is evaluated as a rate per second if the budget is empty.</span></span> <span data-ttu-id="1f5c3-304">Pro aplikace, které překračují rychlost jednotky zřízené požadavků pro kontejner budou požadavky na tuto kolekci omezeny, dokud rychlost klesne pod úroveň vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-304">For applications that exceed the provisioned request unit rate for a container, requests to that collection will be throttled until the rate drops below the reserved level.</span></span> <span data-ttu-id="1f5c3-305">Když dojde omezení, bude ho preventivně ukončení požadavek s RequestRateTooLargeException (kód stavu HTTP 429) a vrátit hlavičku x-ms opakování za ms, která určuje množství času v milisekundách, která uživatel musí počkat před provedením nového pokusu serveru požadavek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-305">When a throttle occurs, the server will preemptively end the request with RequestRateTooLargeException (HTTP status code 429) and return the x-ms-retry-after-ms header indicating the amount of time, in milliseconds, that the user must wait before reattempting the request.</span></span>

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

<span data-ttu-id="1f5c3-306">Pokud používáte klienta SDK rozhraní .NET a LINQ dotazů a potom ve většině případů není nutné řešit výjimku, jako aktuální verze rozhraní .NET Client SDK implicitně zachytí této odpovědi, respektuje záhlaví zadaný server opakovat po a opakuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-306">If you are using the .NET Client SDK and LINQ queries, then most of the time you never have to deal with this exception, as the current version of the .NET Client SDK implicitly catches this response, respects the server-specified retry-after header, and retries the request.</span></span> <span data-ttu-id="1f5c3-307">Pokud váš účet je současně přistupuje více klientů, další pokus bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-307">Unless your account is being accessed concurrently by multiple clients, the next retry will succeed.</span></span>

<span data-ttu-id="1f5c3-308">Pokud máte více než jednoho klienta kumulativně operační vyšší rychlost požadavků nemusí stačit výchozí chování opakování a klient vyvolá výjimku DocumentClientException se stavovým kódem 429 k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-308">If you have more than one client cumulatively operating above the request rate, the default retry behavior may not suffice, and the client will throw a DocumentClientException with status code 429 to the application.</span></span> <span data-ttu-id="1f5c3-309">V případech, jako je tato zvažte zpracování logiky aplikace chyba zpracování rutiny nebo zvýšení vyhrazenou propustností kontejneru a postup pro opakované.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-309">In cases such as this, you may consider handling retry behavior and logic in your application's error handling routines or increasing the reserved throughput for the container.</span></span>

## <span data-ttu-id="1f5c3-310"><a id="RequestRateTooLargeAPIforMongoDB"></a>Překročení omezení vyhrazenou propustností v rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-310"><a id="RequestRateTooLargeAPIforMongoDB"></a> Exceeding reserved throughput limits in API for MongoDB</span></span>
<span data-ttu-id="1f5c3-311">Aplikace, které překračují jednotek zřízené žádosti pro kolekci budou omezeny, dokud rychlost klesne pod úroveň vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-311">Applications that exceed the provisioned request units for a collection will be throttled until the rate drops below the reserved level.</span></span> <span data-ttu-id="1f5c3-312">Když dojde omezení, back-end se ukončí ho preventivně požadavek s *16500* kód chyby - *příliš mnoho požadavků*.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-312">When a throttle occurs, the backend will preemptively end the request with a *16500* error code - *Too Many Requests*.</span></span> <span data-ttu-id="1f5c3-313">Ve výchozím nastavení, rozhraní API pro MongoDB bude automaticky opakovat až 10krát před vrácením *příliš mnoho požadavků* kód chyby.</span><span class="sxs-lookup"><span data-stu-id="1f5c3-313">By default, API for MongoDB will automatically retry up to 10 times before returning a *Too Many Requests* error code.</span></span> <span data-ttu-id="1f5c3-314">Pokud se zobrazuje řada *příliš mnoho požadavků* kódy chyb, můžete zvážit buď přidání opakování chování vaší aplikace chyba zpracování rutiny nebo [zvýšení vyhrazenou propustností pro kolekci](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-314">If you are receiving many *Too Many Requests* error codes, you may consider either adding retry behavior in your application's error handling routines or [increasing the reserved throughput for the collection](set-throughput.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f5c3-315">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f5c3-315">Next steps</span></span>
<span data-ttu-id="1f5c3-316">Další informace o vyhrazenou propustností s databázemi Azure Cosmos DB najdete v těchto zdrojích:</span><span class="sxs-lookup"><span data-stu-id="1f5c3-316">To learn more about reserved throughput with Azure Cosmos DB databases, explore these resources:</span></span>

* [<span data-ttu-id="1f5c3-317">Azure Cosmos DB ceny</span><span class="sxs-lookup"><span data-stu-id="1f5c3-317">Azure Cosmos DB pricing</span></span>](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [<span data-ttu-id="1f5c3-318">Segmentace dat v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1f5c3-318">Partitioning data in Azure Cosmos DB</span></span>](partition-data.md)

<span data-ttu-id="1f5c3-319">Další informace o databázi Cosmos Azure najdete v tématu Azure Cosmos DB [dokumentaci](https://azure.microsoft.com/documentation/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-319">To learn more about Azure Cosmos DB, see the Azure Cosmos DB [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/).</span></span> 

<span data-ttu-id="1f5c3-320">Začínáme s škálování a výkon testování pomocí Azure Cosmos DB, najdete v tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="1f5c3-320">To get started with scale and performance testing with Azure Cosmos DB, see [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md).</span></span>

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
