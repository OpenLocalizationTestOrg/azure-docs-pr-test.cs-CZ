---
title: "Vytváření oddílů a škálování v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak rozdělení funguje v Azure Cosmos DB, jak nakonfigurovat, vytváření oddílů a oddílu klíče a jak vybrat klíč správné oddílu pro vaši aplikaci."
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
ms.openlocfilehash: 81010d91ac7fe8fa7149c52ed56af304cf4e83d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-the-documentdb-api"></a><span data-ttu-id="05f27-103">Vytváření oddílů v Azure DB Cosmos pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="05f27-103">Partitioning in Azure Cosmos DB using the DocumentDB API</span></span>

<span data-ttu-id="05f27-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) je globální databáze distribuované, více modelu služba navržená tak, aby vám pomohou dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem.</span><span class="sxs-lookup"><span data-stu-id="05f27-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is a global distributed, multi-model database service designed to help you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> 

<span data-ttu-id="05f27-105">Tento článek obsahuje přehled o tom, jak pracovat s oddíly Cosmos DB kontejnery s rozhraním API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="05f27-105">This article provides an overview of how to work with partitioning of Cosmos DB containers with the DocumentDB API.</span></span> <span data-ttu-id="05f27-106">V tématu [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="05f27-106">See [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

<span data-ttu-id="05f27-107">Pokud chcete začít s kódem, stáhněte si projekt z [Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="05f27-107">To get started with code, download the project from [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<span data-ttu-id="05f27-108">Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="05f27-108">After reading this article, you will be able to answer the following questions:</span></span>   

* <span data-ttu-id="05f27-109">Jak funguje vytváření oddílů v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="05f27-109">How does partitioning work in Azure Cosmos DB?</span></span>
* <span data-ttu-id="05f27-110">Jak nakonfigurovat, vytváření oddílů v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="05f27-110">How do I configure partitioning in Azure Cosmos DB</span></span>
* <span data-ttu-id="05f27-111">Jaké jsou klíče oddílů a jak vyberte klíč oddílu správné své aplikaci?</span><span class="sxs-lookup"><span data-stu-id="05f27-111">What are partition keys, and how do I pick the right partition key for my application?</span></span>

<span data-ttu-id="05f27-112">Pokud chcete začít s kódem, stáhněte si projekt z [Azure Cosmos DB výkonu testování ovladačů Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="05f27-112">To get started with code, download the project from [Azure Cosmos DB Performance Testing Driver Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a><span data-ttu-id="05f27-113">Klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="05f27-113">Partition keys</span></span>

<span data-ttu-id="05f27-114">V rozhraní API služby DocumentDB zadejte definici klíče oddílu ve formě cesta JSON.</span><span class="sxs-lookup"><span data-stu-id="05f27-114">In the DocumentDB API, you specify the partition key definition in the form of a JSON path.</span></span> <span data-ttu-id="05f27-115">V následující tabulce jsou uvedeny příklady oddílu klíčové definice a hodnoty odpovídající každé.</span><span class="sxs-lookup"><span data-stu-id="05f27-115">The following table shows examples of partition key definitions and the values corresponding to each.</span></span> <span data-ttu-id="05f27-116">Klíč oddílu je zadán jako cesta, například `/department` představuje vlastnost oddělení.</span><span class="sxs-lookup"><span data-stu-id="05f27-116">The partition key is specified as a path, e.g. `/department` represents the property department.</span></span> 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="05f27-117"><strong>Klíč oddílu</strong></span><span class="sxs-lookup"><span data-stu-id="05f27-117"><strong>Partition Key</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="05f27-118"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="05f27-118"><strong>Description</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="05f27-119">/Department</span><span class="sxs-lookup"><span data-stu-id="05f27-119">/department</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="05f27-120">Odpovídá hodnotě doc.department, kde je dokumentace položky.</span><span class="sxs-lookup"><span data-stu-id="05f27-120">Corresponds to the value of doc.department where doc is the item.</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="05f27-121">/ vlastnosti/název</span><span class="sxs-lookup"><span data-stu-id="05f27-121">/properties/name</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="05f27-122">Odpovídá hodnotě doc.properties.name, kde je dokumentace položky (vnořené vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="05f27-122">Corresponds to the value of doc.properties.name where doc is the item (nested property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="05f27-123">/ID</span><span class="sxs-lookup"><span data-stu-id="05f27-123">/id</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="05f27-124">Odpovídá hodnotě doc.id (id a oddílu klíče jsou stejnou vlastnost).</span><span class="sxs-lookup"><span data-stu-id="05f27-124">Corresponds to the value of doc.id (id and partition key are the same property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="05f27-125">/ ""oddělení název</span><span class="sxs-lookup"><span data-stu-id="05f27-125">/"department name"</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="05f27-126">Odpovídá hodnotě doc ["název oddělení"], kde je dokumentace položky.</span><span class="sxs-lookup"><span data-stu-id="05f27-126">Corresponds to the value of doc["department name"] where doc is the item.</span></span></p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> <span data-ttu-id="05f27-127">Syntaxe pro klíč oddílu je podobná specifikace cesty pro indexování zásad cesty s klíčovým rozdílem cestu odpovídající vlastnost namísto hodnoty, tj. žádné zástupný znak není na konci.</span><span class="sxs-lookup"><span data-stu-id="05f27-127">The syntax for partition key is similar to the path specification for indexing policy paths with the key difference that the path corresponds to the property instead of the value, i.e. there is no wild card at the end.</span></span> <span data-ttu-id="05f27-128">Například zadali byste/oddělení /?</span><span class="sxs-lookup"><span data-stu-id="05f27-128">For example, you would specify /department/?</span></span> <span data-ttu-id="05f27-129">Chcete-li index hodnoty v rámci oddělení, ale zadejte /department jako definice klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="05f27-129">to index the values under department, but specify /department as the partition key definition.</span></span> <span data-ttu-id="05f27-130">Klíč oddílu je implicitně indexovaný a nelze vyloučit z indexování pomocí přepsání indexování zásad.</span><span class="sxs-lookup"><span data-stu-id="05f27-130">The partition key is implicitly indexed and cannot be excluded from indexing using indexing policy overrides.</span></span>
> 
> 

<span data-ttu-id="05f27-131">Podíváme, jak volba klíč oddílu má dopad na výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="05f27-131">Let's look at how the choice of partition key impacts the performance of your application.</span></span>

## <a name="working-with-the-azure-cosmos-db-sdks"></a><span data-ttu-id="05f27-132">Práce s Azure Cosmos DB sady SDK</span><span class="sxs-lookup"><span data-stu-id="05f27-132">Working with the Azure Cosmos DB SDKs</span></span>
<span data-ttu-id="05f27-133">Azure Cosmos DB přidala se podpora pro automatické vytváření oddílů s [REST API verze 2015-12-16](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="05f27-133">Azure Cosmos DB added support for automatic partitioning with [REST API version 2015-12-16](/rest/api/documentdb/).</span></span> <span data-ttu-id="05f27-134">Chcete-li vytvořit oddílů kontejnery, je nutné stáhnout verze sady SDK 1.6.0 nebo novějším v jednom z podporovaných platforem SDK (.NET, Node.js, Java, Python, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="05f27-134">In order to create partitioned containers, you must download SDK versions 1.6.0 or newer in one of the supported SDK platforms (.NET, Node.js, Java, Python, MongoDB).</span></span> 

### <a name="creating-containers"></a><span data-ttu-id="05f27-135">Vytvoření kontejnerů</span><span class="sxs-lookup"><span data-stu-id="05f27-135">Creating containers</span></span>
<span data-ttu-id="05f27-136">Následující příklad ukazuje fragment .NET vytvořit kontejner pro uložení zařízení telemetrická data z 20 000 jednotek žádosti za sekundu, propustnosti.</span><span class="sxs-lookup"><span data-stu-id="05f27-136">The following sample shows a .NET snippet to create a container to store device telemetry data of 20,000 request units per second of throughput.</span></span> <span data-ttu-id="05f27-137">Sada SDK nastaví hodnotu OfferThroughput (který naopak nastaví `x-ms-offer-throughput` hlavička požadavku v rozhraní REST API).</span><span class="sxs-lookup"><span data-stu-id="05f27-137">The SDK sets the OfferThroughput value (which in turn sets the `x-ms-offer-throughput` request header in the REST API).</span></span> <span data-ttu-id="05f27-138">Zde jsme nastavit `/deviceId` jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="05f27-138">Here we set the `/deviceId` as the partition key.</span></span> <span data-ttu-id="05f27-139">Volba klíč oddílu se uloží spolu s ostatními metadata kontejneru jako název a zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="05f27-139">The choice of partition key is saved along with the rest of the container metadata like name and indexing policy.</span></span>

<span data-ttu-id="05f27-140">Tato ukázka jsme zachyceny `deviceId` vzhledem k tomu, že nám vědět, že (a) od existuje velký počet zařízení, zápisy mohou být distribuovány na oddíly rovnoměrně a abychom mohli škálovat databázi k ingestování ohromné objemy dat a (b) mnoho požadavků, jako načítání nejnovější čtení pro zařízení jsou omezená na jednom ID zařízení a mohou být načteny z jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="05f27-140">For this sample, we picked `deviceId` since we know that (a) since there are a large number of devices, writes can be distributed across partitions evenly and allowing us to scale the database to ingest massive volumes of data and (b) many of the requests like fetching the latest reading for a device are scoped to a single deviceId and can be retrieved from a single partition.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
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

<span data-ttu-id="05f27-141">Tato metoda umožňuje volání do databáze Cosmos rozhraní REST API a zřizovat počet oddílů podle propustnosti, požadované služby.</span><span class="sxs-lookup"><span data-stu-id="05f27-141">This method makes a REST API call to Cosmos DB, and the service will provision a number of partitions based on the requested throughput.</span></span> <span data-ttu-id="05f27-142">Výkon vašich potřeb momentální, můžete změnit propustnost kontejner.</span><span class="sxs-lookup"><span data-stu-id="05f27-142">You can change the throughput of a container as your performance needs evolve.</span></span> 

### <a name="reading-and-writing-items"></a><span data-ttu-id="05f27-143">Čtení a zápis položky</span><span class="sxs-lookup"><span data-stu-id="05f27-143">Reading and writing items</span></span>
<span data-ttu-id="05f27-144">Nyní Pojďme vkládat data do databáze Cosmos.</span><span class="sxs-lookup"><span data-stu-id="05f27-144">Now, let's insert data into Cosmos DB.</span></span> <span data-ttu-id="05f27-145">Zde je ukázka třída obsahující zařízení, čtení a volání CreateDocumentAsync vložit nového zařízení čtení do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="05f27-145">Here's a sample class containing a device reading, and a call to CreateDocumentAsync to insert a new device reading into a container.</span></span> <span data-ttu-id="05f27-146">Jedná se o příklad využití rozhraní API DocumentDB:</span><span class="sxs-lookup"><span data-stu-id="05f27-146">This is an example leveraging the DocumentDB API:</span></span>

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

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
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

<span data-ttu-id="05f27-147">Umožňuje přečíst položku svým id a klíč oddílu, aktualizovat a jako poslední krok, odstraňte ji. klíč oddílu a id. Všimněte si, že čtení obsahují hodnotu PartitionKey (odpovídající `x-ms-documentdb-partitionkey` hlavička požadavku v rozhraní REST API).</span><span class="sxs-lookup"><span data-stu-id="05f27-147">Let's read the item by its partition key and id, update it, and then as a final step, delete it by partition key and id. Note that the reads include a PartitionKey value (corresponding to the `x-ms-documentdb-partitionkey` request header in the REST API).</span></span>

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
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

### <a name="querying-partitioned-containers"></a><span data-ttu-id="05f27-148">Dotazování oddílů kontejnery</span><span class="sxs-lookup"><span data-stu-id="05f27-148">Querying partitioned containers</span></span>
<span data-ttu-id="05f27-149">Pro dotazování dat v kontejnerech oddílů, Cosmos DB automaticky směruje dotaz na oddíly odpovídající hodnoty klíče oddílu zadaných ve filtru (pokud existují).</span><span class="sxs-lookup"><span data-stu-id="05f27-149">When you query data in partitioned containers, Cosmos DB automatically routes the query to the partitions corresponding to the partition key values specified in the filter (if there are any).</span></span> <span data-ttu-id="05f27-150">Například tento dotaz se směruje na právě oddílu klíč oddílu "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="05f27-150">For example, this query is routed to just the partition containing the partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="05f27-151">Následující dotaz na klíč oddílu (DeviceId) nemá filtr a je fanned pro všechny oddíly, kde je u indexu oddílu spustit.</span><span class="sxs-lookup"><span data-stu-id="05f27-151">The following query does not have a filter on the partition key (DeviceId) and is fanned out to all partitions where it is executed against the partition's index.</span></span> <span data-ttu-id="05f27-152">Všimněte si, že budete muset určit EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v rozhraní REST API) tak, aby měl sady SDK při spuštění dotazu napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="05f27-152">Note that you have to specify the EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in the REST API) to have the SDK to execute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

<span data-ttu-id="05f27-153">Podporuje cosmos DB [agregační funkce](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` a `AVG` přes oddíly kontejnery pomocí SQL spouštění pomocí sady SDK 1.12.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="05f27-153">Cosmos DB supports [aggregate functions](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` and `AVG` over partitioned containers using SQL starting with SDKs 1.12.0 and above.</span></span> <span data-ttu-id="05f27-154">Dotazy musí obsahovat jeden agregační operátor a musí obsahovat jednu hodnotu v projekci.</span><span class="sxs-lookup"><span data-stu-id="05f27-154">Queries must include a single aggregate operator, and must include a single value in the projection.</span></span>

### <a name="parallel-query-execution"></a><span data-ttu-id="05f27-155">Provádění paralelního dotazu</span><span class="sxs-lookup"><span data-stu-id="05f27-155">Parallel query execution</span></span>
<span data-ttu-id="05f27-156">Sady SDK DB Cosmos 1.9.0 a vyšší možnosti provádění paralelního dotazu podpory, které umožňují provádět dotazy s nízkou latencí pro dělené kolekce, i v případě, že potřebují k touch velký počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="05f27-156">The Cosmos DB SDKs 1.9.0 and above support parallel query execution options, which allow you to perform low latency queries against partitioned collections, even when they need to touch a large number of partitions.</span></span> <span data-ttu-id="05f27-157">Například následující dotaz je nakonfigurována pro spuštění paralelně napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="05f27-157">For example, the following query is configured to run in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="05f27-158">Provádění paralelního dotazu můžete spravovat pomocí ladění následující parametry:</span><span class="sxs-lookup"><span data-stu-id="05f27-158">You can manage parallel query execution by tuning the following parameters:</span></span>

* <span data-ttu-id="05f27-159">Nastavením `MaxDegreeOfParallelism`, můžete řídit stupně paralelního zpracování tedy maximální počet souběžných síťová připojení k oddílům kontejneru.</span><span class="sxs-lookup"><span data-stu-id="05f27-159">By setting `MaxDegreeOfParallelism`, you can control the degree of parallelism i.e., the maximum number of simultaneous network connections to the container's partitions.</span></span> <span data-ttu-id="05f27-160">Pokud nastavíte na hodnotu -1, stupně paralelního zpracování spravuje sady SDK.</span><span class="sxs-lookup"><span data-stu-id="05f27-160">If you set this to -1, the degree of parallelism is managed by the SDK.</span></span> <span data-ttu-id="05f27-161">Pokud `MaxDegreeOfParallelism` není zadaný, nebo je nastavený na 0, což je výchozí hodnota, bude jedno síťové připojení k oddílům kontejneru.</span><span class="sxs-lookup"><span data-stu-id="05f27-161">If the `MaxDegreeOfParallelism` is not specified or set to 0, which is the default value, there will be a single network connection to the container's partitions.</span></span>
* <span data-ttu-id="05f27-162">Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="05f27-162">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="05f27-163">Pokud tento parametr vynecháte nebo tuto možnost nastavíte na hodnotu -1, počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje sady SDK.</span><span class="sxs-lookup"><span data-stu-id="05f27-163">If you omit this parameter or set this to -1, the number of items buffered during parallel query execution is managed by the SDK.</span></span>

<span data-ttu-id="05f27-164">Zadaný stav stejné kolekce, paralelní dotaz vrátí výsledky ve stejném pořadí jako sériové provádění.</span><span class="sxs-lookup"><span data-stu-id="05f27-164">Given the same state of the collection, a parallel query will return results in the same order as in serial execution.</span></span> <span data-ttu-id="05f27-165">Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), sadu SDK Azure Cosmos DB vydá dotaz paralelně napříč oddíly a sloučí částečně seřazená výsledky na straně klienta k vytvoření globální seřazené výsledky.</span><span class="sxs-lookup"><span data-stu-id="05f27-165">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), the Azure Cosmos DB SDK issues the query in parallel across partitions and merges partially sorted results in the client side to produce globally ordered results.</span></span>

### <a name="executing-stored-procedures"></a><span data-ttu-id="05f27-166">Provádění uložené procedury</span><span class="sxs-lookup"><span data-stu-id="05f27-166">Executing stored procedures</span></span>
<span data-ttu-id="05f27-167">Můžete také provést jednotlivé transakce na dokumenty se stejným ID zařízení, například pokud jste zachování agregace nebo nejnovější stav zařízení v jedné položce.</span><span class="sxs-lookup"><span data-stu-id="05f27-167">You can also execute atomic transactions against documents with the same device ID, e.g. if you're maintaining aggregates or the latest state of a device in a single item.</span></span> 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
<span data-ttu-id="05f27-168">V další části podíváme na tom, jak můžete přesunout do oddílů kontejnerů z kontejnerů jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="05f27-168">In the next section, we look at how you can move to partitioned containers from single-partition containers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05f27-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05f27-169">Next steps</span></span>
<span data-ttu-id="05f27-170">V tomto článku jsme poskytuje přehled o tom, jak pracovat s oddíly kontejnery Azure Cosmos DB s rozhraním API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="05f27-170">In this article, we provided an overview of how to work with partitioning of Azure Cosmos DB containers with the DocumentDB API.</span></span> <span data-ttu-id="05f27-171">Viz také [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="05f27-171">Also see [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="05f27-172">Proveďte škálování a výkon testování pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="05f27-172">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="05f27-173">V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.</span><span class="sxs-lookup"><span data-stu-id="05f27-173">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="05f27-174">Začínáme s kódování [sady SDK](documentdb-sdk-dotnet.md) nebo [REST API](/rest/api/documentdb/)</span><span class="sxs-lookup"><span data-stu-id="05f27-174">Get started coding with the [SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/)</span></span>
* <span data-ttu-id="05f27-175">Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="05f27-175">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>

