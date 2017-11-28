---
title: "aaaPartitioning a škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak tooconfigure vytváření oddílů a klíče oddílů a jak toopick hello právo oddílu klíč pro vaši aplikaci."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 702c39b4-1798-48dd-9993-4493a2f6df9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30621d2ba0b89efb72005680d5f3a73998347514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a><span data-ttu-id="e5303-103">Vytváření oddílů v Azure DB Cosmos pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e5303-103">Partitioning in Azure Cosmos DB using hello DocumentDB API</span></span>

<span data-ttu-id="e5303-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) je toohelp služba navržená tak, globální distribuované, více modelu databáze můžete dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem.</span><span class="sxs-lookup"><span data-stu-id="e5303-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is a global distributed, multi-model database service designed toohelp you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> 

<span data-ttu-id="e5303-105">Tento článek obsahuje přehled o tom, jak toowork s oddíly Cosmos DB kontejnery s hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5303-105">This article provides an overview of how toowork with partitioning of Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="e5303-106">V tématu [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5303-106">See [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

<span data-ttu-id="e5303-107">tooget spuštění s kódem, stáhněte si projekt hello z [Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="e5303-107">tooget started with code, download hello project from [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<span data-ttu-id="e5303-108">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="e5303-108">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="e5303-109">Jak funguje vytváření oddílů v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="e5303-109">How does partitioning work in Azure Cosmos DB?</span></span>
* <span data-ttu-id="e5303-110">Jak nakonfigurovat, vytváření oddílů v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e5303-110">How do I configure partitioning in Azure Cosmos DB</span></span>
* <span data-ttu-id="e5303-111">Jaké jsou klíče oddílů a jak vyberte klíč oddílu správné hello své aplikaci?</span><span class="sxs-lookup"><span data-stu-id="e5303-111">What are partition keys, and how do I pick hello right partition key for my application?</span></span>

<span data-ttu-id="e5303-112">tooget spuštění s kódem, stáhněte si projekt hello z [Azure Cosmos DB výkonu testování ovladačů Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="e5303-112">tooget started with code, download hello project from [Azure Cosmos DB Performance Testing Driver Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a><span data-ttu-id="e5303-113">Klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="e5303-113">Partition keys</span></span>

<span data-ttu-id="e5303-114">V hello rozhraní API DocumentDB zadejte definici hello klíče oddílu v hello formu cesty JSON.</span><span class="sxs-lookup"><span data-stu-id="e5303-114">In hello DocumentDB API, you specify hello partition key definition in hello form of a JSON path.</span></span> <span data-ttu-id="e5303-115">Hello následující tabulka uvádí příklady oddílu klíčové definice a hello hodnoty odpovídající tooeach.</span><span class="sxs-lookup"><span data-stu-id="e5303-115">hello following table shows examples of partition key definitions and hello values corresponding tooeach.</span></span> <span data-ttu-id="e5303-116">klíč oddílu Hello je zadán jako cesta, například `/department` představuje hello vlastnost oddělení.</span><span class="sxs-lookup"><span data-stu-id="e5303-116">hello partition key is specified as a path, e.g. `/department` represents hello property department.</span></span> 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="e5303-117"><strong>Klíč oddílu</strong></span><span class="sxs-lookup"><span data-stu-id="e5303-117"><strong>Partition Key</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e5303-118"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="e5303-118"><strong>Description</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e5303-119">/Department</span><span class="sxs-lookup"><span data-stu-id="e5303-119">/department</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e5303-120">Odpovídá toohello hodnotu doc.department, kde je dokumentace hello položky.</span><span class="sxs-lookup"><span data-stu-id="e5303-120">Corresponds toohello value of doc.department where doc is hello item.</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e5303-121">/ vlastnosti/název</span><span class="sxs-lookup"><span data-stu-id="e5303-121">/properties/name</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e5303-122">Odpovídá toohello hodnotu doc.properties.name, kde je dokumentace hello položky (vnořené vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="e5303-122">Corresponds toohello value of doc.properties.name where doc is hello item (nested property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e5303-123">/ID</span><span class="sxs-lookup"><span data-stu-id="e5303-123">/id</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e5303-124">Odpovídá toohello hodnotu doc.id (id a oddílu klíče jsou hello stejné vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="e5303-124">Corresponds toohello value of doc.id (id and partition key are hello same property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e5303-125">/ ""oddělení název</span><span class="sxs-lookup"><span data-stu-id="e5303-125">/"department name"</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e5303-126">Odpovídá toohello hodnotu doc ["název oddělení"], kde je dokumentace hello položky.</span><span class="sxs-lookup"><span data-stu-id="e5303-126">Corresponds toohello value of doc["department name"] where doc is hello item.</span></span></p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> <span data-ttu-id="e5303-127">Hello syntaxe pro klíč oddílu je podobné specifikace toohello cesty pro indexování zásad cesty s hello klíče rozdílem, že hello cesta odpovídá vlastnosti toohello namísto hodnoty hello, tj. žádné zástupný znak není na konci hello.</span><span class="sxs-lookup"><span data-stu-id="e5303-127">hello syntax for partition key is similar toohello path specification for indexing policy paths with hello key difference that hello path corresponds toohello property instead of hello value, i.e. there is no wild card at hello end.</span></span> <span data-ttu-id="e5303-128">Například zadali byste/oddělení /?</span><span class="sxs-lookup"><span data-stu-id="e5303-128">For example, you would specify /department/?</span></span> <span data-ttu-id="e5303-129">tooindex hello hodnoty podle oddělení, ale zadat /department jako hello definici klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="e5303-129">tooindex hello values under department, but specify /department as hello partition key definition.</span></span> <span data-ttu-id="e5303-130">klíč oddílu Hello je implicitně indexovaný a nelze vyloučit z indexování pomocí přepsání indexování zásad.</span><span class="sxs-lookup"><span data-stu-id="e5303-130">hello partition key is implicitly indexed and cannot be excluded from indexing using indexing policy overrides.</span></span>
> 
> 

<span data-ttu-id="e5303-131">Podíváme, jak volba hello klíč oddílu ovlivňuje hello výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5303-131">Let's look at how hello choice of partition key impacts hello performance of your application.</span></span>

## <a name="working-with-hello-azure-cosmos-db-sdks"></a><span data-ttu-id="e5303-132">Práce s hello SDK služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e5303-132">Working with hello Azure Cosmos DB SDKs</span></span>
<span data-ttu-id="e5303-133">Azure Cosmos DB přidala se podpora pro automatické vytváření oddílů s [REST API verze 2015-12-16](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="e5303-133">Azure Cosmos DB added support for automatic partitioning with [REST API version 2015-12-16](/rest/api/documentdb/).</span></span> <span data-ttu-id="e5303-134">V kontejnerech pořadí toocreate rozdělena na oddíly, je nutné stáhnout sadu SDK verze 1.6.0 nebo novější v jednom z hello podporována SDK platformy (.NET, Node.js, Java, Python, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="e5303-134">In order toocreate partitioned containers, you must download SDK versions 1.6.0 or newer in one of hello supported SDK platforms (.NET, Node.js, Java, Python, MongoDB).</span></span> 

### <a name="creating-containers"></a><span data-ttu-id="e5303-135">Vytvoření kontejnerů</span><span class="sxs-lookup"><span data-stu-id="e5303-135">Creating containers</span></span>
<span data-ttu-id="e5303-136">Hello následující příklad ukazuje toocreate fragment kódu rozhraní .NET kontejner toostore zařízení telemetrická data z 20 000 jednotek žádosti za sekundu, propustnosti.</span><span class="sxs-lookup"><span data-stu-id="e5303-136">hello following sample shows a .NET snippet toocreate a container toostore device telemetry data of 20,000 request units per second of throughput.</span></span> <span data-ttu-id="e5303-137">Hello SDK nastaví hodnotu OfferThroughput hello (který naopak nastaví hello `x-ms-offer-throughput` hlavička požadavku v hello REST API).</span><span class="sxs-lookup"><span data-stu-id="e5303-137">hello SDK sets hello OfferThroughput value (which in turn sets hello `x-ms-offer-throughput` request header in hello REST API).</span></span> <span data-ttu-id="e5303-138">Zde jsme nastavit hello `/deviceId` jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="e5303-138">Here we set hello `/deviceId` as hello partition key.</span></span> <span data-ttu-id="e5303-139">Volba Hello klíč oddílu se uloží spolu s hello zbytek hello metadata kontejneru jako název a zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="e5303-139">hello choice of partition key is saved along with hello rest of hello container metadata like name and indexing policy.</span></span>

<span data-ttu-id="e5303-140">Tato ukázka jsme zachyceny `deviceId` vzhledem k tomu, že nám vědět, že (a) od existuje velký počet zařízení, zápisy mohou být distribuovány na oddíly rovnoměrně a umožňuje nám tooscale hello databáze tooingest ohromné objemy dat a (b) mnoho požadavků hello jako načítání hello nejnovější čtení pro zařízení jsou deviceId jednoho oboru tooa a je možné načíst z jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="e5303-140">For this sample, we picked `deviceId` since we know that (a) since there are a large number of devices, writes can be distributed across partitions evenly and allowing us tooscale hello database tooingest massive volumes of data and (b) many of hello requests like fetching hello latest reading for a device are scoped tooa single deviceId and can be retrieved from a single partition.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here hello property deviceId will be used as hello partition key too
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

<span data-ttu-id="e5303-141">Tato metoda umožňuje tooCosmos DB volání rozhraní REST API a služby hello zřizovat počet oddílů založené na požadovaný propustnost hello.</span><span class="sxs-lookup"><span data-stu-id="e5303-141">This method makes a REST API call tooCosmos DB, and hello service will provision a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="e5303-142">Výkon vašich potřeb momentální, můžete změnit hello propustnost kontejner.</span><span class="sxs-lookup"><span data-stu-id="e5303-142">You can change hello throughput of a container as your performance needs evolve.</span></span> 

### <a name="reading-and-writing-items"></a><span data-ttu-id="e5303-143">Čtení a zápis položky</span><span class="sxs-lookup"><span data-stu-id="e5303-143">Reading and writing items</span></span>
<span data-ttu-id="e5303-144">Nyní Pojďme vkládat data do databáze Cosmos.</span><span class="sxs-lookup"><span data-stu-id="e5303-144">Now, let's insert data into Cosmos DB.</span></span> <span data-ttu-id="e5303-145">Zde je ukázka třída obsahující čtení zařízení a volání tooCreateDocumentAsync tooinsert nové zařízení čtení do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e5303-145">Here's a sample class containing a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a container.</span></span> <span data-ttu-id="e5303-146">Jedná se o příklad využití hello DocumentDB rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e5303-146">This is an example leveraging hello DocumentDB API:</span></span>

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

<span data-ttu-id="e5303-147">Umožňuje přečíst položku hello svým id a klíč oddílu, aktualizovat a jako poslední krok, odstraňte ji. klíč oddílu a id. Všimněte si, že čtení hello obsahují hodnotu PartitionKey (odpovídající toohello `x-ms-documentdb-partitionkey` hlavička požadavku v hello REST API).</span><span class="sxs-lookup"><span data-stu-id="e5303-147">Let's read hello item by its partition key and id, update it, and then as a final step, delete it by partition key and id. Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a><span data-ttu-id="e5303-148">Dotazování oddílů kontejnery</span><span class="sxs-lookup"><span data-stu-id="e5303-148">Querying partitioned containers</span></span>
<span data-ttu-id="e5303-149">Když dotazujete data do oddílů kontejnerů Cosmos DB automaticky trasy hello oddíly query toohello odpovídající hodnoty klíče toohello oddílu zadané ve filtru hello (pokud existují).</span><span class="sxs-lookup"><span data-stu-id="e5303-149">When you query data in partitioned containers, Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="e5303-150">Tento dotaz je například směrované toojust hello oddílu obsahující hello klíč oddílu "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="e5303-150">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="e5303-151">Hello následující dotaz na klíč oddílu hello (DeviceId) nemá filtr a je fanned na oddíly tooall němž se spustí před hello oddílu indexu.</span><span class="sxs-lookup"><span data-stu-id="e5303-151">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="e5303-152">Všimněte si, že máte toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v hello REST API) toohave hello SDK tooexecute dotazu napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="e5303-152">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

<span data-ttu-id="e5303-153">Podporuje cosmos DB [agregační funkce](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` a `AVG` přes oddíly kontejnery pomocí SQL spouštění pomocí sady SDK 1.12.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="e5303-153">Cosmos DB supports [aggregate functions](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` and `AVG` over partitioned containers using SQL starting with SDKs 1.12.0 and above.</span></span> <span data-ttu-id="e5303-154">Dotazy musí obsahovat jeden agregační operátor a musí obsahovat jednu hodnotu v projekci hello.</span><span class="sxs-lookup"><span data-stu-id="e5303-154">Queries must include a single aggregate operator, and must include a single value in hello projection.</span></span>

### <a name="parallel-query-execution"></a><span data-ttu-id="e5303-155">Provádění paralelního dotazu</span><span class="sxs-lookup"><span data-stu-id="e5303-155">Parallel query execution</span></span>
<span data-ttu-id="e5303-156">Hello Cosmos DB SDK 1.9.0 a výše podpory možnosti provádění paralelního dotazu, které umožňují s nízkou latencí tooperform dotazy proti dělené kolekce, i v případě, že potřebují tootouch velký počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="e5303-156">hello Cosmos DB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="e5303-157">Následující dotaz hello je například nakonfigurované toorun paralelně napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="e5303-157">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="e5303-158">Provádění paralelního dotazu můžete spravovat pomocí ladění hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e5303-158">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="e5303-159">Nastavením `MaxDegreeOfParallelism`, můžete řídit hello stupně paralelního zpracování tedy hello maximální počet souběžných síťové připojení toohello kontejneru oddílů.</span><span class="sxs-lookup"><span data-stu-id="e5303-159">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello container's partitions.</span></span> <span data-ttu-id="e5303-160">Pokud nastavíte příliš-1, hello stupně paralelního zpracování spravuje hello SDK.</span><span class="sxs-lookup"><span data-stu-id="e5303-160">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="e5303-161">Pokud hello `MaxDegreeOfParallelism` není zadaný, nebo nastavte too0, což je výchozí hodnota hello, bude jednoho síťového připojení toohello kontejneru na oddíly.</span><span class="sxs-lookup"><span data-stu-id="e5303-161">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello container's partitions.</span></span>
* <span data-ttu-id="e5303-162">Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e5303-162">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="e5303-163">Pokud tento parametr vynecháte, nebo nastavte příliš-1, hello počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje hello SDK.</span><span class="sxs-lookup"><span data-stu-id="e5303-163">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="e5303-164">Zadané hello stejného stavu hello kolekce paralelní dotaz vrátí výsledky v hello stejné pořadí jako sériové provádění.</span><span class="sxs-lookup"><span data-stu-id="e5303-164">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="e5303-165">Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), problémy Azure Cosmos DB SDK hello hello napříč oddíly a sloučí částečně seřazená výsledky v hello klientské straně tooproduce globálně řazení výsledků dotazu paralelně.</span><span class="sxs-lookup"><span data-stu-id="e5303-165">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello Azure Cosmos DB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

### <a name="executing-stored-procedures"></a><span data-ttu-id="e5303-166">Provádění uložené procedury</span><span class="sxs-lookup"><span data-stu-id="e5303-166">Executing stored procedures</span></span>
<span data-ttu-id="e5303-167">Můžete také provést jednotlivé transakce na dokumenty s hello stejné ID zařízení, například pokud jste zachování agregace nebo hello nejnovější stav zařízení v jedné položce.</span><span class="sxs-lookup"><span data-stu-id="e5303-167">You can also execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single item.</span></span> 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
<span data-ttu-id="e5303-168">V další části hello podíváme na tom, jak můžete přesunout toopartitioned kontejnery z kontejnerů jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="e5303-168">In hello next section, we look at how you can move toopartitioned containers from single-partition containers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5303-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5303-169">Next steps</span></span>
<span data-ttu-id="e5303-170">V tomto článku jsme poskytuje přehled o tom, jak toowork s oddíly Azure Cosmos DB kontejnery s hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5303-170">In this article, we provided an overview of how toowork with partitioning of Azure Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="e5303-171">Viz také [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5303-171">Also see [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="e5303-172">Proveďte škálování a výkon testování pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5303-172">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="e5303-173">V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.</span><span class="sxs-lookup"><span data-stu-id="e5303-173">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="e5303-174">Začínáme s hello kódování [sady SDK](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/)</span><span class="sxs-lookup"><span data-stu-id="e5303-174">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/)</span></span>
* <span data-ttu-id="e5303-175">Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="e5303-175">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>

