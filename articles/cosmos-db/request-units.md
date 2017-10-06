---
title: aaaRequest jednotky & odhadnout propustnost - Azure Cosmos DB | Microsoft Docs
description: "Další informace o tom, jak toounderstand, zadejte a odhadnout požadavky na jednotky žádosti v Azure Cosmos DB."
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
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a><span data-ttu-id="d291d-103">Požadované jednotky v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d291d-103">Request Units in Azure Cosmos DB</span></span>
<span data-ttu-id="d291d-104">Nyní k dispozici: Azure Cosmos DB [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner).</span><span class="sxs-lookup"><span data-stu-id="d291d-104">Now available: Azure Cosmos DB [request unit calculator](https://www.documentdb.com/capacityplanner).</span></span> <span data-ttu-id="d291d-105">Další informace v [odhadnout, musí vaše propustnost](request-units.md#estimating-throughput-needs).</span><span class="sxs-lookup"><span data-stu-id="d291d-105">Learn more in [Estimating your throughput needs](request-units.md#estimating-throughput-needs).</span></span>

![Propustnost kalkulačky][5]

## <a name="introduction"></a><span data-ttu-id="d291d-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="d291d-107">Introduction</span></span>
<span data-ttu-id="d291d-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je globálně distribuované databáze více modelu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d291d-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is Microsoft's globally distributed multi-model database.</span></span> <span data-ttu-id="d291d-109">S Azure DB Cosmos nemáte toorent virtuálních počítačů, nasazování softwaru nebo monitorování databází.</span><span class="sxs-lookup"><span data-stu-id="d291d-109">With Azure Cosmos DB, you don't have toorent virtual machines, deploy software, or monitor databases.</span></span> <span data-ttu-id="d291d-110">Azure Cosmos DB je provozována a průběžně monitorovat pomocí Microsoft nejvyšší technici toodeliver world třída data dostupnosti, výkonu a ochrany.</span><span class="sxs-lookup"><span data-stu-id="d291d-110">Azure Cosmos DB is operated and continuously monitored by Microsoft top engineers toodeliver world class availability, performance, and data protection.</span></span> <span data-ttu-id="d291d-111">Přistupujete k datům pomocí rozhraní API podle svého výběru jako [DocumentDB SQL](documentdb-sql-query.md) (dokumentu), MongoDB (dokumentu), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (klíč hodnota), a [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graf) jsou všechny nativně podporuje.</span><span class="sxs-lookup"><span data-stu-id="d291d-111">You can access your data using APIs of your choice, as [DocumentDB SQL](documentdb-sql-query.md) (document), MongoDB (document), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (key-value), and [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graph) are all natively supported.</span></span> <span data-ttu-id="d291d-112">Měna Hello Azure Cosmos databáze je hello jednotka žádosti (RU).</span><span class="sxs-lookup"><span data-stu-id="d291d-112">hello currency of Azure Cosmos DB is hello Request Unit (RU).</span></span> <span data-ttu-id="d291d-113">S RUs není nutné tooreserve kapacity pro čtení a zápis a zřizovat procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="d291d-113">With RUs, you do not need tooreserve read/write capacities or provision CPU, Memory and IOPS.</span></span>

<span data-ttu-id="d291d-114">Azure Cosmos DB podporuje několik rozhraní API s různé operace, od jednoduchého čte a zapisuje toocomplex dotazy grafu.</span><span class="sxs-lookup"><span data-stu-id="d291d-114">Azure Cosmos DB supports a number of APIs with different operations ranging from simple reads and writes toocomplex graph queries.</span></span> <span data-ttu-id="d291d-115">Vzhledem k tomu, že ne všechny požadavky jsou stejné, jsou přiřazeny normalizovaný objemu **požadované jednotky** podle hello objem výpočtů požadované tooserve hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="d291d-115">Since not all requests are equal, they are assigned a normalized quantity of **request units** based on hello amount of computation required tooserve hello request.</span></span> <span data-ttu-id="d291d-116">Hello počet jednotek žádosti operace je deterministická, a můžete sledovat hello počet jednotek žádosti spotřebovávají všechny operace v Azure Cosmos DB prostřednictvím hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d291d-116">hello number of request units for an operation is deterministic, and you can track hello number of request units consumed by any operation in Azure Cosmos DB via a response header.</span></span> 

<span data-ttu-id="d291d-117">tooprovide předvídatelný výkon, je nutné tooreserve propustnost v jednotkách 100 RU za sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-117">tooprovide predictable performance, you need tooreserve throughput in units of 100 RU/second.</span></span> 

<span data-ttu-id="d291d-118">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="d291d-118">After reading this article, you'll be able tooanswer hello following questions:</span></span>  

* <span data-ttu-id="d291d-119">Jaké jsou požadované jednotky a požádat o poplatky?</span><span class="sxs-lookup"><span data-stu-id="d291d-119">What are request units and request charges?</span></span>
* <span data-ttu-id="d291d-120">Jak určit kapacitu jednotky žádosti pro kolekci?</span><span class="sxs-lookup"><span data-stu-id="d291d-120">How do I specify request unit capacity for a collection?</span></span>
* <span data-ttu-id="d291d-121">Jak odhadnout, že je jednotka žádosti Moje aplikace?</span><span class="sxs-lookup"><span data-stu-id="d291d-121">How do I estimate my application's request unit needs?</span></span>
* <span data-ttu-id="d291d-122">Co se stane, když I překročit kapacitu jednotky žádosti pro kolekci?</span><span class="sxs-lookup"><span data-stu-id="d291d-122">What happens if I exceed request unit capacity for a collection?</span></span>

<span data-ttu-id="d291d-123">Jak Azure Cosmos DB je více modelu databáze, je důležité toonote, že bude označujeme tooa kolekce či dokumentu pro dokument rozhraní API, grafu nebo uzel pro graf rozhraní API a tabulka/entity pro tabulku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d291d-123">As Azure Cosmos DB is a multi-model database, it is important toonote that we will refer tooa collection/document for a document API, a graph/node for a graph API and a table/entity for table API.</span></span> <span data-ttu-id="d291d-124">Propustnost tohoto dokumentu jsme se generalize toohello koncepty kontejneru nebo položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-124">Throughput this document we will generalize toohello concepts of container/item.</span></span>

## <a name="request-units-and-request-charges"></a><span data-ttu-id="d291d-125">Jednotek žádosti a poplatky požadavku</span><span class="sxs-lookup"><span data-stu-id="d291d-125">Request units and request charges</span></span>
<span data-ttu-id="d291d-126">Azure Cosmos DB poskytuje rychlé, předvídatelný výkon pomocí *rezervování* toosatisfy prostředky musí propustnost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d291d-126">Azure Cosmos DB delivers fast, predictable performance by *reserving* resources toosatisfy your application's throughput needs.</span></span>  <span data-ttu-id="d291d-127">Vzhledem k aplikaci načíst a přístup k vzory změny v čase, Azure Cosmos DB vám umožní zvýšení tooeasily nebo snížit množství hello aplikace k dispozici tooyour vyhrazenou propustností.</span><span class="sxs-lookup"><span data-stu-id="d291d-127">Because application load and access patterns change over time, Azure Cosmos DB allows you tooeasily increase or decrease hello amount of reserved throughput available tooyour application.</span></span>

<span data-ttu-id="d291d-128">S Azure Cosmos databáze je zadána vyhrazenou propustností z hlediska jednotek žádosti zpracování za sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-128">With Azure Cosmos DB, reserved throughput is specified in terms of request units processing per second.</span></span> <span data-ttu-id="d291d-129">Si můžete představit jednotek žádosti jako měnu propustnost, které jste *rezervovat* množství zaručit jednotek žádosti k dispozici tooyour aplikace na základě za sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-129">You can think of request units as throughput currency, whereby you *reserve* an amount of guaranteed request units available tooyour application on per second basis.</span></span>  <span data-ttu-id="d291d-130">Každé operace v Azure DB Cosmos - zápis dokumentu, provádění dotazu, aktualizace dokumentu - spotřebuje procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="d291d-130">Each operation in Azure Cosmos DB - writing a document, performing a query, updating a document - consumes CPU, memory, and IOPS.</span></span>  <span data-ttu-id="d291d-131">To znamená, každou operaci způsobuje *požadavku poplatků*, vyjádřeného v *požadované jednotky*.</span><span class="sxs-lookup"><span data-stu-id="d291d-131">That is, each operation incurs a *request charge*, which is expressed in *request units*.</span></span>  <span data-ttu-id="d291d-132">Pochopení hello faktory, což ovlivňuje poplatky jednotek žádosti, společně s požadavky na propustnost vaší aplikace, umožňuje vám toorun aplikaci jako efektivně možné náklady.</span><span class="sxs-lookup"><span data-stu-id="d291d-132">Understanding hello factors which impact request unit charges, along with your application's throughput requirements, enables you toorun your application as cost effectively as possible.</span></span> <span data-ttu-id="d291d-133">Průzkumník dotazů Hello je také jádro hello tootest skvělý nástroj dotazu.</span><span class="sxs-lookup"><span data-stu-id="d291d-133">hello query explorer is also a wonderful tool tootest hello core of a query.</span></span>

<span data-ttu-id="d291d-134">Doporučujeme začít sledování hello následující video, kde vysvětluje Aravind Ramachandran jednotek žádosti a předvídatelného výkonu s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d291d-134">We recommend getting started by watching hello following video, where Aravind Ramachandran explains request units and predictable performance with Azure Cosmos DB.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a><span data-ttu-id="d291d-135">Určení požadavku jednotka kapacity v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d291d-135">Specifying request unit capacity in Azure Cosmos DB</span></span>
<span data-ttu-id="d291d-136">Při spouštění novou kolekci, tabulka nebo grafu, můžete zadat číslo hello jednotek žádosti za sekundu (RU za sekundu), kterou chcete vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="d291d-136">When starting a new collection, table or graph, you specify hello number of request units per second (RU per second) you want reserved.</span></span> <span data-ttu-id="d291d-137">Na základě hello zřízené propustnosti, Azure Cosmos DB přiděluje fyzické oddíly toohost kolekce a rozdělení/rebalances dat napříč oddíly ho s růstem.</span><span class="sxs-lookup"><span data-stu-id="d291d-137">Based on hello provisioned throughput, Azure Cosmos DB allocates physical partitions toohost your collection and splits/rebalances data across partitions as it grows.</span></span>

<span data-ttu-id="d291d-138">Azure Cosmos DB vyžaduje že toobe klíče oddílu zadat, když je kolekce s 2 500 jednotek žádosti přiděleným nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d291d-138">Azure Cosmos DB requires a partition key toobe specified when a collection is provisioned with 2,500 request units or higher.</span></span> <span data-ttu-id="d291d-139">Klíč oddílu je taky požadované tooscale propustnost vaší kolekce nad rámec odpovídající 2500 jednotek žádosti v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="d291d-139">A partition key is also required tooscale your collection's throughput beyond 2,500 request units in hello future.</span></span> <span data-ttu-id="d291d-140">Proto důrazně doporučujeme tooconfigure [klíč oddílu](partition-data.md) při vytváření kontejneru bez ohledu na počáteční propustnosti.</span><span class="sxs-lookup"><span data-stu-id="d291d-140">Therefore, it is highly recommended tooconfigure a [partition key](partition-data.md) when creating a container regardless of your initial throughput.</span></span> <span data-ttu-id="d291d-141">Vzhledem k tomu, že vaše data mít toobe rozdělit do několika oddílů, je nutné toopick klíč oddílu, který má vysokou kardinalitou (100 toomillions jedinečných hodnot), aby kolekce, tabulka nebo graf a žádostí je možné rozšířit jednotně pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d291d-141">Since your data might have toobe split across multiple partitions, it is necessary toopick a partition key that has a high cardinality (100 toomillions of distinct values) so that your collection/table/graph and requests can be scaled uniformly by Azure Cosmos DB.</span></span> 

> [!NOTE]
> <span data-ttu-id="d291d-142">Klíč oddílu je logické hranice a není fyzický jeden.</span><span class="sxs-lookup"><span data-stu-id="d291d-142">A partition key is a logical boundary, and not a physical one.</span></span> <span data-ttu-id="d291d-143">Proto není nutné toolimit hello počet hodnoty klíče jedinečné oddílu.</span><span class="sxs-lookup"><span data-stu-id="d291d-143">Therefore, you do not need toolimit hello number of distinct partition key values.</span></span> <span data-ttu-id="d291d-144">Ve skutečnosti je lepší toohave více jedinečných oddílu hodnoty klíče menší, než databázi Cosmos Azure má další možnosti vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d291d-144">It is in fact better toohave more distinct partition key values than less, as Azure Cosmos DB has more load balancing options.</span></span>

<span data-ttu-id="d291d-145">Zde je fragment kódu pro vytvoření kolekce s 3 000 požadavek hello jednotek na druhý pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="d291d-145">Here is a code snippet for creating a collection with 3,000 request units per second using hello .NET SDK:</span></span>

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

<span data-ttu-id="d291d-146">Azure Cosmos DB funguje na rezervace modelu na propustnost.</span><span class="sxs-lookup"><span data-stu-id="d291d-146">Azure Cosmos DB operates on a reservation model on throughput.</span></span> <span data-ttu-id="d291d-147">To znamená, že se vám účtuje hello množství propustnost *vyhrazené*, bez ohledu na to, kolik z této propustnost je aktivně *používá*.</span><span class="sxs-lookup"><span data-stu-id="d291d-147">That is, you are billed for hello amount of throughput *reserved*, regardless of how much of that throughput is actively *used*.</span></span> <span data-ttu-id="d291d-148">Jako aplikace zatížení, data a využití vzory změn je možné snadno škálovat nahoru a dolů hello množství vyhrazené RUs prostřednictvím sady SDK nebo pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d291d-148">As your application's load, data, and usage patterns change you can easily scale up and down hello amount of reserved RUs through SDKs or using hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="d291d-149">Každý kolekce a tabulka/grafika jsou namapované tooan `Offer` prostředků v Azure DB Cosmos, který má metadata o hello zřízené propustnosti.</span><span class="sxs-lookup"><span data-stu-id="d291d-149">Each collection/table/graph are mapped tooan `Offer` resource in Azure Cosmos DB, which has metadata about hello provisioned throughput.</span></span> <span data-ttu-id="d291d-150">Vyhledávání hello odpovídající prostředek nabídka pro kontejner a poté aktualizace pomocí hello novou hodnotu propustnosti, můžete změnit hello přidělené propustnost.</span><span class="sxs-lookup"><span data-stu-id="d291d-150">You can change hello allocated throughput by looking up hello corresponding offer resource for a container, then updating it with hello new throughput value.</span></span> <span data-ttu-id="d291d-151">Zde je fragment kódu pro změnu hello propustnost kolekce too5, hello 000 jednotek žádosti za druhé pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="d291d-151">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second using hello .NET SDK:</span></span>

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="d291d-152">Neexistuje žádný dopad toohello dostupnosti vaší kontejneru při změně hello propustnost.</span><span class="sxs-lookup"><span data-stu-id="d291d-152">There is no impact toohello availability of your container when you change hello throughput.</span></span> <span data-ttu-id="d291d-153">Nové vyhrazenou propustností hello je obvykle efektivní během několika sekund na aplikace hello nové propustnosti.</span><span class="sxs-lookup"><span data-stu-id="d291d-153">Typically hello new reserved throughput is effective within seconds on application of hello new throughput.</span></span>

## <a name="request-unit-considerations"></a><span data-ttu-id="d291d-154">Aspekty jednotek žádosti</span><span class="sxs-lookup"><span data-stu-id="d291d-154">Request unit considerations</span></span>
<span data-ttu-id="d291d-155">Při odhadování hello počet tooreserve jednotek žádosti pro váš kontejner Azure Cosmos DB, je důležité tootake hello v úvahu následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="d291d-155">When estimating hello number of request units tooreserve for your Azure Cosmos DB container, it is important tootake hello following variables into consideration:</span></span>

* <span data-ttu-id="d291d-156">**Velikost položky**.</span><span class="sxs-lookup"><span data-stu-id="d291d-156">**Item size**.</span></span> <span data-ttu-id="d291d-157">Jak roste množství hello jednotek použití tooread nebo zapisovat data hello se taky zvýší.</span><span class="sxs-lookup"><span data-stu-id="d291d-157">As size increases hello units consumed tooread or write hello data will also increase.</span></span>
* <span data-ttu-id="d291d-158">**Počet vlastností položky**.</span><span class="sxs-lookup"><span data-stu-id="d291d-158">**Item property count**.</span></span> <span data-ttu-id="d291d-159">Za předpokladu, že výchozí indexování všech vlastností, toowrite hello jednotek použití, které dokumentu nebo uzel nebo ntity zvýší jako zvyšuje počet vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="d291d-159">Assuming default indexing of all properties, hello units consumed toowrite a document/node/ntity will increase as hello property count increases.</span></span>
* <span data-ttu-id="d291d-160">**Konzistenci dat**.</span><span class="sxs-lookup"><span data-stu-id="d291d-160">**Data consistency**.</span></span> <span data-ttu-id="d291d-161">Při použití úrovně konzistence dat silného nebo typu s ohraničenou Prošlostí, bude další jednotky spotřebované tooread položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-161">When using data consistency levels of Strong or Bounded Staleness, additional units will be consumed tooread items.</span></span>
* <span data-ttu-id="d291d-162">**Indexované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d291d-162">**Indexed properties**.</span></span> <span data-ttu-id="d291d-163">Zásadu indexu na každý kontejner určuje vlastnosti, které jsou uloženy ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d291d-163">An index policy on each container determines which properties are indexed by default.</span></span> <span data-ttu-id="d291d-164">Omezením hello počet indexované vlastnosti nebo povolením Opožděné indexování můžete snížit spotřebu jednotky vaší žádosti.</span><span class="sxs-lookup"><span data-stu-id="d291d-164">You can reduce your request unit consumption by limiting hello number of indexed properties or by enabling lazy indexing.</span></span>
* <span data-ttu-id="d291d-165">**Indexování dokumentů**.</span><span class="sxs-lookup"><span data-stu-id="d291d-165">**Document indexing**.</span></span> <span data-ttu-id="d291d-166">Ve výchozím nastavení je každá položka automaticky indexovaný bude využívat méně jednotek žádosti, pokud si zvolíte není tooindex některé položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-166">By default each item is automatically indexed, you will consume fewer request units if you choose not tooindex some of your items.</span></span>
* <span data-ttu-id="d291d-167">**Dotaz vzory**.</span><span class="sxs-lookup"><span data-stu-id="d291d-167">**Query patterns**.</span></span> <span data-ttu-id="d291d-168">složitost Hello dotazu má dopad na tom, kolik jednotek žádosti se spotřebovávají pro operace.</span><span class="sxs-lookup"><span data-stu-id="d291d-168">hello complexity of a query impacts how many Request Units are consumed for an operation.</span></span> <span data-ttu-id="d291d-169">Hello počet predikáty, povaha hello predikáty, projekce, počet UDF a velikost hello hello zdroje dat sady všechny ovlivnit hello náklady na dotaz operace.</span><span class="sxs-lookup"><span data-stu-id="d291d-169">hello number of predicates, nature of hello predicates, projections, number of UDFs, and hello size of hello source data set all influence hello cost of query operations.</span></span>
* <span data-ttu-id="d291d-170">**Použití skriptu**.</span><span class="sxs-lookup"><span data-stu-id="d291d-170">**Script usage**.</span></span>  <span data-ttu-id="d291d-171">Stejně jako u dotazů, využívat jednotek žádosti podle složitosti hello operací prováděných na hello uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="d291d-171">As with queries, stored procedures and triggers consume request units based on hello complexity of hello operations being performed.</span></span> <span data-ttu-id="d291d-172">Když budete vyvíjet aplikace, zkontrolujte hello požadavek poplatků záhlaví toobetter pochopit, jak každou operaci spotřebovává požadavek jednotky kapacity.</span><span class="sxs-lookup"><span data-stu-id="d291d-172">As you develop your application, inspect hello request charge header toobetter understand how each operation is consuming request unit capacity.</span></span>

## <a name="estimating-throughput-needs"></a><span data-ttu-id="d291d-173">Odhad potřeb propustnost</span><span class="sxs-lookup"><span data-stu-id="d291d-173">Estimating throughput needs</span></span>
<span data-ttu-id="d291d-174">Jednotka žádosti je normalizovaný míru náklady na zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="d291d-174">A request unit is a normalized measure of request processing cost.</span></span> <span data-ttu-id="d291d-175">Jednotka jedné žádosti představuje hello zpracování požadovaná kapacita tooread (prostřednictvím id nebo vlastní odkaz) jeden 1KB položky skládající se z 10 jedinečnou vlastnost hodnot (s výjimkou vlastnosti systému).</span><span class="sxs-lookup"><span data-stu-id="d291d-175">A single request unit represents hello processing capacity required tooread (via self link or id) a single 1KB item consisting of 10 unique property values (excluding system properties).</span></span> <span data-ttu-id="d291d-176">Žádost o toocreate (Vložit), nahraďte nebo odstranit hello stejné položky spotřebuje další zpracování ze služby hello a tak další požadované jednotky.</span><span class="sxs-lookup"><span data-stu-id="d291d-176">A request toocreate (insert), replace or delete hello same item will consume more processing from hello service and thereby more request units.</span></span>   

> [!NOTE]
> <span data-ttu-id="d291d-177">Hello účaří požadavků 1 jednotka pro 1KB položky odpovídá tooa jednoduché získat vlastní odkaz nebo id položky hello.</span><span class="sxs-lookup"><span data-stu-id="d291d-177">hello baseline of 1 request unit for a 1KB item corresponds tooa simple GET by self link or id of hello item.</span></span>
> 
> 

<span data-ttu-id="d291d-178">Zde je tabulku, která ukazuje, kolik požadavků například jednotky tooprovision na tři různé položky velikosti (1KB, 4KB a 64KB) a na dvou různých výkonu úrovních (500 čtení za sekundu + 100 zápisů za sekundu a 500 čtení za sekundu + 500 zápisů za sekundu).</span><span class="sxs-lookup"><span data-stu-id="d291d-178">For example, here's a table that shows how many request units tooprovision at three different item sizes (1KB, 4KB, and 64KB) and at two different performance levels (500 reads/second + 100 writes/second and 500 reads/second + 500 writes/second).</span></span> <span data-ttu-id="d291d-179">Hello konzistenci dat byla nakonfigurována v relaci a hello zásady indexování byla nastavena tooNone.</span><span class="sxs-lookup"><span data-stu-id="d291d-179">hello data consistency was configured at Session, and hello indexing policy was set tooNone.</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-180"><strong>Velikost položky</strong></span><span class="sxs-lookup"><span data-stu-id="d291d-180"><strong>Item size</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-181"><strong>Čtení za sekundu</strong></span><span class="sxs-lookup"><span data-stu-id="d291d-181"><strong>Reads/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-182"><strong>Zápisů za sekundu</strong></span><span class="sxs-lookup"><span data-stu-id="d291d-182"><strong>Writes/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-183"><strong>Jednotky žádostí</strong></span><span class="sxs-lookup"><span data-stu-id="d291d-183"><strong>Request units</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-184">1 KB</span><span class="sxs-lookup"><span data-stu-id="d291d-184">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-185">500</span><span class="sxs-lookup"><span data-stu-id="d291d-185">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-186">100</span><span class="sxs-lookup"><span data-stu-id="d291d-186">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-187">(500 * 1) + (100 * 5) = 1 000 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-187">(500 * 1) + (100 * 5) = 1,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-188">1 KB</span><span class="sxs-lookup"><span data-stu-id="d291d-188">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-189">500</span><span class="sxs-lookup"><span data-stu-id="d291d-189">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-190">500</span><span class="sxs-lookup"><span data-stu-id="d291d-190">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-191">(500 * 1) + (500 * 5) = 3000 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-191">(500 * 1) + (500 * 5) = 3,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-192">4 KB</span><span class="sxs-lookup"><span data-stu-id="d291d-192">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-193">500</span><span class="sxs-lookup"><span data-stu-id="d291d-193">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-194">100</span><span class="sxs-lookup"><span data-stu-id="d291d-194">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-195">(500 * 1,3) + (100 * 7) = 1,350 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-196">4 KB</span><span class="sxs-lookup"><span data-stu-id="d291d-196">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-197">500</span><span class="sxs-lookup"><span data-stu-id="d291d-197">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-198">500</span><span class="sxs-lookup"><span data-stu-id="d291d-198">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-199">(500 * 1,3) + (500 * 7) = 4,150 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-200">64 kB</span><span class="sxs-lookup"><span data-stu-id="d291d-200">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-201">500</span><span class="sxs-lookup"><span data-stu-id="d291d-201">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-202">100</span><span class="sxs-lookup"><span data-stu-id="d291d-202">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d291d-204">64 kB</span><span class="sxs-lookup"><span data-stu-id="d291d-204">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-205">500</span><span class="sxs-lookup"><span data-stu-id="d291d-205">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-206">500</span><span class="sxs-lookup"><span data-stu-id="d291d-206">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d291d-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span><span class="sxs-lookup"><span data-stu-id="d291d-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span></span></p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a><span data-ttu-id="d291d-208">Použití kalkulačky jednotek žádosti hello</span><span class="sxs-lookup"><span data-stu-id="d291d-208">Use hello request unit calculator</span></span>
<span data-ttu-id="d291d-209">Zákazníci toohelp jemné vyladění jejich odhady propustnost, je web, na základě [kalkulačky jednotek žádosti](https://www.documentdb.com/capacityplanner) požadavky jednotky toohelp odhad hello požadavku pro typická operace, včetně:</span><span class="sxs-lookup"><span data-stu-id="d291d-209">toohelp customers fine tune their throughput estimations, there is a web based [request unit calculator](https://www.documentdb.com/capacityplanner) toohelp estimate hello request unit requirements for typical operations, including:</span></span>

* <span data-ttu-id="d291d-210">Vytvoří položku (zápisy)</span><span class="sxs-lookup"><span data-stu-id="d291d-210">Item creates (writes)</span></span>
* <span data-ttu-id="d291d-211">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="d291d-211">Item reads</span></span>
* <span data-ttu-id="d291d-212">Odstranění položky</span><span class="sxs-lookup"><span data-stu-id="d291d-212">Item deletes</span></span>
* <span data-ttu-id="d291d-213">Aktualizace položky</span><span class="sxs-lookup"><span data-stu-id="d291d-213">Item updates</span></span>

<span data-ttu-id="d291d-214">Nástroj Hello zahrnuje taky podporu odhadnout požadavky na úložiště dat podle hello ukázkové položky, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="d291d-214">hello tool also includes support for estimating data storage needs based on hello sample items you provide.</span></span>

<span data-ttu-id="d291d-215">Pomocí nástroje hello je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="d291d-215">Using hello tool is simple:</span></span>

1. <span data-ttu-id="d291d-216">Nahrajte jednu nebo více reprezentativní položek.</span><span class="sxs-lookup"><span data-stu-id="d291d-216">Upload one or more representative items.</span></span>
   
    ![Nahrát položky toohello požadavek jednotky kalkulačky][2]
2. <span data-ttu-id="d291d-218">požadavky na úložiště dat tooestimate, zadejte hello celkový počet položek očekáváte, že toostore.</span><span class="sxs-lookup"><span data-stu-id="d291d-218">tooestimate data storage requirements, enter hello total number of items you expect toostore.</span></span>
3. <span data-ttu-id="d291d-219">Zadejte hello počet položek, které vytvářet, číst, aktualizovat a odstranit operations vyžadují (na základě za sekundu).</span><span class="sxs-lookup"><span data-stu-id="d291d-219">Enter hello number of items create, read, update, and delete operations you require (on a per-second basis).</span></span> <span data-ttu-id="d291d-220">tooestimate hello požadavek jednotky poplatky položky aktualizace operací, nahrajte kopii hello ukázkové položky z kroku 1 výše, zahrnuje typické pole aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d291d-220">tooestimate hello request unit charges of item update operations, upload a copy of hello sample item from step 1 above that includes typical field updates.</span></span>  <span data-ttu-id="d291d-221">Například pokud položku aktualizace obvykle upravit dvě vlastnosti s názvem lastLogin a userVisits a pak položku ukázka hello jednoduše zkopírovat, aktualizujte hello hodnoty pro tyto dvě vlastnosti a nahrajte hello zkopírovat položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-221">For example, if item updates typically modify two properties named lastLogin and userVisits, then simply copy hello sample item, update hello values for those two properties, and upload hello copied item.</span></span>
   
    ![Zadejte požadavky na propustnost v kalkulačky jednotek žádosti hello][3]
4. <span data-ttu-id="d291d-223">Klikněte na tlačítko Vypočítat a prozkoumejte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="d291d-223">Click calculate and examine hello results.</span></span>
   
    ![Žádosti o výsledky kalkulačky jednotky][4]

> [!NOTE]
> <span data-ttu-id="d291d-225">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, nahrajte vzorek každého *typ* z toohello typické položky nástroje a potom vypočítat hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="d291d-225">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then upload a sample of each *type* of typical item toohello tool and then calculate hello results.</span></span>
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a><span data-ttu-id="d291d-226">Použít hlavičku odpovědi hello Azure Cosmos DB požadavek zdarma</span><span class="sxs-lookup"><span data-stu-id="d291d-226">Use hello Azure Cosmos DB request charge response header</span></span>
<span data-ttu-id="d291d-227">Každou odpověď z hello služby Azure Cosmos DB obsahuje vlastní hlavičky (`x-ms-request-charge`) obsahující jednotek žádosti hello využité pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="d291d-227">Every response from hello Azure Cosmos DB service includes a custom header (`x-ms-request-charge`) that contains hello request units consumed for hello request.</span></span> <span data-ttu-id="d291d-228">Tuto hlavičku je také přístupné prostřednictvím hello Azure Cosmos DB sady SDK.</span><span class="sxs-lookup"><span data-stu-id="d291d-228">This header is also accessible through hello Azure Cosmos DB SDKs.</span></span> <span data-ttu-id="d291d-229">RequestCharge v hello .NET SDK, je vlastnost hello ResourceResponse objektu.</span><span class="sxs-lookup"><span data-stu-id="d291d-229">In hello .NET SDK, RequestCharge is a property of hello ResourceResponse object.</span></span>  <span data-ttu-id="d291d-230">Pro dotazy hello Průzkumníka dotazů DB Cosmos Azure v hello portál Azure poskytuje informace poplatků požadavku pro spuštění dotazů.</span><span class="sxs-lookup"><span data-stu-id="d291d-230">For queries, hello Azure Cosmos DB Query Explorer in hello Azure portal provides request charge information for executed queries.</span></span>

![Zkoumání RU poplatky v hello Průzkumníka dotazů][1]

<span data-ttu-id="d291d-232">Myslete na to, jednu metodu k odhadování hello množství vyhrazenou propustností požadované aplikací je toorecord hello požadavek jednotky poplatků přidružené spuštěným typických operací pro položku reprezentativní používá vaše aplikace a potom odhad hello počet operací předpokládáte provádění každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-232">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>  <span data-ttu-id="d291d-233">Být jisti toomeasure a zahrnují typické dotazy a také při použití skriptu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d291d-233">Be sure toomeasure and include typical queries and Azure Cosmos DB script usage as well.</span></span>

> [!NOTE]
> <span data-ttu-id="d291d-234">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, potom si poznamenejte hello použít operaci požadavku jednotky poplatků spojených s jednotlivými *typ* typické položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-234">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

<span data-ttu-id="d291d-235">Například:</span><span class="sxs-lookup"><span data-stu-id="d291d-235">For example:</span></span>

1. <span data-ttu-id="d291d-236">Zaznamenejte poplatků jednotky hello žádost o vytvoření (vkládání) typické položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-236">Record hello request unit charge of creating (inserting) a typical item.</span></span> 
2. <span data-ttu-id="d291d-237">Záznamů hello poplatků jednotky žádosti o čtení typické položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-237">Record hello request unit charge of reading a typical item.</span></span>
3. <span data-ttu-id="d291d-238">Záznamů hello požadavek jednotky poplatků aktualizace typické položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-238">Record hello request unit charge of updating a typical item.</span></span>
4. <span data-ttu-id="d291d-239">Záznamů hello požadavek jednotky poplatků typické, běžné položky dotazů.</span><span class="sxs-lookup"><span data-stu-id="d291d-239">Record hello request unit charge of typical, common item queries.</span></span>
5. <span data-ttu-id="d291d-240">Záznamů hello požadavek jednotky poplatků vlastních skriptů (uložené procedury, triggery, funkce definované uživatelem) využít aplikace hello</span><span class="sxs-lookup"><span data-stu-id="d291d-240">Record hello request unit charge of any custom scripts (stored procedures, triggers, user-defined functions) leveraged by hello application</span></span>
6. <span data-ttu-id="d291d-241">Vypočítejte hello požadované žádosti že jednotky dané hello odhadovaný počet operací předpokládáte toorun každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-241">Calculate hello required request units given hello estimated number of operations you anticipate toorun each second.</span></span>

### <span data-ttu-id="d291d-242"><a id="GetLastRequestStatistics"></a>Použití rozhraní API pro příkaz GetLastRequestStatistics pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="d291d-242"><a id="GetLastRequestStatistics"></a>Use API for MongoDB's GetLastRequestStatistics command</span></span>
<span data-ttu-id="d291d-243">Rozhraní API pro MongoDB podporuje vlastního příkazu *getLastRequestStatistics*, pro načítání hello požadavek zdarma pro zadané operace.</span><span class="sxs-lookup"><span data-stu-id="d291d-243">API for MongoDB supports a custom command, *getLastRequestStatistics*, for retrieving hello request charge for specified operations.</span></span>

<span data-ttu-id="d291d-244">Například v hello prostředí Mongo, hello operaci provést, které chcete tooverify hello požadavek zdarma pro.</span><span class="sxs-lookup"><span data-stu-id="d291d-244">For example, in hello Mongo Shell, execute hello operation you want tooverify hello request charge for.</span></span>
```
> db.sample.find()
```

<span data-ttu-id="d291d-245">Potom spusťte příkaz hello *getLastRequestStatistics*.</span><span class="sxs-lookup"><span data-stu-id="d291d-245">Next, execute hello command *getLastRequestStatistics*.</span></span>
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

<span data-ttu-id="d291d-246">Myslete na to, jednu metodu k odhadování hello množství vyhrazenou propustností požadované aplikací je toorecord hello požadavek jednotky poplatků přidružené spuštěným typických operací pro položku reprezentativní používá vaše aplikace a potom odhad hello počet operací předpokládáte provádění každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="d291d-246">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>

> [!NOTE]
> <span data-ttu-id="d291d-247">Pokud máte typy položek, které se výrazně liší z hlediska velikosti a hello počet indexované vlastnosti, potom si poznamenejte hello použít operaci požadavku jednotky poplatků spojených s jednotlivými *typ* typické položky.</span><span class="sxs-lookup"><span data-stu-id="d291d-247">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a><span data-ttu-id="d291d-248">Použít rozhraní API pro portál metriky pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="d291d-248">Use API for MongoDB's portal metrics</span></span>
<span data-ttu-id="d291d-249">Nejjednodušší způsob, jak tooget dobrý odhad požadavek jednotky poplatky za vaše rozhraní API pro databázi MongoDB je toouse hello Hello [portál Azure](https://portal.azure.com) metriky.</span><span class="sxs-lookup"><span data-stu-id="d291d-249">hello simplest way tooget a good estimation of request unit charges for your API for MongoDB database is toouse hello [Azure portal](https://portal.azure.com) metrics.</span></span> <span data-ttu-id="d291d-250">S hello *počet požadavků, které* a *požadavek poplatků* grafy, můžete získat odhad, kolik jednotek žádosti je každé operace využívání a kolik jednotek žádosti budou využívat relativní tooone jiné.</span><span class="sxs-lookup"><span data-stu-id="d291d-250">With hello *Number of requests* and *Request Charge* charts, you can get an estimation of how many request units each operation is consuming and how many request units they consume relative tooone another.</span></span>

![Rozhraní API pro MongoDB portálu metriky][6]

## <a name="a-request-unit-estimation-example"></a><span data-ttu-id="d291d-252">V příkladu odhad jednotek žádosti</span><span class="sxs-lookup"><span data-stu-id="d291d-252">A request unit estimation example</span></span>
<span data-ttu-id="d291d-253">Vezměte v úvahu následující ~ 1KB dokumentu hello:</span><span class="sxs-lookup"><span data-stu-id="d291d-253">Consider hello following ~1KB document:</span></span>

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
> <span data-ttu-id="d291d-254">Dokumenty jsou minifikovaný v Azure Cosmos DB, takže hello systému vypočítat velikost hello dokumentu výše je něco menší než 1 KB.</span><span class="sxs-lookup"><span data-stu-id="d291d-254">Documents are minified in Azure Cosmos DB, so hello system calculated size of hello document above is slightly less than 1KB.</span></span>
> 
> 

<span data-ttu-id="d291d-255">Hello následující tabulka uvádí přibližnou požadavek jednotky poplatky za typických operací u této položky (hello přibližnou požadavek jednotky poplatků předpokládá úroveň konzistence účtu hello nastavena příliš "Relace" a že jsou všechny položky automaticky indexovány):</span><span class="sxs-lookup"><span data-stu-id="d291d-255">hello following table shows approximate request unit charges for typical operations on this item (hello approximate request unit charge assumes that hello account consistency level is set too“Session” and that all items are automatically indexed):</span></span>

| <span data-ttu-id="d291d-256">Operace</span><span class="sxs-lookup"><span data-stu-id="d291d-256">Operation</span></span> | <span data-ttu-id="d291d-257">Žádost o jednotky zdarma</span><span class="sxs-lookup"><span data-stu-id="d291d-257">Request Unit Charge</span></span> |
| --- | --- |
| <span data-ttu-id="d291d-258">Vytvoření položky</span><span class="sxs-lookup"><span data-stu-id="d291d-258">Create item</span></span> |<span data-ttu-id="d291d-259">~ 15 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-259">~15 RU</span></span> |
| <span data-ttu-id="d291d-260">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="d291d-260">Read item</span></span> |<span data-ttu-id="d291d-261">~ 1 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-261">~1 RU</span></span> |
| <span data-ttu-id="d291d-262">Dotaz položky podle id</span><span class="sxs-lookup"><span data-stu-id="d291d-262">Query item by id</span></span> |<span data-ttu-id="d291d-263">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-263">~2.5 RU</span></span> |

<span data-ttu-id="d291d-264">Kromě toho tato tabulka ukazuje přibližnou požadavek jednotky poplatky za typické dotazy používané v aplikaci hello:</span><span class="sxs-lookup"><span data-stu-id="d291d-264">Additionally, this table shows approximate request unit charges for typical queries used in hello application:</span></span>

| <span data-ttu-id="d291d-265">Dotaz</span><span class="sxs-lookup"><span data-stu-id="d291d-265">Query</span></span> | <span data-ttu-id="d291d-266">Žádost o jednotky zdarma</span><span class="sxs-lookup"><span data-stu-id="d291d-266">Request Unit Charge</span></span> | <span data-ttu-id="d291d-267">počet vrácených položek</span><span class="sxs-lookup"><span data-stu-id="d291d-267"># of Returned Items</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d291d-268">Vyberte jídlo podle id</span><span class="sxs-lookup"><span data-stu-id="d291d-268">Select food by id</span></span> |<span data-ttu-id="d291d-269">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-269">~2.5 RU</span></span> |<span data-ttu-id="d291d-270">1</span><span class="sxs-lookup"><span data-stu-id="d291d-270">1</span></span> |
| <span data-ttu-id="d291d-271">Vyberte potravin podle výrobce</span><span class="sxs-lookup"><span data-stu-id="d291d-271">Select foods by manufacturer</span></span> |<span data-ttu-id="d291d-272">~ 7 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-272">~7 RU</span></span> |<span data-ttu-id="d291d-273">7</span><span class="sxs-lookup"><span data-stu-id="d291d-273">7</span></span> |
| <span data-ttu-id="d291d-274">Vyberte skupiny jídlo a pořadí podle váhy</span><span class="sxs-lookup"><span data-stu-id="d291d-274">Select by food group and order by weight</span></span> |<span data-ttu-id="d291d-275">~ 70 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-275">~70 RU</span></span> |<span data-ttu-id="d291d-276">100</span><span class="sxs-lookup"><span data-stu-id="d291d-276">100</span></span> |
| <span data-ttu-id="d291d-277">Vyberte nejvyšší 10 potravin ve skupině jídlo</span><span class="sxs-lookup"><span data-stu-id="d291d-277">Select top 10 foods in a food group</span></span> |<span data-ttu-id="d291d-278">~ 10 RU</span><span class="sxs-lookup"><span data-stu-id="d291d-278">~10 RU</span></span> |<span data-ttu-id="d291d-279">10</span><span class="sxs-lookup"><span data-stu-id="d291d-279">10</span></span> |

> [!NOTE]
> <span data-ttu-id="d291d-280">Poplatky za RU lišit v závislosti na hello počet vrácených položek.</span><span class="sxs-lookup"><span data-stu-id="d291d-280">RU charges vary based on hello number of items returned.</span></span>
> 
> 

<span data-ttu-id="d291d-281">Pomocí těchto informací jsme odhadnout hello RU požadavky pro toto číslo zadané aplikace hello operací a dotazy že Očekáváme, že za sekundu:</span><span class="sxs-lookup"><span data-stu-id="d291d-281">With this information, we can estimate hello RU requirements for this application given hello number of operations and queries we expect per second:</span></span>

| <span data-ttu-id="d291d-282">Operace nebo dotazu</span><span class="sxs-lookup"><span data-stu-id="d291d-282">Operation/Query</span></span> | <span data-ttu-id="d291d-283">Očekávaný počet za sekundu</span><span class="sxs-lookup"><span data-stu-id="d291d-283">Estimated number per second</span></span> | <span data-ttu-id="d291d-284">Požadované RUs</span><span class="sxs-lookup"><span data-stu-id="d291d-284">Required RUs</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d291d-285">Vytvoření položky</span><span class="sxs-lookup"><span data-stu-id="d291d-285">Create item</span></span> |<span data-ttu-id="d291d-286">10</span><span class="sxs-lookup"><span data-stu-id="d291d-286">10</span></span> |<span data-ttu-id="d291d-287">150</span><span class="sxs-lookup"><span data-stu-id="d291d-287">150</span></span> |
| <span data-ttu-id="d291d-288">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="d291d-288">Read item</span></span> |<span data-ttu-id="d291d-289">100</span><span class="sxs-lookup"><span data-stu-id="d291d-289">100</span></span> |<span data-ttu-id="d291d-290">100</span><span class="sxs-lookup"><span data-stu-id="d291d-290">100</span></span> |
| <span data-ttu-id="d291d-291">Vyberte potravin podle výrobce</span><span class="sxs-lookup"><span data-stu-id="d291d-291">Select foods by manufacturer</span></span> |<span data-ttu-id="d291d-292">25</span><span class="sxs-lookup"><span data-stu-id="d291d-292">25</span></span> |<span data-ttu-id="d291d-293">175</span><span class="sxs-lookup"><span data-stu-id="d291d-293">175</span></span> |
| <span data-ttu-id="d291d-294">Vyberte jídlo skupinou</span><span class="sxs-lookup"><span data-stu-id="d291d-294">Select by food group</span></span> |<span data-ttu-id="d291d-295">10</span><span class="sxs-lookup"><span data-stu-id="d291d-295">10</span></span> |<span data-ttu-id="d291d-296">700</span><span class="sxs-lookup"><span data-stu-id="d291d-296">700</span></span> |
| <span data-ttu-id="d291d-297">Vyberte nejvyšší 10</span><span class="sxs-lookup"><span data-stu-id="d291d-297">Select top 10</span></span> |<span data-ttu-id="d291d-298">15</span><span class="sxs-lookup"><span data-stu-id="d291d-298">15</span></span> |<span data-ttu-id="d291d-299">Celkem 150</span><span class="sxs-lookup"><span data-stu-id="d291d-299">150 Total</span></span> |

<span data-ttu-id="d291d-300">V takovém případě Očekáváme, že požadavek průměrnou propustností 1,275 RU/s.</span><span class="sxs-lookup"><span data-stu-id="d291d-300">In this case, we expect an average throughput requirement of 1,275 RU/s.</span></span>  <span data-ttu-id="d291d-301">Zaokrouhlení nahoru toohello nejbližší 100, jsme by zřídit 1 300 RU/s pro kolekci této aplikace.</span><span class="sxs-lookup"><span data-stu-id="d291d-301">Rounding up toohello nearest 100, we would provision 1,300 RU/s for this application's collection.</span></span>

## <span data-ttu-id="d291d-302"><a id="RequestRateTooLarge"></a>Překročení omezení vyhrazenou propustností v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d291d-302"><a id="RequestRateTooLarge"></a> Exceeding reserved throughput limits in Azure Cosmos DB</span></span>
<span data-ttu-id="d291d-303">Odvolat, že spotřeba jednotek žádosti budou vyhodnocené jako za sekundu Pokud hello rozpočtu je prázdný.</span><span class="sxs-lookup"><span data-stu-id="d291d-303">Recall that request unit consumption is evaluated as a rate per second if hello budget is empty.</span></span> <span data-ttu-id="d291d-304">Pro aplikace, které překračují hello míra jednotky zřízené požadavku pro kontejner, požadavků, že kolekce toothat budou omezeny, dokud hello rychlost klesne pod úroveň hello vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="d291d-304">For applications that exceed hello provisioned request unit rate for a container, requests toothat collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="d291d-305">Když dojde omezení, hello server se ukončí ho preventivně hello žádost s RequestRateTooLargeException (kód stavu HTTP 429) a návratové hello hlavičky x-ms opakování za ms informující o hello množství času v milisekundách, která hello uživatele čekat, než neúspěšných hello požadavek.</span><span class="sxs-lookup"><span data-stu-id="d291d-305">When a throttle occurs, hello server will preemptively end hello request with RequestRateTooLargeException (HTTP status code 429) and return hello x-ms-retry-after-ms header indicating hello amount of time, in milliseconds, that hello user must wait before reattempting hello request.</span></span>

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

<span data-ttu-id="d291d-306">Pokud používáte hello .NET klienta SDK a LINQ dotazů a pak většinu času hello toodeal s výjimku, máte nikdy jako hello aktuální verzi hello .NET klienta SDK implicitně zachytí této odpovědi, respektuje hello zadaný server opakovat po záhlaví, a žádost o hello opakování.</span><span class="sxs-lookup"><span data-stu-id="d291d-306">If you are using hello .NET Client SDK and LINQ queries, then most of hello time you never have toodeal with this exception, as hello current version of hello .NET Client SDK implicitly catches this response, respects hello server-specified retry-after header, and retries hello request.</span></span> <span data-ttu-id="d291d-307">Pokud váš účet je současně přistupuje více klientů, hello další pokus bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="d291d-307">Unless your account is being accessed concurrently by multiple clients, hello next retry will succeed.</span></span>

<span data-ttu-id="d291d-308">Pokud máte více než jednoho klienta kumulativně operační vyšší rychlost požadavků hello, hello výchozí chování opakování nemusí stačit a hello klienta vyvolá výjimku DocumentClientException s aplikaci toohello 429 kódu stavu.</span><span class="sxs-lookup"><span data-stu-id="d291d-308">If you have more than one client cumulatively operating above hello request rate, hello default retry behavior may not suffice, and hello client will throw a DocumentClientException with status code 429 toohello application.</span></span> <span data-ttu-id="d291d-309">V případech, jako je tato zvažte zpracování logiky aplikace chyba zpracování rutiny nebo zvýšení hello vyhrazenou propustností hello kontejneru a postup pro opakované.</span><span class="sxs-lookup"><span data-stu-id="d291d-309">In cases such as this, you may consider handling retry behavior and logic in your application's error handling routines or increasing hello reserved throughput for hello container.</span></span>

## <span data-ttu-id="d291d-310"><a id="RequestRateTooLargeAPIforMongoDB"></a>Překročení omezení vyhrazenou propustností v rozhraní API pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="d291d-310"><a id="RequestRateTooLargeAPIforMongoDB"></a> Exceeding reserved throughput limits in API for MongoDB</span></span>
<span data-ttu-id="d291d-311">Aplikace, které překračují jednotek žádosti hello zřízené pro kolekci budou omezeny, dokud hello rychlost klesne pod úroveň hello vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="d291d-311">Applications that exceed hello provisioned request units for a collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="d291d-312">Když dojde omezení, back-end hello se ukončí ho preventivně hello žádosti s *16500* kód chyby - *příliš mnoho požadavků*.</span><span class="sxs-lookup"><span data-stu-id="d291d-312">When a throttle occurs, hello backend will preemptively end hello request with a *16500* error code - *Too Many Requests*.</span></span> <span data-ttu-id="d291d-313">Ve výchozím nastavení, bude rozhraní API pro MongoDB automaticky opakovat too10 časů před vrácením *příliš mnoho požadavků* kód chyby.</span><span class="sxs-lookup"><span data-stu-id="d291d-313">By default, API for MongoDB will automatically retry up too10 times before returning a *Too Many Requests* error code.</span></span> <span data-ttu-id="d291d-314">Pokud se zobrazuje řada *příliš mnoho požadavků* kódy chyb, můžete zvážit buď přidání opakování chování vaší aplikace chyba zpracování rutiny nebo [zvýšení hello vyhrazenou propustností pro kolekci hello](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="d291d-314">If you are receiving many *Too Many Requests* error codes, you may consider either adding retry behavior in your application's error handling routines or [increasing hello reserved throughput for hello collection](set-throughput.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d291d-315">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d291d-315">Next steps</span></span>
<span data-ttu-id="d291d-316">toolearn Další informace o vyhrazenou propustností s databází Azure Cosmos DB těchto materiálech:</span><span class="sxs-lookup"><span data-stu-id="d291d-316">toolearn more about reserved throughput with Azure Cosmos DB databases, explore these resources:</span></span>

* [<span data-ttu-id="d291d-317">Azure Cosmos DB ceny</span><span class="sxs-lookup"><span data-stu-id="d291d-317">Azure Cosmos DB pricing</span></span>](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [<span data-ttu-id="d291d-318">Segmentace dat v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d291d-318">Partitioning data in Azure Cosmos DB</span></span>](partition-data.md)

<span data-ttu-id="d291d-319">toolearn Další informace o databázi Cosmos Azure, najdete v části hello Azure Cosmos DB [dokumentaci](https://azure.microsoft.com/documentation/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="d291d-319">toolearn more about Azure Cosmos DB, see hello Azure Cosmos DB [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/).</span></span> 

<span data-ttu-id="d291d-320">tooget začít s škálování a výkon testování pomocí Azure Cosmos DB, najdete v části [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="d291d-320">tooget started with scale and performance testing with Azure Cosmos DB, see [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md).</span></span>

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
