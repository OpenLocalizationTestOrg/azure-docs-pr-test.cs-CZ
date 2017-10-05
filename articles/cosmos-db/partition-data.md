---
title: "Vytváření oddílů a horizontální škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak nakonfigurovat, vytváření oddílů a oddílu klíče a jak vybrat klíč správné oddílu pro vaši aplikaci."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2d2847276e553d7511241ff323c3e00aad8e5c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-partition-and-scale-in-azure-cosmos-db"></a><span data-ttu-id="c6d94-103">Vytvoření oddílů a škálování v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c6d94-103">How to partition and scale in Azure Cosmos DB</span></span>

<span data-ttu-id="c6d94-104">[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je globální databáze distribuované, více modelu služba navržená tak, aby vám pomohou dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem.</span><span class="sxs-lookup"><span data-stu-id="c6d94-104">[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a global distributed, multi-model database service designed to help you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> <span data-ttu-id="c6d94-105">Tento článek obsahuje přehled jak rozdělení funguje u všech datových modelech v Azure Cosmos DB a popisuje konfiguraci kontejnery Azure Cosmos DB efektivní škálování vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="c6d94-105">This article provides an overview of how partitioning works for all the data models in Azure Cosmos DB, and describes how you can configure Azure Cosmos DB containers to effectively scale your applications.</span></span>

<span data-ttu-id="c6d94-106">Dělení a klíče oddílů jsou také popsané v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Shireesh Thota pátek.</span><span class="sxs-lookup"><span data-stu-id="c6d94-106">Partitioning and partition keys are also covered in this Azure Friday video with Scott Hanselman and Azure Cosmos DB Principal Engineering Manager, Shireesh Thota.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a><span data-ttu-id="c6d94-107">Vytváření oddílů v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c6d94-107">Partitioning in Azure Cosmos DB</span></span>
<span data-ttu-id="c6d94-108">V Azure DB Cosmos můžete ukládat a zadávat dotazy na data bez schémat s pořadí milisekundu odezvy v jakémkoli měřítku.</span><span class="sxs-lookup"><span data-stu-id="c6d94-108">In Azure Cosmos DB, you can store and query schema-less data with order-of-millisecond response times at any scale.</span></span> <span data-ttu-id="c6d94-109">Cosmos DB poskytuje kontejnery pro ukládání dat volat **kolekce (pro dokument), grafy nebo tabulky**.</span><span class="sxs-lookup"><span data-stu-id="c6d94-109">Cosmos DB provides containers for storing data called **collections (for document), graphs, or tables**.</span></span> <span data-ttu-id="c6d94-110">Kontejnery jsou logické prostředky a může mít rozsah jeden nebo více fyzických oddílů nebo serverů.</span><span class="sxs-lookup"><span data-stu-id="c6d94-110">Containers are logical resources and can span one or more physical partitions or servers.</span></span> <span data-ttu-id="c6d94-111">Počet oddílů je dáno DB Cosmos na základě velikosti úložiště a zřízené propustnosti kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c6d94-111">The number of partitions is determined by Cosmos DB based on the storage size and the provisioned throughput of the container.</span></span> <span data-ttu-id="c6d94-112">Každý oddíl v Cosmos DB má pevně stanovený objem zálohovaná na SSD úložiště s ním spojená a se replikují pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="c6d94-112">Every partition in Cosmos DB has a fixed amount of SSD-backed storage associated with it, and is replicated for high availability.</span></span> <span data-ttu-id="c6d94-113">Oddíl správy je plně spravovat Azure Cosmos DB a není nutné zapsat složitý kód nebo spravovat vaše oddíly.</span><span class="sxs-lookup"><span data-stu-id="c6d94-113">Partition management is fully managed by Azure Cosmos DB, and you do not have to write complex code or manage your partitions.</span></span> <span data-ttu-id="c6d94-114">Kontejnery DB cosmos neomezená z hlediska úložiště a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="c6d94-114">Cosmos DB containers are unlimited in terms of storage and throughput.</span></span> 

![vodorovné](./media/introduction/azure-cosmos-db-partitioning.png) 

<span data-ttu-id="c6d94-116">Vytváření oddílů je transparentní do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6d94-116">Partitioning is transparent to your application.</span></span> <span data-ttu-id="c6d94-117">Cosmos DB podporuje rychlé čtení a zápisy, dotazy, transakční logiku, úrovně konzistence a řízení přístupu podrobných prostřednictvím metody nebo rozhraní API pro jediný kontejner prostředků.</span><span class="sxs-lookup"><span data-stu-id="c6d94-117">Cosmos DB supports fast reads and writes, queries, transactional logic, consistency levels, and fine-grained access control via methods/APIs to a single container resource.</span></span> <span data-ttu-id="c6d94-118">Služba zpracovává distribuce data mezi oddílů a směrování požadavků na dotazy do správné oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-118">The service handles distributing data across partitions and routing query requests to the right partition.</span></span> 

<span data-ttu-id="c6d94-119">Jak funguje dělení</span><span class="sxs-lookup"><span data-stu-id="c6d94-119">How does partitioning work?</span></span> <span data-ttu-id="c6d94-120">Každá položka musí mít klíč oddílu a klíč řádku, které jeho jednoznačné identifikaci.</span><span class="sxs-lookup"><span data-stu-id="c6d94-120">Each item must have a partition key and a row key, which uniquely identify it.</span></span> <span data-ttu-id="c6d94-121">Klíč oddílu funguje jako logický oddíl pro vaše data a poskytne Cosmos DB přirozené hranice pro distribuci dat mezi oddílů.</span><span class="sxs-lookup"><span data-stu-id="c6d94-121">Your partition key acts as a logical partition for your data, and provides Cosmos DB with a natural boundary for distributing data across partitions.</span></span> <span data-ttu-id="c6d94-122">Stručně řečeno zde je Princip vytváření oddílů v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="c6d94-122">In brief, here is how partitioning works in Azure Cosmos DB:</span></span>

* <span data-ttu-id="c6d94-123">Zřídit Cosmos DB kontejner s `T` propustnost požadavky/s</span><span class="sxs-lookup"><span data-stu-id="c6d94-123">You provision a Cosmos DB container with `T` requests/s throughput</span></span>
* <span data-ttu-id="c6d94-124">Na pozadí Cosmos DB zřídí oddíly, které jsou potřebné k obsluze `T` požadavky/s.</span><span class="sxs-lookup"><span data-stu-id="c6d94-124">Behind the scenes, Cosmos DB provisions partitions needed to serve `T` requests/s.</span></span> <span data-ttu-id="c6d94-125">Pokud `T` je vyšší než maximální propustnost na oddíl `t`, pak Cosmos DB zřizuje `N`  =  `T/t` oddíly</span><span class="sxs-lookup"><span data-stu-id="c6d94-125">If `T` is higher than the maximum throughput per partition `t`, then Cosmos DB provisions `N` = `T/t` partitions</span></span>
* <span data-ttu-id="c6d94-126">Cosmos DB přiděluje místo na klíče oddílu klíče hash rovnoměrně napříč `N` oddíly.</span><span class="sxs-lookup"><span data-stu-id="c6d94-126">Cosmos DB allocates the key space of partition key hashes evenly across the `N` partitions.</span></span> <span data-ttu-id="c6d94-127">Ano každý oddíl (fyzickém oddílu) hostitelem hodnoty klíče oddílu 1-N (logické oddíly)</span><span class="sxs-lookup"><span data-stu-id="c6d94-127">So, each partition (physical partition) hosts 1-N partition key values (logical partitions)</span></span>
* <span data-ttu-id="c6d94-128">Při fyzickém oddílu `p` dosáhnou limitu úložiště Cosmos DB bezproblémově rozdělí `p` do dvou nových oddílů `p1` a `p2` a distribuuje hodnoty odpovídající přibližně poloviční klíče pro každou nadefinovaných oddílů.</span><span class="sxs-lookup"><span data-stu-id="c6d94-128">When a physical partition `p` reaches its storage limit, Cosmos DB seamlessly splits `p` into two new partitions `p1` and `p2` and distributes values corresponding to roughly half the keys to each of the partitions.</span></span> <span data-ttu-id="c6d94-129">Toto rozdělení operace je pro vaše aplikace skrytá.</span><span class="sxs-lookup"><span data-stu-id="c6d94-129">This split operation is invisible to your application.</span></span>
* <span data-ttu-id="c6d94-130">Podobně když zřídit propustnost vyšší než `t*N` propustnost, Cosmos DB rozdělí jeden nebo více oddíly mohou podporovat vyšší propustnost</span><span class="sxs-lookup"><span data-stu-id="c6d94-130">Similarly, when you provision throughput higher than `t*N` throughput, Cosmos DB splits one or more of your partitions to support the higher throughput</span></span>

<span data-ttu-id="c6d94-131">Sémantika pro klíče oddílů je mírně odlišný tak, aby odpovídaly sémantika každé rozhraní API, jak je znázorněno v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="c6d94-131">The semantics for partition keys are slightly different to match the semantics of each API, as shown in the following table:</span></span>

| <span data-ttu-id="c6d94-132">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c6d94-132">API</span></span> | <span data-ttu-id="c6d94-133">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="c6d94-133">Partition Key</span></span> | <span data-ttu-id="c6d94-134">Klíč řádku</span><span class="sxs-lookup"><span data-stu-id="c6d94-134">Row Key</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c6d94-135">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="c6d94-135">DocumentDB</span></span> | <span data-ttu-id="c6d94-136">Cesta klíče vlastní oddíl</span><span class="sxs-lookup"><span data-stu-id="c6d94-136">custom partition key path</span></span> | <span data-ttu-id="c6d94-137">Pevná`id`</span><span class="sxs-lookup"><span data-stu-id="c6d94-137">fixed `id`</span></span> | 
| <span data-ttu-id="c6d94-138">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c6d94-138">MongoDB</span></span> | <span data-ttu-id="c6d94-139">vlastní horizontálních klíč</span><span class="sxs-lookup"><span data-stu-id="c6d94-139">custom shard key</span></span>  | <span data-ttu-id="c6d94-140">Pevná`_id`</span><span class="sxs-lookup"><span data-stu-id="c6d94-140">fixed `_id`</span></span> | 
| <span data-ttu-id="c6d94-141">Graph</span><span class="sxs-lookup"><span data-stu-id="c6d94-141">Graph</span></span> | <span data-ttu-id="c6d94-142">klíčovou vlastností vlastní oddíl</span><span class="sxs-lookup"><span data-stu-id="c6d94-142">custom partition key property</span></span> | <span data-ttu-id="c6d94-143">Pevná`id`</span><span class="sxs-lookup"><span data-stu-id="c6d94-143">fixed `id`</span></span> | 
| <span data-ttu-id="c6d94-144">Table</span><span class="sxs-lookup"><span data-stu-id="c6d94-144">Table</span></span> | <span data-ttu-id="c6d94-145">Pevná`PartitionKey`</span><span class="sxs-lookup"><span data-stu-id="c6d94-145">fixed `PartitionKey`</span></span> | <span data-ttu-id="c6d94-146">Pevná`RowKey`</span><span class="sxs-lookup"><span data-stu-id="c6d94-146">fixed `RowKey`</span></span> | 

<span data-ttu-id="c6d94-147">Cosmos DB používá algoritmus HMAC rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="c6d94-147">Cosmos DB uses hash-based partitioning.</span></span> <span data-ttu-id="c6d94-148">Při zápisu položky databáze Cosmos hashuje hodnotu klíče oddílu a hash výsledek použít k určení oddíl, který k uložení položky v.</span><span class="sxs-lookup"><span data-stu-id="c6d94-148">When you write an item, Cosmos DB hashes the partition key value and use the hashed result to determine which partition to store the item in.</span></span> <span data-ttu-id="c6d94-149">Cosmos DB ukládá všechny položky se stejným klíčem oddílu v jednom fyzickém oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-149">Cosmos DB stores all items with the same partition key in the same physical partition.</span></span> <span data-ttu-id="c6d94-150">Volba klíč oddílu je důležité rozhodnutí, která je nutné provést v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-150">The choice of the partition key is an important decision that you have to make at design time.</span></span> <span data-ttu-id="c6d94-151">Je třeba vybrat název vlastnosti, která má široký rozsah hodnot a má i přístupové vzorce.</span><span class="sxs-lookup"><span data-stu-id="c6d94-151">You must pick a property name that has a wide range of values and has even access patterns.</span></span>

> [!NOTE]
> <span data-ttu-id="c6d94-152">Je vhodné mít klíč oddílu s mnoha jedinečných hodnot (100s-1000s minimálně).</span><span class="sxs-lookup"><span data-stu-id="c6d94-152">It is a best practice to have a partition key with many distinct values (100s-1000s at a minimum).</span></span>
>

<span data-ttu-id="c6d94-153">Kontejnery Azure Cosmos DB se dá vytvořit jako "Pevná" nebo "neomezená."</span><span class="sxs-lookup"><span data-stu-id="c6d94-153">Azure Cosmos DB containers can be created as "fixed" or "unlimited."</span></span> <span data-ttu-id="c6d94-154">Kontejnery pevné velikosti mají maximální limit 10 GB a propustnost 10 000 RU/s.</span><span class="sxs-lookup"><span data-stu-id="c6d94-154">Fixed-size containers have a maximum limit of 10 GB and 10,000 RU/s throughput.</span></span> <span data-ttu-id="c6d94-155">Některé rozhraní API umožňují klíč oddílu vynechává kontejnerů s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="c6d94-155">Some APIs allow the partition key to be omitted for fixed-size containers.</span></span> <span data-ttu-id="c6d94-156">Chcete-li vytvořit kontejner jako neomezená, musíte zadat minimální propustnost 2 500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="c6d94-156">To create a container as unlimited, you must specify a minimum throughput of 2500 RU/s.</span></span>

## <a name="partitioning-and-provisioned-throughput"></a><span data-ttu-id="c6d94-157">Vytváření oddílů a zřízené propustnosti</span><span class="sxs-lookup"><span data-stu-id="c6d94-157">Partitioning and provisioned throughput</span></span>
<span data-ttu-id="c6d94-158">Cosmos DB je určená pro předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="c6d94-158">Cosmos DB is designed for predictable performance.</span></span> <span data-ttu-id="c6d94-159">Když vytvoříte kontejner, můžete vyhradit propustnost z hlediska  **[požadované jednotky](request-units.md) (RU) za sekundu s potenciální rozšíření pro RU za minutu**.</span><span class="sxs-lookup"><span data-stu-id="c6d94-159">When you create a container, you reserve throughput in terms of **[request units](request-units.md) (RU) per second with a potential add-on for RU per minute**.</span></span> <span data-ttu-id="c6d94-160">Každý požadavek je přiřazený nákladů jednotky žádosti, která je úměrné množství systémové prostředky jako procesoru, paměti a spotřebovávají operaci vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="c6d94-160">Each request is assigned a request unit charge that is proportionate to the amount of system resources like CPU, Memory, and IO consumed by the operation.</span></span> <span data-ttu-id="c6d94-161">Čtení 1 KB dokumentu s konzistence typu relace spotřebuje jednu jednotku požadavku.</span><span class="sxs-lookup"><span data-stu-id="c6d94-161">A read of a 1-KB document with Session consistency consumes one request unit.</span></span> <span data-ttu-id="c6d94-162">Pro čtení je 1 RU bez ohledu na počet položek, které jsou uložené nebo počet souběžných požadavků, které se spouští ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-162">A read is 1 RU regardless of the number of items stored or the number of concurrent requests running at the same time.</span></span> <span data-ttu-id="c6d94-163">Položky větší vyžadují vyšší jednotek žádosti v závislosti na velikosti.</span><span class="sxs-lookup"><span data-stu-id="c6d94-163">Larger items require higher request units depending on the size.</span></span> <span data-ttu-id="c6d94-164">Pokud znáte velikost vaší entity a počet čtení, které budete potřebovat k podpoře pro aplikaci, můžete zřídit přesné množství propustnost požadované pro vaše aplikace je vyžaduje čtení.</span><span class="sxs-lookup"><span data-stu-id="c6d94-164">If you know the size of your entities and the number of reads you need to support for your application, you can provision the exact amount of throughput required for your application's read needs.</span></span> 

> [!NOTE]
> <span data-ttu-id="c6d94-165">K dosažení úplné propustnosti kontejneru, je nutné zvolit klíč oddílu, který umožňuje rovnoměrně rozdělit požadavky mezi některé hodnoty klíče jedinečné oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-165">To achieve the full throughput of the container, you must choose a partition key that allows you to evenly distribute requests among some distinct partition key values.</span></span>
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-the-azure-cosmos-db-apis"></a><span data-ttu-id="c6d94-166">Práce s Azure Cosmos DB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c6d94-166">Working with the Azure Cosmos DB APIs</span></span>
<span data-ttu-id="c6d94-167">Portál Azure nebo rozhraní příkazového řádku Azure slouží k vytvoření kontejnerů a škálování je kdykoli.</span><span class="sxs-lookup"><span data-stu-id="c6d94-167">You can use the Azure portal or Azure CLI to create containers and scale them at any time.</span></span> <span data-ttu-id="c6d94-168">Tato část ukazuje způsob vytvoření kontejnerů a zadejte definici klíče propustnost a oddílu v každé z podporovaných rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c6d94-168">This section shows how to create containers and specify the throughput and partition key definition in each of the supported APIs.</span></span>

### <a name="documentdb-api"></a><span data-ttu-id="c6d94-169">Rozhraní DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="c6d94-169">DocumentDB API</span></span>
<span data-ttu-id="c6d94-170">Následující příklad ukazuje, jak vytvořit kontejner (kolekce) pomocí rozhraní API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="c6d94-170">The following sample shows how to create a container (collection) using the DocumentDB API.</span></span> <span data-ttu-id="c6d94-171">Můžete najít další podrobnosti v [rozdělení do oddílů s rozhraním API DocumentDB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="c6d94-171">You can find more details in [Partitioning with DocumentDB API](partition-data.md).</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

<span data-ttu-id="c6d94-172">Můžete si přečíst k položky (dokument) pomocí `GET` metoda v rozhraní API REST nebo pomocí `ReadDocumentAsync` v jednom ze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c6d94-172">You can read an item (document) using the `GET` method in the REST API or using `ReadDocumentAsync` in one of the SDKs.</span></span>

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a><span data-ttu-id="c6d94-173">Rozhraní MongoDB API</span><span class="sxs-lookup"><span data-stu-id="c6d94-173">MongoDB API</span></span>
<span data-ttu-id="c6d94-174">S rozhraním API pro MongoDB můžete vytvořit kolekci horizontálně dělené prostřednictvím vaše oblíbené nástroje, ovladače nebo sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c6d94-174">With the MongoDB API, you can create a sharded collection through your favorite tool, driver, or SDK.</span></span> <span data-ttu-id="c6d94-175">V tomto příkladu používáme prostředí Mongo pro vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="c6d94-175">In this example, we use the Mongo Shell for the collection creation.</span></span>

<span data-ttu-id="c6d94-176">V prostředí Mongo:</span><span class="sxs-lookup"><span data-stu-id="c6d94-176">In the Mongo Shell:</span></span>

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
<span data-ttu-id="c6d94-177">Výsledky:</span><span class="sxs-lookup"><span data-stu-id="c6d94-177">Results:</span></span>

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a><span data-ttu-id="c6d94-178">Rozhraní Table API</span><span class="sxs-lookup"><span data-stu-id="c6d94-178">Table API</span></span>

<span data-ttu-id="c6d94-179">Pomocí rozhraní API tabulky je určit propustnost pro tabulky v konfiguraci appSettings pro vaši aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c6d94-179">With the Table API, you specify the throughput for tables in the appSettings configuration for your application:</span></span>

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

<span data-ttu-id="c6d94-180">Potom můžete vytvořit tabulku pomocí Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="c6d94-180">Then you create a table using the Azure Table storage SDK.</span></span> <span data-ttu-id="c6d94-181">Klíč oddílu je vytvořena implicitně jako `PartitionKey` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-181">The partition key is implicitly created as the `PartitionKey` value.</span></span> 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

<span data-ttu-id="c6d94-182">Můžete načíst jednu entitu pomocí následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c6d94-182">You can retrieve a single entity using the following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
<span data-ttu-id="c6d94-183">V tématu [vývoj s rozhraním API pro tabulku](tutorial-develop-table-dotnet.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c6d94-183">See [Developing with the Table API](tutorial-develop-table-dotnet.md) for more details.</span></span>

### <a name="graph-api"></a><span data-ttu-id="c6d94-184">Graph API</span><span class="sxs-lookup"><span data-stu-id="c6d94-184">Graph API</span></span>

<span data-ttu-id="c6d94-185">Rozhraní Graph API musíte použít portál Azure nebo rozhraní příkazového řádku k vytvoření kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c6d94-185">With the Graph API, you must use the Azure portal or CLI to create containers.</span></span> <span data-ttu-id="c6d94-186">Alternativně vzhledem k tomu, že Azure Cosmos DB je více modelu, můžete další modely vytvořit a škálovat vaše kontejneru grafu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-186">Alternatively, since Azure Cosmos DB is multi-model, you can use one of the other models to create and scale your graph container.</span></span>

<span data-ttu-id="c6d94-187">Může číst všechny vrchol nebo Microsoft edge v Gremlin klíč oddílu a id.</span><span class="sxs-lookup"><span data-stu-id="c6d94-187">You can read any vertex or edge using the partition key and id in Gremlin.</span></span> <span data-ttu-id="c6d94-188">Graf s oblasti ("USA") jako klíč oddílu a "Seattle" jako klíč řádku, například můžete najít vrchol pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="c6d94-188">For example, for a graph with region ("USA") as the partition key, and "Seattle" as the row key, you can find a vertex using the following syntax:</span></span>

```
g.V(['USA', 'Seattle'])
```

<span data-ttu-id="c6d94-189">Stejné jako u okraje, můžete odkazovat pomocí klíč oddílu a klíč řádku okraj.</span><span class="sxs-lookup"><span data-stu-id="c6d94-189">Same with edges, you can reference an edge using the partition key and row key.</span></span>

```
g.E(['USA', 'I5'])
```

<span data-ttu-id="c6d94-190">V tématu [Gremlin podpora systému Cosmos DB](gremlin-support.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c6d94-190">See [Gremlin support for Cosmos DB](gremlin-support.md) for more details.</span></span>


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a><span data-ttu-id="c6d94-191">Návrh a vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="c6d94-191">Designing for partitioning</span></span>
<span data-ttu-id="c6d94-192">Efektivní škálování s Azure Cosmos DB, musíte vybrat vhodným klíčem oddílu, při vytváření vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c6d94-192">To scale effectively with Azure Cosmos DB, you need to pick a good partition key when you create your container.</span></span> <span data-ttu-id="c6d94-193">Existují dva klíčové faktory pro výběr klíč oddílu:</span><span class="sxs-lookup"><span data-stu-id="c6d94-193">There are two key considerations for choosing a partition key:</span></span>

* <span data-ttu-id="c6d94-194">**Hranice pro dotaz a transakce**: zvoleného klíč oddílu by měl vyvážit potřeba povolit používání transakce proti požadavku, distribuovat vaší entity napříč více klíčů oddílů zajistit škálovatelné řešení.</span><span class="sxs-lookup"><span data-stu-id="c6d94-194">**Boundary for query and transactions**: Your choice of partition key should balance the need to enable the use of transactions against the requirement to distribute your entities across multiple partition keys to ensure a scalable solution.</span></span> <span data-ttu-id="c6d94-195">V jedné extreme stejným klíčem oddílu můžete nastavit pro všechny položky, ale to může omezit škálovatelnost řešení.</span><span class="sxs-lookup"><span data-stu-id="c6d94-195">At one extreme, you could set the same partition key for all your items, but this may limit the scalability of your solution.</span></span> <span data-ttu-id="c6d94-196">V jiných extreme může přiřadit oddílu jedinečný klíč pro každou položku, která by byla vysoce škálovatelné, ale by bránily použití transakcí křížové dokumentu prostřednictvím uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="c6d94-196">At the other extreme, you could assign a unique partition key for each item, which would be highly scalable but would prevent you from using cross document transactions via stored procedures and triggers.</span></span> <span data-ttu-id="c6d94-197">Klíč ideální oddílu je jeden, který vám umožňuje používat efektivní dotazy a má dostatek mohutnost zajistit, že vaše řešení je škálovatelná.</span><span class="sxs-lookup"><span data-stu-id="c6d94-197">An ideal partition key is one that enables you to use efficient queries and that has sufficient cardinality to ensure your solution is scalable.</span></span> 
* <span data-ttu-id="c6d94-198">**Žádné úložiště a výkon kritických bodů**: je důležité vybrat vlastnost, která umožňuje zápis být distribuován do různých jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="c6d94-198">**No storage and performance bottlenecks**: It is important to pick a property that allows writes to be distributed across various distinct values.</span></span> <span data-ttu-id="c6d94-199">Požadavky na stejným klíčem oddílu nesmí být delší než propustnost jednoho oddílu a jsou omezené.</span><span class="sxs-lookup"><span data-stu-id="c6d94-199">Requests to the same partition key cannot exceed the throughput of a single partition, and are throttled.</span></span> <span data-ttu-id="c6d94-200">Proto je důležité vybrat klíč oddílu, který nemá za následek "aktivní body" v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6d94-200">So it is important to pick a partition key that does not result in "hot spots" within your application.</span></span> <span data-ttu-id="c6d94-201">Vzhledem k tomu, že všechna data pro jeden oddíl klíč musí být uložen v rámci oddílu, doporučujeme také Vyhněte se klíče oddílů, které mají velký objem dat pro stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-201">Since all the data for a single partition key must be stored within a partition, it is also recommended to avoid partition keys that have high volumes of data for the same value.</span></span> 

<span data-ttu-id="c6d94-202">Podívejme se na několik reálných scénářů a klíče dobrý oddílů pro každou:</span><span class="sxs-lookup"><span data-stu-id="c6d94-202">Let's look at a few real-world scenarios, and good partition keys for each:</span></span>
* <span data-ttu-id="c6d94-203">Pokud jste implementace back-end profilu uživatele, ID uživatele je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-203">If you’re implementing a user profile backend, then the user ID is a good choice for partition key.</span></span>
* <span data-ttu-id="c6d94-204">Pokud ukládáte data IoT například stavu zařízení, je ID zařízení dobrou volbou pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-204">If you’re storing IoT data for example, device state, a device ID is a good choice for partition key.</span></span>
* <span data-ttu-id="c6d94-205">Pokud používáte Azure Cosmos DB pro protokolování data časové řady, název hostitele nebo proces ID je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-205">If you’re using Azure Cosmos DB for logging time-series data, then the hostname or process ID is a good choice for partition key.</span></span>
* <span data-ttu-id="c6d94-206">Pokud máte víceklientské architektuře, ID klienta je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-206">If you have a multi-tenant architecture, the tenant ID is a good choice for partition key.</span></span>

<span data-ttu-id="c6d94-207">V některých případech jako IoT a uživatelské profily použití, klíč oddílu může být stejný jako vaše id (klíč dokumentu).</span><span class="sxs-lookup"><span data-stu-id="c6d94-207">In some use cases like IoT and user profiles, the partition key might be the same as your id (document key).</span></span> <span data-ttu-id="c6d94-208">V jiných jako data časové řady může mít klíč oddílu, který se liší od id.</span><span class="sxs-lookup"><span data-stu-id="c6d94-208">In others like the time series data, you might have a partition key that’s different than the id.</span></span>

### <a name="partitioning-and-loggingtime-series-data"></a><span data-ttu-id="c6d94-209">Vytváření oddílů a protokolování nebo časových řad dat</span><span class="sxs-lookup"><span data-stu-id="c6d94-209">Partitioning and logging/time-series data</span></span>
<span data-ttu-id="c6d94-210">Mezi běžné případy použití Cosmos databáze je pro protokolování a telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c6d94-210">One of the common use cases of Cosmos DB is for logging and telemetry.</span></span> <span data-ttu-id="c6d94-211">Je důležité vybrat vhodným klíčem oddílu, protože může být nutné obrovské objemy dat pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="c6d94-211">It is important to pick a good partition key since you might need to read/write vast volumes of data.</span></span> <span data-ttu-id="c6d94-212">Výběr závisí na čtení a zápisu sazby a typy dotazů, které chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="c6d94-212">The choice depends on your read and write rates and kinds of queries you expect to run.</span></span> <span data-ttu-id="c6d94-213">Tady jsou některé tipy, jak zvolit vhodným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-213">Here are some tips on how to choose a good partition key.</span></span>

* <span data-ttu-id="c6d94-214">Pokud váš případ použití zahrnuje malý počet zapíše pak pomocí souhrnu časové razítko, například datum jako klíč oddílu je dobrý způsob shromažďování po dlouhou dobu a potřeba dotazu rozsahy časová razítka a ostatní filtry.</span><span class="sxs-lookup"><span data-stu-id="c6d94-214">If your use case involves a small rate of writes accumulating over a long period of time, and need to query by ranges of timestamps and other filters, then using a rollup of the timestamp, for example,  date as a partition key is a good approach.</span></span> <span data-ttu-id="c6d94-215">To umožňuje dotazu přes všechna data pro datum z jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="c6d94-215">This allows you to query over all the data for a date from a single partition.</span></span> 
* <span data-ttu-id="c6d94-216">Pokud vaše úlohy je napsán náročné, což je více obvyklé, používejte klíč oddílu, který není založen na časové razítko tak, aby Cosmos DB můžete rovnoměrně distribuovat zápisy mezi různé oddíly.</span><span class="sxs-lookup"><span data-stu-id="c6d94-216">If your workload is written heavy, which is more common, you should use a partition key that’s not based on timestamp so that Cosmos DB can distribute writes evenly across various partitions.</span></span> <span data-ttu-id="c6d94-217">Název hostitele, ID procesu, ID aktivity nebo jinou vlastnost s vysokou kardinalitou tady je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="c6d94-217">Here a hostname, process ID, activity ID, or another property with high cardinality is a good choice.</span></span> 
* <span data-ttu-id="c6d94-218">Třetí přístup je hybridní, jeden kde máte více kontejnerů, jednu pro každý den/měsíc a klíč oddílu je podrobné vlastnosti, jako je název hostitele.</span><span class="sxs-lookup"><span data-stu-id="c6d94-218">A third approach is a hybrid one where you have multiple containers, one for each day/month and the partition key is a granular property like hostname.</span></span> <span data-ttu-id="c6d94-219">Výhodou je, kterou můžete nastavit různé propustnost podle časový interval, například kontejner pro aktuální měsíc je opatřen vyšší propustnost vzhledem k tomu, že funguje čtení a zápisu, zatímco předchozích měsíců s nižší propustnost od jejich pouze sloužit čtení.</span><span class="sxs-lookup"><span data-stu-id="c6d94-219">This has the benefit that you can set different throughput based on the time window, for example, the container for the current month is provisioned with higher throughput since it serves reads and writes, whereas previous months with lower throughput since they only serve reads.</span></span>

### <a name="partitioning-and-multi-tenancy"></a><span data-ttu-id="c6d94-220">Vytváření oddílů a víceklientské prostředí</span><span class="sxs-lookup"><span data-stu-id="c6d94-220">Partitioning and multi-tenancy</span></span>
<span data-ttu-id="c6d94-221">Pokud implementujete víceklientské aplikace pomocí Cosmos DB, existují dva oblíbených vzory – jeden oddíl klíč každého klienta a jednoho kontejneru typu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c6d94-221">If you are implementing a multi-tenant application using Cosmos DB, there are two popular patterns – one partition key per tenant, and one container per tenant.</span></span> <span data-ttu-id="c6d94-222">Zde jsou výhody a nevýhody pro každou:</span><span class="sxs-lookup"><span data-stu-id="c6d94-222">Here are the pros and cons for each:</span></span>

* <span data-ttu-id="c6d94-223">Jeden klíč oddílu každého klienta: V tomto modelu, klienti jsou společně umístěná v rámci jednoho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c6d94-223">One Partition Key per tenant: In this model, tenants are collocated within a single container.</span></span> <span data-ttu-id="c6d94-224">Ale dotazy a vložení pro položky v rámci jednoho klienta je možné provádět proti jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="c6d94-224">But queries and inserts for items within a single tenant can be performed against a single partition.</span></span> <span data-ttu-id="c6d94-225">Můžete také implementovat logiku transakcí mezi všechny položky v rámci klienta.</span><span class="sxs-lookup"><span data-stu-id="c6d94-225">You can also implement transactional logic across all items within a tenant.</span></span> <span data-ttu-id="c6d94-226">Vzhledem k tomu, že více klientů sdílet kontejner, můžete uložit náklady na úložiště a propustnost sdružování prostředků pro klienty v rámci jednoho kontejneru spíše než zřizování navíc rezervou pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c6d94-226">Since multiple tenants share a container, you can save storage and throughput costs by pooling resources for tenants within a single container rather than provisioning extra headroom for each tenant.</span></span> <span data-ttu-id="c6d94-227">Nevýhodou je, nemají izolaci výkonu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c6d94-227">The drawback is that you do not have performance isolation per tenant.</span></span> <span data-ttu-id="c6d94-228">Zvýšení výkonu nebo propustnost platí pro celou kontejneru vs cílové zvyšuje pro klienty.</span><span class="sxs-lookup"><span data-stu-id="c6d94-228">Performance/throughput increases apply to the entire container vs targeted increases for tenants.</span></span>
* <span data-ttu-id="c6d94-229">Jeden kontejner každého klienta: vlastní kontejner má každý klient.</span><span class="sxs-lookup"><span data-stu-id="c6d94-229">One Container per tenant: Each tenant has its own container.</span></span> <span data-ttu-id="c6d94-230">V tomto modelu je možné rezervovat výkonu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c6d94-230">In this model, you can reserve performance per tenant.</span></span> <span data-ttu-id="c6d94-231">Tento model je s Cosmos DB nový zajišťování cenový model pro víceklientské aplikace cenově výhodnější s několika klienty.</span><span class="sxs-lookup"><span data-stu-id="c6d94-231">With Cosmos DB's new provisioning pricing model, this model is more cost-effective for multi-tenant applications with a few tenants.</span></span>

<span data-ttu-id="c6d94-232">Můžete také vrstvené nebo kombinaci přístup, collocates malé klientů a migraci větší klienty do svých vlastních kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c6d94-232">You can also use a combination/tiered approach that collocates small tenants and migrates larger tenants to their own container.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6d94-233">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6d94-233">Next steps</span></span>
<span data-ttu-id="c6d94-234">V tomto článku jsme Přehled poskytuje přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c6d94-234">In this article, we provided an overview for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="c6d94-235">Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="c6d94-235">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>
* <span data-ttu-id="c6d94-236">Další informace o [globální distribuce v Azure Cosmos DB](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="c6d94-236">Learn about [global distribution in Azure Cosmos DB](distribute-data-globally.md)</span></span>



