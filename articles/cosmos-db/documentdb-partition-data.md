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
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a>Vytváření oddílů v Azure DB Cosmos pomocí hello DocumentDB rozhraní API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) je toohelp služba navržená tak, globální distribuované, více modelu databáze můžete dosáhnout rychlé, předvídatelný výkon a škálování bezproblémově společně s vaší aplikací je s růstem. 

Tento článek obsahuje přehled o tom, jak toowork s oddíly Cosmos DB kontejnery s hello DocumentDB rozhraní API. V tématu [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB. 

tooget spuštění s kódem, stáhněte si projekt hello z [Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:   

* Jak funguje vytváření oddílů v Azure Cosmos DB?
* Jak nakonfigurovat, vytváření oddílů v Azure Cosmos DB
* Jaké jsou klíče oddílů a jak vyberte klíč oddílu správné hello své aplikaci?

tooget spuštění s kódem, stáhněte si projekt hello z [Azure Cosmos DB výkonu testování ovladačů Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Klíče oddílů

V hello rozhraní API DocumentDB zadejte definici hello klíče oddílu v hello formu cesty JSON. Hello následující tabulka uvádí příklady oddílu klíčové definice a hello hodnoty odpovídající tooeach. klíč oddílu Hello je zadán jako cesta, například `/department` představuje hello vlastnost oddělení. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Klíč oddílu</strong></p></td>
            <td valign="top"><p><strong>Popis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Odpovídá toohello hodnotu doc.department, kde je dokumentace hello položky.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ vlastnosti/název</p></td>
            <td valign="top"><p>Odpovídá toohello hodnotu doc.properties.name, kde je dokumentace hello položky (vnořené vlastnosti).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Odpovídá toohello hodnotu doc.id (id a oddílu klíče jsou hello stejné vlastnosti).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ""oddělení název</p></td>
            <td valign="top"><p>Odpovídá toohello hodnotu doc ["název oddělení"], kde je dokumentace hello položky.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> Hello syntaxe pro klíč oddílu je podobné specifikace toohello cesty pro indexování zásad cesty s hello klíče rozdílem, že hello cesta odpovídá vlastnosti toohello namísto hodnoty hello, tj. žádné zástupný znak není na konci hello. Například zadali byste/oddělení /? tooindex hello hodnoty podle oddělení, ale zadat /department jako hello definici klíče oddílu. klíč oddílu Hello je implicitně indexovaný a nelze vyloučit z indexování pomocí přepsání indexování zásad.
> 
> 

Podíváme, jak volba hello klíč oddílu ovlivňuje hello výkon aplikace.

## <a name="working-with-hello-azure-cosmos-db-sdks"></a>Práce s hello SDK služby Azure Cosmos DB
Azure Cosmos DB přidala se podpora pro automatické vytváření oddílů s [REST API verze 2015-12-16](/rest/api/documentdb/). V kontejnerech pořadí toocreate rozdělena na oddíly, je nutné stáhnout sadu SDK verze 1.6.0 nebo novější v jednom z hello podporována SDK platformy (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Vytvoření kontejnerů
Hello následující příklad ukazuje toocreate fragment kódu rozhraní .NET kontejner toostore zařízení telemetrická data z 20 000 jednotek žádosti za sekundu, propustnosti. Hello SDK nastaví hodnotu OfferThroughput hello (který naopak nastaví hello `x-ms-offer-throughput` hlavička požadavku v hello REST API). Zde jsme nastavit hello `/deviceId` jako klíč oddílu hello. Volba Hello klíč oddílu se uloží spolu s hello zbytek hello metadata kontejneru jako název a zásady indexování.

Tato ukázka jsme zachyceny `deviceId` vzhledem k tomu, že nám vědět, že (a) od existuje velký počet zařízení, zápisy mohou být distribuovány na oddíly rovnoměrně a umožňuje nám tooscale hello databáze tooingest ohromné objemy dat a (b) mnoho požadavků hello jako načítání hello nejnovější čtení pro zařízení jsou deviceId jednoho oboru tooa a je možné načíst z jednoho oddílu.

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

Tato metoda umožňuje tooCosmos DB volání rozhraní REST API a služby hello zřizovat počet oddílů založené na požadovaný propustnost hello. Výkon vašich potřeb momentální, můžete změnit hello propustnost kontejner. 

### <a name="reading-and-writing-items"></a>Čtení a zápis položky
Nyní Pojďme vkládat data do databáze Cosmos. Zde je ukázka třída obsahující čtení zařízení a volání tooCreateDocumentAsync tooinsert nové zařízení čtení do kontejneru. Jedná se o příklad využití hello DocumentDB rozhraní API:

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

Umožňuje přečíst položku hello svým id a klíč oddílu, aktualizovat a jako poslední krok, odstraňte ji. klíč oddílu a id. Všimněte si, že čtení hello obsahují hodnotu PartitionKey (odpovídající toohello `x-ms-documentdb-partitionkey` hlavička požadavku v hello REST API).

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

### <a name="querying-partitioned-containers"></a>Dotazování oddílů kontejnery
Když dotazujete data do oddílů kontejnerů Cosmos DB automaticky trasy hello oddíly query toohello odpovídající hodnoty klíče toohello oddílu zadané ve filtru hello (pokud existují). Tento dotaz je například směrované toojust hello oddílu obsahující hello klíč oddílu "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Hello následující dotaz na klíč oddílu hello (DeviceId) nemá filtr a je fanned na oddíly tooall němž se spustí před hello oddílu indexu. Všimněte si, že máte toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v hello REST API) toohave hello SDK tooexecute dotazu napříč oddíly.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Podporuje cosmos DB [agregační funkce](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` a `AVG` přes oddíly kontejnery pomocí SQL spouštění pomocí sady SDK 1.12.0 a vyšší. Dotazy musí obsahovat jeden agregační operátor a musí obsahovat jednu hodnotu v projekci hello.

### <a name="parallel-query-execution"></a>Provádění paralelního dotazu
Hello Cosmos DB SDK 1.9.0 a výše podpory možnosti provádění paralelního dotazu, které umožňují s nízkou latencí tooperform dotazy proti dělené kolekce, i v případě, že potřebují tootouch velký počet oddílů. Následující dotaz hello je například nakonfigurované toorun paralelně napříč oddíly.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Provádění paralelního dotazu můžete spravovat pomocí ladění hello následující parametry:

* Nastavením `MaxDegreeOfParallelism`, můžete řídit hello stupně paralelního zpracování tedy hello maximální počet souběžných síťové připojení toohello kontejneru oddílů. Pokud nastavíte příliš-1, hello stupně paralelního zpracování spravuje hello SDK. Pokud hello `MaxDegreeOfParallelism` není zadaný, nebo nastavte too0, což je výchozí hodnota hello, bude jednoho síťového připojení toohello kontejneru na oddíly.
* Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta. Pokud tento parametr vynecháte, nebo nastavte příliš-1, hello počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje hello SDK.

Zadané hello stejného stavu hello kolekce paralelní dotaz vrátí výsledky v hello stejné pořadí jako sériové provádění. Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), problémy Azure Cosmos DB SDK hello hello napříč oddíly a sloučí částečně seřazená výsledky v hello klientské straně tooproduce globálně řazení výsledků dotazu paralelně.

### <a name="executing-stored-procedures"></a>Provádění uložené procedury
Můžete také provést jednotlivé transakce na dokumenty s hello stejné ID zařízení, například pokud jste zachování agregace nebo hello nejnovější stav zařízení v jedné položce. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
V další části hello podíváme na tom, jak můžete přesunout toopartitioned kontejnery z kontejnerů jedním oddílem.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme poskytuje přehled o tom, jak toowork s oddíly Azure Cosmos DB kontejnery s hello DocumentDB rozhraní API. Viz také [vytváření oddílů a horizontální škálování](../cosmos-db/partition-data.md) přehled o konceptech a osvědčené postupy pro vytváření oddílů s jakéhokoli rozhraní API Azure Cosmos DB. 

* Proveďte škálování a výkon testování pomocí Azure Cosmos DB. V tématu [testování výkonu a škálování s Azure Cosmos DB](performance-testing.md) pro ukázku.
* Začínáme s hello kódování [sady SDK](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/)
* Další informace o [zřízené propustnosti v Azure Cosmos DB](request-units.md)

