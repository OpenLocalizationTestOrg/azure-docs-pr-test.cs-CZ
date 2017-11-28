---
title: "aaaPartitioning a horizontální škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak tooconfigure vytváření oddílů a klíče oddílů a jak toopick hello právo oddílu klíč pro vaši aplikaci."
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
ms.openlocfilehash: 87d56db8c4ccc6b94b1650baff0fcfb3db6d1777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopartition-and-scale-in-azure-cosmos-db"></a><span data-ttu-id="eb116-103">Jak toopartition a škálování v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eb116-103">How toopartition and scale in Azure Cosmos DB</span></span>

<span data-ttu-id="eb116-104">[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je toohelp služba navržená tak, globální distribuované, více modelu databáze můžete dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem.</span><span class="sxs-lookup"><span data-stu-id="eb116-104">[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a global distributed, multi-model database service designed toohelp you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> <span data-ttu-id="eb116-105">Tento článek obsahuje přehled o tom, jak dělení platí pro všechna data hello modelů v Azure Cosmos DB a popisuje, jak konfigurovat Azure Cosmos DB kontejnery tooeffectively škálování aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb116-105">This article provides an overview of how partitioning works for all hello data models in Azure Cosmos DB, and describes how you can configure Azure Cosmos DB containers tooeffectively scale your applications.</span></span>

<span data-ttu-id="eb116-106">Dělení a klíče oddílů jsou také popsané v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Shireesh Thota pátek.</span><span class="sxs-lookup"><span data-stu-id="eb116-106">Partitioning and partition keys are also covered in this Azure Friday video with Scott Hanselman and Azure Cosmos DB Principal Engineering Manager, Shireesh Thota.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a><span data-ttu-id="eb116-107">Vytváření oddílů v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eb116-107">Partitioning in Azure Cosmos DB</span></span>
<span data-ttu-id="eb116-108">V Azure DB Cosmos můžete ukládat a zadávat dotazy na data bez schémat s pořadí milisekundu odezvy v jakémkoli měřítku.</span><span class="sxs-lookup"><span data-stu-id="eb116-108">In Azure Cosmos DB, you can store and query schema-less data with order-of-millisecond response times at any scale.</span></span> <span data-ttu-id="eb116-109">Cosmos DB poskytuje kontejnery pro ukládání dat volat **kolekce (pro dokument), grafy nebo tabulky**.</span><span class="sxs-lookup"><span data-stu-id="eb116-109">Cosmos DB provides containers for storing data called **collections (for document), graphs, or tables**.</span></span> <span data-ttu-id="eb116-110">Kontejnery jsou logické prostředky a může mít rozsah jeden nebo více fyzických oddílů nebo serverů.</span><span class="sxs-lookup"><span data-stu-id="eb116-110">Containers are logical resources and can span one or more physical partitions or servers.</span></span> <span data-ttu-id="eb116-111">Hello počet oddílů je dáno DB Cosmos na základě hello velikost úložiště a zřízené propustnosti hello hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eb116-111">hello number of partitions is determined by Cosmos DB based on hello storage size and hello provisioned throughput of hello container.</span></span> <span data-ttu-id="eb116-112">Každý oddíl v Cosmos DB má pevně stanovený objem zálohovaná na SSD úložiště s ním spojená a se replikují pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="eb116-112">Every partition in Cosmos DB has a fixed amount of SSD-backed storage associated with it, and is replicated for high availability.</span></span> <span data-ttu-id="eb116-113">Oddíl správy je plně spravovat Azure Cosmos DB a nemáte mít složitý kód toowrite nebo spravovat vaše oddíly.</span><span class="sxs-lookup"><span data-stu-id="eb116-113">Partition management is fully managed by Azure Cosmos DB, and you do not have toowrite complex code or manage your partitions.</span></span> <span data-ttu-id="eb116-114">Kontejnery DB cosmos neomezená z hlediska úložiště a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="eb116-114">Cosmos DB containers are unlimited in terms of storage and throughput.</span></span> 

![vodorovné](./media/introduction/azure-cosmos-db-partitioning.png) 

<span data-ttu-id="eb116-116">Vytváření oddílů je transparentní tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb116-116">Partitioning is transparent tooyour application.</span></span> <span data-ttu-id="eb116-117">Cosmos DB podporuje rychlé čtení a zápisy, dotazy, transakční logiku, úrovně konzistence a řízení přístupu podrobných prostřednictvím metody nebo rozhraní API tooa jediný kontejner prostředků.</span><span class="sxs-lookup"><span data-stu-id="eb116-117">Cosmos DB supports fast reads and writes, queries, transactional logic, consistency levels, and fine-grained access control via methods/APIs tooa single container resource.</span></span> <span data-ttu-id="eb116-118">Hello služba zpracovává rozděluje data mezi oddílů a směrování správné oddíl toohello dotazu žádosti.</span><span class="sxs-lookup"><span data-stu-id="eb116-118">hello service handles distributing data across partitions and routing query requests toohello right partition.</span></span> 

<span data-ttu-id="eb116-119">Jak funguje dělení</span><span class="sxs-lookup"><span data-stu-id="eb116-119">How does partitioning work?</span></span> <span data-ttu-id="eb116-120">Každá položka musí mít klíč oddílu a klíč řádku, které jeho jednoznačné identifikaci.</span><span class="sxs-lookup"><span data-stu-id="eb116-120">Each item must have a partition key and a row key, which uniquely identify it.</span></span> <span data-ttu-id="eb116-121">Klíč oddílu funguje jako logický oddíl pro vaše data a poskytne Cosmos DB přirozené hranice pro distribuci dat mezi oddílů.</span><span class="sxs-lookup"><span data-stu-id="eb116-121">Your partition key acts as a logical partition for your data, and provides Cosmos DB with a natural boundary for distributing data across partitions.</span></span> <span data-ttu-id="eb116-122">Stručně řečeno zde je Princip vytváření oddílů v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="eb116-122">In brief, here is how partitioning works in Azure Cosmos DB:</span></span>

* <span data-ttu-id="eb116-123">Zřídit Cosmos DB kontejner s `T` propustnost požadavky/s</span><span class="sxs-lookup"><span data-stu-id="eb116-123">You provision a Cosmos DB container with `T` requests/s throughput</span></span>
* <span data-ttu-id="eb116-124">Pozadí se děje hello Cosmos DB zřizuje oddíly potřeby tooserve `T` požadavky/s.</span><span class="sxs-lookup"><span data-stu-id="eb116-124">Behind hello scenes, Cosmos DB provisions partitions needed tooserve `T` requests/s.</span></span> <span data-ttu-id="eb116-125">Pokud `T` je vyšší než maximální propustnost hello na oddíl `t`, pak Cosmos DB zřizuje `N`  =  `T/t` oddíly</span><span class="sxs-lookup"><span data-stu-id="eb116-125">If `T` is higher than hello maximum throughput per partition `t`, then Cosmos DB provisions `N` = `T/t` partitions</span></span>
* <span data-ttu-id="eb116-126">Cosmos DB přiděluje hello klíče místo oddílu klíče hash rovnoměrně mezi hello `N` oddíly.</span><span class="sxs-lookup"><span data-stu-id="eb116-126">Cosmos DB allocates hello key space of partition key hashes evenly across hello `N` partitions.</span></span> <span data-ttu-id="eb116-127">Ano každý oddíl (fyzickém oddílu) hostitelem hodnoty klíče oddílu 1-N (logické oddíly)</span><span class="sxs-lookup"><span data-stu-id="eb116-127">So, each partition (physical partition) hosts 1-N partition key values (logical partitions)</span></span>
* <span data-ttu-id="eb116-128">Při fyzickém oddílu `p` dosáhnou limitu úložiště Cosmos DB bezproblémově rozdělí `p` do dvou nových oddílů `p1` a `p2` a distribuuje hodnoty odpovídající tooroughly poloviční hello klíče tooeach Dobrý den oddíly.</span><span class="sxs-lookup"><span data-stu-id="eb116-128">When a physical partition `p` reaches its storage limit, Cosmos DB seamlessly splits `p` into two new partitions `p1` and `p2` and distributes values corresponding tooroughly half hello keys tooeach of hello partitions.</span></span> <span data-ttu-id="eb116-129">Toto rozdělení operaci je neviditelná tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb116-129">This split operation is invisible tooyour application.</span></span>
* <span data-ttu-id="eb116-130">Podobně když zřídit propustnost vyšší než `t*N` propustnost, Cosmos DB rozdělí jeden nebo více vyšší propustnost vaší oddíly toosupport hello</span><span class="sxs-lookup"><span data-stu-id="eb116-130">Similarly, when you provision throughput higher than `t*N` throughput, Cosmos DB splits one or more of your partitions toosupport hello higher throughput</span></span>

<span data-ttu-id="eb116-131">Sémantika Hello pro klíče oddílu je mírně odlišný toomatch hello sémantiku každé rozhraní API, jak je znázorněno v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="eb116-131">hello semantics for partition keys are slightly different toomatch hello semantics of each API, as shown in hello following table:</span></span>

| <span data-ttu-id="eb116-132">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="eb116-132">API</span></span> | <span data-ttu-id="eb116-133">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="eb116-133">Partition Key</span></span> | <span data-ttu-id="eb116-134">Klíč řádku</span><span class="sxs-lookup"><span data-stu-id="eb116-134">Row Key</span></span> |
| --- | --- | --- |
| <span data-ttu-id="eb116-135">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="eb116-135">DocumentDB</span></span> | <span data-ttu-id="eb116-136">Cesta klíče vlastní oddíl</span><span class="sxs-lookup"><span data-stu-id="eb116-136">custom partition key path</span></span> | <span data-ttu-id="eb116-137">Pevná`id`</span><span class="sxs-lookup"><span data-stu-id="eb116-137">fixed `id`</span></span> | 
| <span data-ttu-id="eb116-138">MongoDB</span><span class="sxs-lookup"><span data-stu-id="eb116-138">MongoDB</span></span> | <span data-ttu-id="eb116-139">vlastní horizontálních klíč</span><span class="sxs-lookup"><span data-stu-id="eb116-139">custom shard key</span></span>  | <span data-ttu-id="eb116-140">Pevná`_id`</span><span class="sxs-lookup"><span data-stu-id="eb116-140">fixed `_id`</span></span> | 
| <span data-ttu-id="eb116-141">Graph</span><span class="sxs-lookup"><span data-stu-id="eb116-141">Graph</span></span> | <span data-ttu-id="eb116-142">klíčovou vlastností vlastní oddíl</span><span class="sxs-lookup"><span data-stu-id="eb116-142">custom partition key property</span></span> | <span data-ttu-id="eb116-143">Pevná`id`</span><span class="sxs-lookup"><span data-stu-id="eb116-143">fixed `id`</span></span> | 
| <span data-ttu-id="eb116-144">Table</span><span class="sxs-lookup"><span data-stu-id="eb116-144">Table</span></span> | <span data-ttu-id="eb116-145">Pevná`PartitionKey`</span><span class="sxs-lookup"><span data-stu-id="eb116-145">fixed `PartitionKey`</span></span> | <span data-ttu-id="eb116-146">Pevná`RowKey`</span><span class="sxs-lookup"><span data-stu-id="eb116-146">fixed `RowKey`</span></span> | 

<span data-ttu-id="eb116-147">Cosmos DB používá algoritmus HMAC rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="eb116-147">Cosmos DB uses hash-based partitioning.</span></span> <span data-ttu-id="eb116-148">Při zápisu položky Cosmos DB hashuje hodnotu klíče oddílu hello a použití hello rozdělí výsledek toodetermine která oddílu toostore hello položka v.</span><span class="sxs-lookup"><span data-stu-id="eb116-148">When you write an item, Cosmos DB hashes hello partition key value and use hello hashed result toodetermine which partition toostore hello item in.</span></span> <span data-ttu-id="eb116-149">Hello cosmos DB úložiště hello všechny položky se stejným klíčem oddílu v jednom fyzickém oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-149">Cosmos DB stores all items with hello same partition key in hello same physical partition.</span></span> <span data-ttu-id="eb116-150">Vybraná Hello hello klíč oddílu je rozhodnutí o důležité, abyste měli toomake v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="eb116-150">hello choice of hello partition key is an important decision that you have toomake at design time.</span></span> <span data-ttu-id="eb116-151">Je třeba vybrat název vlastnosti, která má široký rozsah hodnot a má i přístupové vzorce.</span><span class="sxs-lookup"><span data-stu-id="eb116-151">You must pick a property name that has a wide range of values and has even access patterns.</span></span>

> [!NOTE]
> <span data-ttu-id="eb116-152">Je nejlepší postup toohave klíč oddílu s mnoha jedinečných hodnot (100s-1000s minimálně).</span><span class="sxs-lookup"><span data-stu-id="eb116-152">It is a best practice toohave a partition key with many distinct values (100s-1000s at a minimum).</span></span>
>

<span data-ttu-id="eb116-153">Kontejnery Azure Cosmos DB se dá vytvořit jako "Pevná" nebo "neomezená."</span><span class="sxs-lookup"><span data-stu-id="eb116-153">Azure Cosmos DB containers can be created as "fixed" or "unlimited."</span></span> <span data-ttu-id="eb116-154">Kontejnery pevné velikosti mají maximální limit 10 GB a propustnost 10 000 RU/s.</span><span class="sxs-lookup"><span data-stu-id="eb116-154">Fixed-size containers have a maximum limit of 10 GB and 10,000 RU/s throughput.</span></span> <span data-ttu-id="eb116-155">Některé rozhraní API umožňují toobe klíče oddílu hello vynechání kontejnerů s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="eb116-155">Some APIs allow hello partition key toobe omitted for fixed-size containers.</span></span> <span data-ttu-id="eb116-156">toocreate kontejneru jako neomezená, je nutné zadat minimální propustnost 2 500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="eb116-156">toocreate a container as unlimited, you must specify a minimum throughput of 2500 RU/s.</span></span>

## <a name="partitioning-and-provisioned-throughput"></a><span data-ttu-id="eb116-157">Vytváření oddílů a zřízené propustnosti</span><span class="sxs-lookup"><span data-stu-id="eb116-157">Partitioning and provisioned throughput</span></span>
<span data-ttu-id="eb116-158">Cosmos DB je určená pro předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="eb116-158">Cosmos DB is designed for predictable performance.</span></span> <span data-ttu-id="eb116-159">Když vytvoříte kontejner, můžete vyhradit propustnost z hlediska  **[požadované jednotky](request-units.md) (RU) za sekundu s potenciální rozšíření pro RU za minutu**.</span><span class="sxs-lookup"><span data-stu-id="eb116-159">When you create a container, you reserve throughput in terms of **[request units](request-units.md) (RU) per second with a potential add-on for RU per minute**.</span></span> <span data-ttu-id="eb116-160">Každý požadavek není přiřazen nákladů jednotek žádosti, která je přiměřené toohello množství systémové prostředky jako procesoru, paměti a spotřebovávají hello operaci vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="eb116-160">Each request is assigned a request unit charge that is proportionate toohello amount of system resources like CPU, Memory, and IO consumed by hello operation.</span></span> <span data-ttu-id="eb116-161">Čtení 1 KB dokumentu s konzistence typu relace spotřebuje jednu jednotku požadavku.</span><span class="sxs-lookup"><span data-stu-id="eb116-161">A read of a 1-KB document with Session consistency consumes one request unit.</span></span> <span data-ttu-id="eb116-162">Pro čtení je 1 RU bez ohledu na počet hello položek, které uložené nebo hello počet souběžných požadavků, které jsou spuštěné v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="eb116-162">A read is 1 RU regardless of hello number of items stored or hello number of concurrent requests running at hello same time.</span></span> <span data-ttu-id="eb116-163">Položky větší vyžadují vyšší jednotek žádosti v závislosti na velikosti hello.</span><span class="sxs-lookup"><span data-stu-id="eb116-163">Larger items require higher request units depending on hello size.</span></span> <span data-ttu-id="eb116-164">Pokud znáte hello velikost vašeho entit a hello počet čtení toosupport pro vaši aplikaci, můžete zřídit hello přesné množství propustnost požadované pro vaše aplikace je vyžaduje čtení.</span><span class="sxs-lookup"><span data-stu-id="eb116-164">If you know hello size of your entities and hello number of reads you need toosupport for your application, you can provision hello exact amount of throughput required for your application's read needs.</span></span> 

> [!NOTE]
> <span data-ttu-id="eb116-165">hello úplnou propustnost tooachieve hello kontejneru, musíte zvolit, klíč oddílu, který vám umožní tooevenly distribuci požadavků mezi některé hodnoty klíče jedinečné oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-165">tooachieve hello full throughput of hello container, you must choose a partition key that allows you tooevenly distribute requests among some distinct partition key values.</span></span>
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-hello-azure-cosmos-db-apis"></a><span data-ttu-id="eb116-166">Práce s hello rozhraní API Správce Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eb116-166">Working with hello Azure Cosmos DB APIs</span></span>
<span data-ttu-id="eb116-167">Můžete použít hello portál Azure nebo Azure CLI toocreate kontejnerů a škálování je kdykoli.</span><span class="sxs-lookup"><span data-stu-id="eb116-167">You can use hello Azure portal or Azure CLI toocreate containers and scale them at any time.</span></span> <span data-ttu-id="eb116-168">Tato část uvádí, jak toocreate kontejnery a zadejte hello propustnost a oddíl definici klíče v každé z hello podporované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="eb116-168">This section shows how toocreate containers and specify hello throughput and partition key definition in each of hello supported APIs.</span></span>

### <a name="documentdb-api"></a><span data-ttu-id="eb116-169">Rozhraní DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="eb116-169">DocumentDB API</span></span>
<span data-ttu-id="eb116-170">Následující ukázka Hello ukazuje, jak hello toocreate kontejneru (kolekce) pomocí rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="eb116-170">hello following sample shows how toocreate a container (collection) using hello DocumentDB API.</span></span> <span data-ttu-id="eb116-171">Můžete najít další podrobnosti v [rozdělení do oddílů s rozhraním API DocumentDB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="eb116-171">You can find more details in [Partitioning with DocumentDB API](partition-data.md).</span></span>

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

<span data-ttu-id="eb116-172">Můžete si přečíst položku (dokument) pomocí hello `GET` metoda v hello REST API nebo pomocí `ReadDocumentAsync` v jednom z hello sady SDK.</span><span class="sxs-lookup"><span data-stu-id="eb116-172">You can read an item (document) using hello `GET` method in hello REST API or using `ReadDocumentAsync` in one of hello SDKs.</span></span>

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a><span data-ttu-id="eb116-173">Rozhraní MongoDB API</span><span class="sxs-lookup"><span data-stu-id="eb116-173">MongoDB API</span></span>
<span data-ttu-id="eb116-174">S hello MongoDB rozhraní API můžete vytvořit kolekci horizontálně dělené prostřednictvím vaše oblíbené nástroje, ovladače nebo sady SDK.</span><span class="sxs-lookup"><span data-stu-id="eb116-174">With hello MongoDB API, you can create a sharded collection through your favorite tool, driver, or SDK.</span></span> <span data-ttu-id="eb116-175">V tomto příkladu používáme hello prostředí Mongo pro vytvoření kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="eb116-175">In this example, we use hello Mongo Shell for hello collection creation.</span></span>

<span data-ttu-id="eb116-176">V hello prostředí Mongo:</span><span class="sxs-lookup"><span data-stu-id="eb116-176">In hello Mongo Shell:</span></span>

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
<span data-ttu-id="eb116-177">Výsledky:</span><span class="sxs-lookup"><span data-stu-id="eb116-177">Results:</span></span>

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a><span data-ttu-id="eb116-178">Rozhraní Table API</span><span class="sxs-lookup"><span data-stu-id="eb116-178">Table API</span></span>

<span data-ttu-id="eb116-179">Pomocí hello API tabulky je určit hello propustnost pro tabulky v hello appSettings konfiguraci pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="eb116-179">With hello Table API, you specify hello throughput for tables in hello appSettings configuration for your application:</span></span>

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

<span data-ttu-id="eb116-180">Potom můžete vytvořit tabulku pomocí Azure Table storage hello SDK.</span><span class="sxs-lookup"><span data-stu-id="eb116-180">Then you create a table using hello Azure Table storage SDK.</span></span> <span data-ttu-id="eb116-181">klíč oddílu Hello je vytvořena implicitně jako hello `PartitionKey` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eb116-181">hello partition key is implicitly created as hello `PartitionKey` value.</span></span> 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

<span data-ttu-id="eb116-182">Můžete načíst jednu entitu pomocí hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="eb116-182">You can retrieve a single entity using hello following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
<span data-ttu-id="eb116-183">V tématu [vývoj s hello tabulky API](tutorial-develop-table-dotnet.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="eb116-183">See [Developing with hello Table API](tutorial-develop-table-dotnet.md) for more details.</span></span>

### <a name="graph-api"></a><span data-ttu-id="eb116-184">Graph API</span><span class="sxs-lookup"><span data-stu-id="eb116-184">Graph API</span></span>

<span data-ttu-id="eb116-185">S hello rozhraní Graph API musíte použít hello portál Azure nebo rozhraní příkazového řádku toocreate kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="eb116-185">With hello Graph API, you must use hello Azure portal or CLI toocreate containers.</span></span> <span data-ttu-id="eb116-186">Alternativně vzhledem k tomu, že Azure Cosmos DB je více modelu, můžete použít jeden hello toocreate ostatní modely a škálovat vaše grafu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eb116-186">Alternatively, since Azure Cosmos DB is multi-model, you can use one of hello other models toocreate and scale your graph container.</span></span>

<span data-ttu-id="eb116-187">Může číst všechny vrchol nebo Microsoft edge v Gremlin hello klíč oddílu a id.</span><span class="sxs-lookup"><span data-stu-id="eb116-187">You can read any vertex or edge using hello partition key and id in Gremlin.</span></span> <span data-ttu-id="eb116-188">Graf s oblasti ("USA") jako klíč oddílu hello a "Seattle" jako klíč řádku hello, například můžete najít pomocí následující syntaxe hello vrchol:</span><span class="sxs-lookup"><span data-stu-id="eb116-188">For example, for a graph with region ("USA") as hello partition key, and "Seattle" as hello row key, you can find a vertex using hello following syntax:</span></span>

```
g.V(['USA', 'Seattle'])
```

<span data-ttu-id="eb116-189">Stejné jako u okraje, můžete odkazovat pomocí hello klíč oddílu a klíč řádku okraj.</span><span class="sxs-lookup"><span data-stu-id="eb116-189">Same with edges, you can reference an edge using hello partition key and row key.</span></span>

```
g.E(['USA', 'I5'])
```

<span data-ttu-id="eb116-190">V tématu [Gremlin podpora systému Cosmos DB](gremlin-support.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="eb116-190">See [Gremlin support for Cosmos DB](gremlin-support.md) for more details.</span></span>


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a><span data-ttu-id="eb116-191">Návrh a vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="eb116-191">Designing for partitioning</span></span>
<span data-ttu-id="eb116-192">tooscale efektivně s Azure Cosmos DB, musíte toopick vhodným klíčem oddílu při vytváření vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eb116-192">tooscale effectively with Azure Cosmos DB, you need toopick a good partition key when you create your container.</span></span> <span data-ttu-id="eb116-193">Existují dva klíčové faktory pro výběr klíč oddílu:</span><span class="sxs-lookup"><span data-stu-id="eb116-193">There are two key considerations for choosing a partition key:</span></span>

* <span data-ttu-id="eb116-194">**Hranice pro dotaz a transakce**: zvoleného klíč oddílu by měl vyrovnávat hello nutné tooenable hello použití transakce proti hello požadavek toodistribute vaší entity ve více tooensure klíče oddílu škálovatelné řešení.</span><span class="sxs-lookup"><span data-stu-id="eb116-194">**Boundary for query and transactions**: Your choice of partition key should balance hello need tooenable hello use of transactions against hello requirement toodistribute your entities across multiple partition keys tooensure a scalable solution.</span></span> <span data-ttu-id="eb116-195">V jedné extreme, můžete nastavit hello stejným klíčem oddílu pro všechny položky, ale to může omezit škálovatelnost hello vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="eb116-195">At one extreme, you could set hello same partition key for all your items, but this may limit hello scalability of your solution.</span></span> <span data-ttu-id="eb116-196">V hello jiných extreme můžete přiřadit oddílu jedinečný klíč pro každou položku, která by byla vysoce škálovatelné, ale by bránily použití transakcí křížové dokumentu prostřednictvím uložených procedur a aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="eb116-196">At hello other extreme, you could assign a unique partition key for each item, which would be highly scalable but would prevent you from using cross document transactions via stored procedures and triggers.</span></span> <span data-ttu-id="eb116-197">Klíč ideální oddílu je jedna, díky kterému toouse efektivní dotazy a který má dostatečná mohutnost tooensure řešení je škálovatelná.</span><span class="sxs-lookup"><span data-stu-id="eb116-197">An ideal partition key is one that enables you toouse efficient queries and that has sufficient cardinality tooensure your solution is scalable.</span></span> 
* <span data-ttu-id="eb116-198">**Žádné úložiště a výkon kritických bodů**: je důležité toopick vlastnost, která umožňuje zapíše toobe rozmístěny v různých jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="eb116-198">**No storage and performance bottlenecks**: It is important toopick a property that allows writes toobe distributed across various distinct values.</span></span> <span data-ttu-id="eb116-199">Požadavky toohello stejným klíčem oddílu nesmí být delší než hello propustnost jednoho oddílu a jsou omezené.</span><span class="sxs-lookup"><span data-stu-id="eb116-199">Requests toohello same partition key cannot exceed hello throughput of a single partition, and are throttled.</span></span> <span data-ttu-id="eb116-200">Proto je důležité toopick klíč oddílu, který nemá za následek "aktivní body" v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="eb116-200">So it is important toopick a partition key that does not result in "hot spots" within your application.</span></span> <span data-ttu-id="eb116-201">Od všech hello dat pro jeden oddíl klíč musí být uložen v rámci oddílu, je také vhodné tooavoid klíče oddílů, které mají velký objem dat pro hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eb116-201">Since all hello data for a single partition key must be stored within a partition, it is also recommended tooavoid partition keys that have high volumes of data for hello same value.</span></span> 

<span data-ttu-id="eb116-202">Podívejme se na několik reálných scénářů a klíče dobrý oddílů pro každou:</span><span class="sxs-lookup"><span data-stu-id="eb116-202">Let's look at a few real-world scenarios, and good partition keys for each:</span></span>
* <span data-ttu-id="eb116-203">Pokud jste implementace back-end profilu uživatele, hello ID uživatele je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-203">If you’re implementing a user profile backend, then hello user ID is a good choice for partition key.</span></span>
* <span data-ttu-id="eb116-204">Pokud ukládáte data IoT například stavu zařízení, je ID zařízení dobrou volbou pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-204">If you’re storing IoT data for example, device state, a device ID is a good choice for partition key.</span></span>
* <span data-ttu-id="eb116-205">Pokud používáte Azure Cosmos DB pro protokolování data časové řady, pak hello název hostitele nebo ID procesu je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-205">If you’re using Azure Cosmos DB for logging time-series data, then hello hostname or process ID is a good choice for partition key.</span></span>
* <span data-ttu-id="eb116-206">Pokud máte víceklientské architektuře, hello ID klienta je vhodná pro klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-206">If you have a multi-tenant architecture, hello tenant ID is a good choice for partition key.</span></span>

<span data-ttu-id="eb116-207">V některých případech jako IoT použití a uživatelské profily, hello klíč oddílu může být hello stejné jako vaše id (klíč dokumentu).</span><span class="sxs-lookup"><span data-stu-id="eb116-207">In some use cases like IoT and user profiles, hello partition key might be hello same as your id (document key).</span></span> <span data-ttu-id="eb116-208">V jiných jako data hello časové řady může mít klíč oddílu, který se liší od hello id.</span><span class="sxs-lookup"><span data-stu-id="eb116-208">In others like hello time series data, you might have a partition key that’s different than hello id.</span></span>

### <a name="partitioning-and-loggingtime-series-data"></a><span data-ttu-id="eb116-209">Vytváření oddílů a protokolování nebo časových řad dat</span><span class="sxs-lookup"><span data-stu-id="eb116-209">Partitioning and logging/time-series data</span></span>
<span data-ttu-id="eb116-210">Mezi běžné případy použití hello Cosmos databáze je pro protokolování a telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eb116-210">One of hello common use cases of Cosmos DB is for logging and telemetry.</span></span> <span data-ttu-id="eb116-211">Vzhledem k tomu může být nutné tooread a zápis obrovské objemy dat je důležité toopick vhodným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-211">It is important toopick a good partition key since you might need tooread/write vast volumes of data.</span></span> <span data-ttu-id="eb116-212">Výběr Hello závisí na čtení a zápisu sazby a typy dotazů očekávat toorun.</span><span class="sxs-lookup"><span data-stu-id="eb116-212">hello choice depends on your read and write rates and kinds of queries you expect toorun.</span></span> <span data-ttu-id="eb116-213">Tady jsou některé tipy, jak toochoose vhodným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-213">Here are some tips on how toochoose a good partition key.</span></span>

* <span data-ttu-id="eb116-214">Pokud váš případ použití zahrnuje malý počet zapíše pak pomocí souhrnu hello časové razítko, například datum jako klíč oddílu je dobrý způsob shromažďování po dlouhou dobu, a nutnost tooquery podle rozsahů časová razítka a ostatní filtry.</span><span class="sxs-lookup"><span data-stu-id="eb116-214">If your use case involves a small rate of writes accumulating over a long period of time, and need tooquery by ranges of timestamps and other filters, then using a rollup of hello timestamp, for example,  date as a partition key is a good approach.</span></span> <span data-ttu-id="eb116-215">To vám umožní tooquery přes všechny hello data pro datum z jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="eb116-215">This allows you tooquery over all hello data for a date from a single partition.</span></span> 
* <span data-ttu-id="eb116-216">Pokud vaše úlohy je napsán náročné, což je více obvyklé, používejte klíč oddílu, který není založen na časové razítko tak, aby Cosmos DB můžete rovnoměrně distribuovat zápisy mezi různé oddíly.</span><span class="sxs-lookup"><span data-stu-id="eb116-216">If your workload is written heavy, which is more common, you should use a partition key that’s not based on timestamp so that Cosmos DB can distribute writes evenly across various partitions.</span></span> <span data-ttu-id="eb116-217">Název hostitele, ID procesu, ID aktivity nebo jinou vlastnost s vysokou kardinalitou tady je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="eb116-217">Here a hostname, process ID, activity ID, or another property with high cardinality is a good choice.</span></span> 
* <span data-ttu-id="eb116-218">Třetí přístup je hybridní, jeden kde máte více kontejnerů, jednu pro každý den/měsíc a klíč oddílu hello je podrobné vlastnosti, jako je název hostitele.</span><span class="sxs-lookup"><span data-stu-id="eb116-218">A third approach is a hybrid one where you have multiple containers, one for each day/month and hello partition key is a granular property like hostname.</span></span> <span data-ttu-id="eb116-219">Tato akce nemá hello výhody, kterou můžete nastavit různé propustnost podle hello časový interval, například hello kontejner pro aktuální měsíc hello je opatřen vyšší propustnost vzhledem k tomu, že funguje čtení a zápisu, zatímco předchozích měsíců s nižší propustnost od slouží jenom čtení.</span><span class="sxs-lookup"><span data-stu-id="eb116-219">This has hello benefit that you can set different throughput based on hello time window, for example, hello container for hello current month is provisioned with higher throughput since it serves reads and writes, whereas previous months with lower throughput since they only serve reads.</span></span>

### <a name="partitioning-and-multi-tenancy"></a><span data-ttu-id="eb116-220">Vytváření oddílů a víceklientské prostředí</span><span class="sxs-lookup"><span data-stu-id="eb116-220">Partitioning and multi-tenancy</span></span>
<span data-ttu-id="eb116-221">Pokud implementujete víceklientské aplikace pomocí Cosmos DB, existují dva oblíbených vzory – jeden oddíl klíč každého klienta a jednoho kontejneru typu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="eb116-221">If you are implementing a multi-tenant application using Cosmos DB, there are two popular patterns – one partition key per tenant, and one container per tenant.</span></span> <span data-ttu-id="eb116-222">Zde jsou hello výhody a nevýhody pro každou:</span><span class="sxs-lookup"><span data-stu-id="eb116-222">Here are hello pros and cons for each:</span></span>

* <span data-ttu-id="eb116-223">Jeden klíč oddílu každého klienta: V tomto modelu, klienti jsou společně umístěná v rámci jednoho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eb116-223">One Partition Key per tenant: In this model, tenants are collocated within a single container.</span></span> <span data-ttu-id="eb116-224">Ale dotazy a vložení pro položky v rámci jednoho klienta je možné provádět proti jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="eb116-224">But queries and inserts for items within a single tenant can be performed against a single partition.</span></span> <span data-ttu-id="eb116-225">Můžete také implementovat logiku transakcí mezi všechny položky v rámci klienta.</span><span class="sxs-lookup"><span data-stu-id="eb116-225">You can also implement transactional logic across all items within a tenant.</span></span> <span data-ttu-id="eb116-226">Vzhledem k tomu, že více klientů sdílet kontejner, můžete uložit náklady na úložiště a propustnost sdružování prostředků pro klienty v rámci jednoho kontejneru spíše než zřizování navíc rezervou pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="eb116-226">Since multiple tenants share a container, you can save storage and throughput costs by pooling resources for tenants within a single container rather than provisioning extra headroom for each tenant.</span></span> <span data-ttu-id="eb116-227">Nevýhodou Hello je nemají izolaci výkonu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="eb116-227">hello drawback is that you do not have performance isolation per tenant.</span></span> <span data-ttu-id="eb116-228">Zvýšení výkonu nebo propustnost použít toohello celého kontejneru vs cílové zvyšuje pro klienty.</span><span class="sxs-lookup"><span data-stu-id="eb116-228">Performance/throughput increases apply toohello entire container vs targeted increases for tenants.</span></span>
* <span data-ttu-id="eb116-229">Jeden kontejner každého klienta: vlastní kontejner má každý klient.</span><span class="sxs-lookup"><span data-stu-id="eb116-229">One Container per tenant: Each tenant has its own container.</span></span> <span data-ttu-id="eb116-230">V tomto modelu je možné rezervovat výkonu každého klienta.</span><span class="sxs-lookup"><span data-stu-id="eb116-230">In this model, you can reserve performance per tenant.</span></span> <span data-ttu-id="eb116-231">Tento model je s Cosmos DB nový zajišťování cenový model pro víceklientské aplikace cenově výhodnější s několika klienty.</span><span class="sxs-lookup"><span data-stu-id="eb116-231">With Cosmos DB's new provisioning pricing model, this model is more cost-effective for multi-tenant applications with a few tenants.</span></span>

<span data-ttu-id="eb116-232">Také můžete použít kombinaci/vrstvené přístup, collocates malé klientů a migraci větší klienty tootheir vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="eb116-232">You can also use a combination/tiered approach that collocates small tenants and migrates larger tenants tootheir own container.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb116-233">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eb116-233">Next steps</span></span>
<span data-ttu-id="eb116-234">V tomto článku jsme Přehled poskytuje přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="eb116-234">In this article, we provided an overview for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="eb116-235">Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="eb116-235">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>
* <span data-ttu-id="eb116-236">Další informace o [globální distribuce v Azure Cosmos DB](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="eb116-236">Learn about [global distribution in Azure Cosmos DB](distribute-data-globally.md)</span></span>



