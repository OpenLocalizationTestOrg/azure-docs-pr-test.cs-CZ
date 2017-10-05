---
title: "Práce se změnami kanálu podpory v Azure Cosmos DB | Microsoft Docs"
description: "Použijte Azure Cosmos DB změnu informačního kanálu podporu sledování změn v dokumentech a provádět na základě událostí zpracování jako aktivační události a průběžná aktualizace mezipaměti a analýzy systémy."
keywords: "Změna kanálu"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 160fbc98e0f3dcc7d17cbe0c7f7425811596a896
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Práce se změnami kanálu podpory v Azure Cosmos DB
[Azure Cosmos DB](../cosmos-db/introduction.md) je rychlého a flexibilní globálně replikované databáze služby, který slouží k ukládání velkých objemů dat transakcí a funkční s latencí předvídatelný jednociferné milisekund pro čtení a zápisu. Díky tomu dobře hodí pro IoT, hry, maloobchodní a provozní protokolování aplikace. Sledovat změny provedené v Azure Cosmos DB dat a aktualizovat materializovaným zobrazením, provádět analýzu v reálném čase, archivaci dat na studené úložiště a aktivovat oznámení na určité události na základě těchto změn je běžný vzor návrhu v těchto aplikacích. **Změnu kanálu podporu** v Azure Cosmos DB umožňuje vytvářet efektivní a škálovatelné řešení pro každou z těchto vzorků.

Změny kanálu podpory poskytuje Azure Cosmos DB seřazený seznam dokumenty v kolekci Azure Cosmos DB v pořadí, ve kterém byly upraveny. Tento informační kanál lze použít k naslouchání změny dat v rámci kolekce a provádět akce, jako:

* Aktivovat volání rozhraní API, kdy je dokument vložit nebo úpravě
* Na aktualizace provést zpracování v reálném čase (proud)
* Synchronizaci dat s mezipaměti, vyhledávací web nebo datového skladu

Změny v Azure Cosmos DB jsou nastavené jako trvalé může být zpracována asynchronně a distribuovaná do jednoho nebo více spotřebitelů pro paralelní zpracování. Podívejme se na rozhraní API pro změnu kanálu a jak je můžete použít k vytváření škálovatelné aplikace v reálném čase. Tento článek ukazuje, jak pracovat s Azure Cosmos DB změn kanálu a rozhraní API DocumentDB. 

![Pomocí Azure Cosmos DB změnu kanálu power analýzu v reálném čase a událostmi řízené výpočetní scénáře](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Změna kanálu podpora je k dispozici pouze pro rozhraní API DocumentDB v tuto chvíli; rozhraní Graph API a rozhraní API tabulky nejsou aktuálně podporovány.

## <a name="use-cases-and-scenarios"></a>Případy použití a scénáře
Změna kanálu umožňuje efektivní zpracování rozsáhlých datových sad k velkému počtu zápisy a nabízí alternativu k dotazování celé datové sady pro identifikaci, co se změnilo. Například můžete provádět následující úlohy efektivně:

* Aktualizace mezipaměti, index vyhledávání nebo datového skladu s daty uloženými v databázi Azure Cosmos.
* Aplikační úrovni využití dat vrstvení a archivaci, tedy ukládání "horkých dat." v Azure Cosmos DB a po určité době odstraněny "pomaleji přístupná data" k [Azure Blob Storage](../storage/common/storage-introduction.md) nebo [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementace batch analytics na data pomocí [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Implementace [lambda kanály v Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) s Azure Cosmos DB. Azure Cosmos DB poskytuje řešení škálovatelná databáze, které může zpracovat přijímání a dotazů a implementovat lambda architektury s nízkou celkové náklady na vlastnictví. 
* Provést nulové době migrací na jiný účet Azure Cosmos DB jiné schéma rozdělení oddílů.

**Lambda kanálů s Azure DB Cosmos pro přijímání a dotazů:**

![Azure Cosmos DB na základě lambda kanálu pro přijímání a dotazů](./media/change-feed/lambda.png)

Můžete použít Azure Cosmos DB přijmout a ukládání dat události ze zařízení, senzorů, infrastruktury a aplikace a zpracování těchto událostí v reálném čase pomocí [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), nebo [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

V rámci vaší [bez serveru](http://azure.com/serverless) webových a mobilních aplikací, můžete sledovat události, jako jsou například změny do vašeho zákazníka profilu, předvolby nebo umístění pro spuštění určitých akcí, jako je odesílání nabízených oznámení do jejich zařízení pomocí [ Azure Functions](../azure-functions/functions-bindings-documentdb.md) nebo [aplikační služby](https://azure.microsoft.com/services/app-service/). Pokud používáte databázi Cosmos Azure k vytvoření hry, můžete, například použití změnit informačního kanálu v reálném čase tabulky podle skóre z dokončené hry implementovat.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Jak funguje změnu informační kanál v Azure Cosmos DB
Azure Cosmos DB poskytuje možnost číst přírůstkové aktualizace provedené na kolekci Azure Cosmos DB. Tento informační kanál změnu má následující vlastnosti:

* Změny jsou trvalé v Azure Cosmos DB a může být zpracována asynchronně.
* Jsou k dispozici okamžitě změnu kanálu změny do dokumentů v rámci kolekce.
* Každé změně do dokumentu se zobrazí právě jednou v změnu kanálu a klienti spravovat své logiku vytváření kontrolních bodů. Knihovna informačního kanálu procesoru změn poskytuje automatické vytváření kontrolních bodů a "alespoň jednou" sémantiku.
* Protokol změn je součástí pouze poslední změny u daného dokumentu. Přechodných změn nemusí být k dispozici.
* Změna informačního kanálu je řazen u úpravy v rámci každé hodnotu klíče oddílu. Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.
* Změny mohou být synchronizovány z jakékoli bodu v čase, tedy neexistuje žádné období uchovávání pevné dat, pro které jsou k dispozici změny.
* Změny jsou k dispozici v bloky rozsahy klíčů oddílů. Tato možnost umožňuje změny z rozsáhlých kolekcí, které mají být zpracovány současně více příjemci nebo servery.
* Aplikace může požadovat pro více informačních kanálů změnu současně na stejné kolekci.

Informační kanál Azure Cosmos DB změnu je povoleno ve výchozím nastavení pro všechny účty. Můžete použít vaše [zřízené propustnosti](request-units.md) ve vaší oblasti zápisu ani žádné [číst oblast](distribute-data-globally.md) číst z změnu kanálu, stejně jako všechny ostatní operace z Azure Cosmos DB. Informační kanál změnu zahrnuje vložení a operace aktualizace provedené na dokumenty v rámci kolekce. Můžete zaznamenat odstranění nastavením příznak "soft odstranění" v rámci dokumentů místo odstranění. Alternativně můžete nastavit konečný platnosti pro dokumentů prostřednictvím [TTL schopností](time-to-live.md), například 24 hodin a použití hodnota této vlastnosti k zachycení odstranění. S tímto řešením je nutné zpracovat změny v časovém intervalu kratší než doba TTL vypršení platnosti. Změna informačního kanálu je k dispozici pro každý oddíl klíče rozsahem v rámci kolekce dokumentů a proto mohou být distribuovány na jeden nebo více příjemce pro paralelní zpracování. 

![Distribuované zpracování změn Azure Cosmos DB kanálu](./media/change-feed/changefeedvisual.png)

Máte několik možností v tom, jak implementovat změnu kanálu v klientském kódu. Okamžitě následujících částech popisují, jak k implementaci změny kanálu pomocí rozhraní REST API Azure Cosmos DB a DocumentDB SDK. Ale pro aplikace .NET, doporučujeme používat novou [změnu kanálu procesoru knihovna](#change-feed-processor) pro zpracování události z změnu kanálu tak, jak zjednodušuje čtení změny mezi oddílů a umožňuje více podprocesů práce paralelní. 

## <a id="rest-apis"></a>Práce s rozhraní REST API a DocumentDB SDK
Azure Cosmos DB poskytuje elastické kontejnerů úložiště a propustnost názvem **kolekce**. Data v rámci kolekce je logicky seskupeny pomocí [oddílu klíče](partition-data.md) a výkon a škálovatelnost. Azure Cosmos DB poskytuje různé rozhraní API pro přístup k těmto datům, včetně vyhledávání podle ID (pro čtení nebo získat), dotazů a číst informační kanály (kontroly). Změna informačního kanálu je možné získat naplnění dva nové hlavičky požadavku k DocumentDB `ReadDocumentFeed` rozhraní API, může zpracovat paralelní napříč rozsahy klíčů oddílů.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed rozhraní API
Podívejme se stručný v tom, jak funguje ReadDocumentFeed. Azure Cosmos DB podporuje čtení dokumentů v rámci kolekce prostřednictvím informačního kanálu `ReadDocumentFeed` rozhraní API. Například následující požadavek vrátí stránku dokumenty uvnitř `serverlogs` kolekce. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Výsledky mohou být omezeny pomocí `x-ms-max-item-count` záhlaví a čtení lze obnovit pomocí opětovným odesláním požadavek s `x-ms-continuation` záhlaví, vrátí se v předchozí odpovědi. Při provádění z jednoho klienta, `ReadDocumentFeed` iteruje výsledky napříč oddíly sériově. 

**Sériové čtení dokumentu kanálu**

Můžete také načíst informační kanál dokumentů pomocí jedné z podporovaném [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md). Například následující fragment kódu ukazuje způsob použití [ReadDocumentFeedAsync metoda](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) v rozhraní .NET.

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a>Distribuované provádění ReadDocumentFeed
Pro kolekce, které obsahují terabajtů dat nebo víc nebo ingestování velký objem aktualizací nemusí být praktické sériové provádění čtení kanálu z jednoho klientského počítače. Aby bylo možné podporovat scénáře velkých objemů dat, Azure Cosmos DB poskytuje rozhraní API pro distribuci `ReadDocumentFeed` volání transparentně napříč více klienta čtečky na příjemců. 

**Informační kanál pro distribuované čtení dokumentu**

Zajistit škálovatelné zpracování přírůstkové změny Azure Cosmos DB podporuje model Škálováním na více systémů pro změnu kanálu rozhraní API založené na rozsahy klíčů oddílů.

* Můžete získat seznam oddílu rozsahy klíčů pro kolekci provádění `ReadPartitionKeyRanges` volání. 
* Pro každý rozsah klíče oddílu můžete provádět `ReadDocumentFeed` čtení dokumentů pomocí klíčů oddílů v tomto rozsahu.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Načítání oblastí klíče oddílu pro kolekci
Rozsahy klíčů oddílu můžete načíst tím, že požádá `pkranges` prostředků v rámci kolekce. Například následující požadavek načte seznam rozsahy klíče oddílu `serverlogs` kolekce:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

Tento požadavek vrátí následující odpověď obsahující metadata o rozsahy klíče oddílu:

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


**Oddílu Vlastnosti klíčové oblasti**: každý rozsah klíče oddílů zahrnují metadata vlastnosti v následující tabulce:

<table>
    <tr>
        <th>Název hlavičky</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>ID pro rozsah klíče oddílu. Toto je stabilní a jedinečné ID v každé kolekci.</p>
            <p>Musí být použít v následující volání ke čtení změny podle rozsahu klíče oddílu.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>Hodnota maximální oddílu hodnota hash klíče pro rozsah klíče oddílu. Pro interní použití.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>Hodnota hash klíče minimální oddílu pro rozsah klíče oddílu. Pro interní použití.</td>
    </tr>       
</table>

To provedete pomocí jedné z podporovaném [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md). Například následující fragment kódu ukazuje, jak načíst rozsahy klíčů oddílu v rozhraní .NET pomocí [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metoda.

```csharp
string pkRangesResponseContinuation = null;
List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

do
{
    FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri, 
        new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
}
while (pkRangesResponseContinuation != null);
```

Azure Cosmos DB podporuje načtení dokumentů na rozsah klíče oddílu pomocí nastavení volitelné `x-ms-documentdb-partitionkeyrangeid` záhlaví. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Provádění přírůstkové ReadDocumentFeed
ReadDocumentFeed podporuje následující scénáře a úkoly pro přírůstkové zpracování změny v kolekcích Azure Cosmos DB:

* Číst všechny změny do dokumentů od začátku, tedy z vytvoření kolekce.
* Číst všechny změny pro budoucí aktualizace do dokumentů z aktuálního času nebo změny od čas zadaného uživatelem.
* Číst všechny změny do dokumentů z verze logické kolekce (ETag). Kontrolní bod můžete uživatele podle vrácená značka ETag z přírůstkové žádostí kanálu pro čtení.

Změny zahrnují vložení a aktualizace do dokumentů. Můžete zaznamenat odstranění, musí používat vlastnost "obnovitelného odstranění" v rámci dokumentů nebo použít [předdefinované vlastnosti TTL](time-to-live.md) signál čeká na odstranění v změn informačního kanálu.

Následující tabulka uvádí [požadavku](/rest/api/documentdb/common-documentdb-rest-request-headers.md) a [hlavičky odpovědi](/rest/api/documentdb/common-documentdb-rest-response-headers.md) pro ReadDocumentFeed operace.

**Hlavičky požadavku pro přírůstkové ReadDocumentFeed**:

<table>
    <tr>
        <th>Název hlavičky</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>A ZASÍLÁNÍ RYCHLÝCH ZPRÁV</td>
        <td>Musí být nastavena na "Přírůstkové informační kanál", nebo tento parametr vynechán jinak</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Žádné záhlaví: vrátí všechny změny od začátku (vytvoření kolekce)</p>
            <p>"*": vrátí všechny nové změny dat v rámci kolekce</p>           
            <p>&lt;Značka Etag&gt;: Pokud vrátí sadu do kolekce značka ETag, všechny změny provedené od této logické časové razítko</p>
        </td>
    </tr>
    <tr>    
        <td>Pokud upravit – od</td> 
        <td>Formát času RFC 1123; Pokud je zadána If-None-Match ignorována</td> 
    </tr> 
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>ID klíče rozsahu oddílu pro čtení dat</td>
    </tr>
</table>

**Hlavičky odpovědi pro přírůstkové ReadDocumentFeed**:

<table> <tr>
        <th>Název hlavičky</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>Značka Etag</td>
        <td>
            <p>Logické pořadové číslo (položky LSN) poslední dokumentu vrácený v odpovědi.</p>
            <p>Přírůstkové ReadDocumentFeed můžete obnovit pomocí opětovným odesláním tuto hodnotu v If-None-Match.</p>
        </td>
    </tr>
</table>

Zde je ukázka požadavek na vrácení všechny přírůstkové změny v kolekci z logické verze nebo ETag `28535` a rozdělit na oddíly klíče rozsah = `16`:

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Změny jsou seřazené podle času v rámci každé hodnotu klíče oddílu v rozsahu klíče oddílu. Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu. Pokud existují další výsledky, než lze zobrazit v jediné stránce, si můžete přečíst další stránky výsledků podle opětovným odesláním požadavek s `If-None-Match` záhlaví s hodnotou rovna `etag` z předchozí odpovědi. Pokud více dokumentů se přidají nebo aktualizují transakčně v rámci uložené procedury nebo aktivační událost, budou se všechny vrátí na jedné stránce odpovědi.

> [!NOTE]
> S změnu kanálu, může získat více položek vrátí na stránce, než je zadáno v `x-ms-max-item-count` v případě více dokumentů, které se přidají nebo aktualizují v rámci uložené procedury nebo aktivační události. 

Když pomocí sady .NET SDK (1.17.0), nastavte hodnotu pole `StartTime` v `ChangeFeedOptions` přímo vrátit změněné dokumenty od `StartTime` při volání metody `CreateDocumentChangeFeedQuery`. Zadáním `If-Modified-Since` pomocí rozhraní REST API, vaši žádost o vrátí není dokumenty sami, ale spíš token pokračování nebo `etag` v hlavičce odpovědi. Vrátit zadaný čas, token pro pokračování změny dokumenty `etag` pak použije v další požadavek s `If-None-Match` vrátit skutečné dokumenty. 

.NET SDK poskytuje [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) a [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) pomocné třídy pro přístup k změny provedené v kolekci. Následující fragment kódu ukazuje, jak načíst všechny změny od začátku pomocí sady .NET SDK z jednoho klienta.

```csharp
private async Task<Dictionary<string, string>> GetChanges(
    DocumentClient client,
    string collection,
    Dictionary<string, string> checkpoints)
{
    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

    foreach (PartitionKeyRange pkRange in partitionKeyRanges)
    {
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);

        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collection,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = 1
            });

        while (query.HasMoreResults)
        {
            FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

            foreach (DeviceReading changedDocument in readChangesResponse)
            {
                Console.WriteLine(changedDocument.Id);
            }

            checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
        }
    }

    return checkpoints;
}
```
A následující fragment kódu ukazuje, jak zpracovat změny v reálném čase s Azure DB Cosmos pomocí kanálu změnu podporu a předchozí funkce. První volání vrátí všechny dokumenty v kolekci, a druhá pouze vrátí vytvořili dva dokumenty, které byly vytvořeny od posledního kontrolního bodu.

```csharp
// Returns all documents in the collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only the two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

Můžete také filtrovat změnu kanálu pomocí logiky straně klienta se selektivně zpracovat události. Například zde je fragment kódu, který používá na straně klienta LINQ ke zpracování události změny pouze teploty ze senzorů zařízení.

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <a id="change-feed-processor"></a>Knihovna kanálu procesoru změn
Další možností je použít [knihovny Azure Cosmos DB změnit kanálu procesoru](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), který vám může pomoct snadno distribuovat zpracování z důvodu změny kanálu napříč více příjemců událostí. Knihovna je skvěle hodí k vytváření změnu kanálu čtečky na platformě .NET. Některé pracovní postupy, které by zjednodušit pomocí knihovně procesoru kanálu změn prostřednictvím metod obsažených v jiných SDK DB Cosmos patří: 

* Vyžádání aktualizace z změnu informačního kanálu, když data budou uložená napříč více oddílů
* Přesunutí nebo replikaci dat z jedné kolekce do jiného
* Paralelní provádění akcí, které jsou aktivovány aktualizace dat a změna kanálu 

Zatímco pomocí rozhraní API v Cosmos SDK poskytuje přesné přístup k změnit informačního kanálu aktualizace v každém oddílu, pomocí knihovny změnit kanálu procesoru zjednodušuje čtení změny mezi oddílů a paralelně fungujících více vláken. Procesor kanálu změnu místo ručně čtení změny z každý kontejner a ukládání token pokračování pro každý oddíl, automaticky spravuje čtení změny napříč oddíly používající mechanismus zapůjčení.

Je k dispozici jako balíčku NuGet, knihovna: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) a ze zdrojového kódu jako Githubu [ukázka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor). 

### <a name="understanding-change-feed-processor-library"></a>Principy změn kanálu procesoru knihovny 

Existují čtyři hlavní součásti implementace změnu kanálu procesoru: monitorovaných kolekce, kolekce zapůjčení, procesoru hostitele a uživatelé. 

**Monitorované kolekci:** monitorovaných kolekce je dat, ze které se generuje změnu informačního kanálu. Všechny operace INSERT a změny monitorovaných kolekce se projeví v informačním kanálu změnu kolekce. 

**Kolekce zapůjčení:** souřadnice kolekce zapůjčení zpracování změn kanálu napříč více pracovníků. Samostatné kolekce slouží k uložení zapůjčení jeden zapůjčení na jeden oddíl. Je výhodné pro uložení této kolekce zapůjčení na jiný účet s oblasti zápisu blíže k se spuštěným systémem změnu kanálu procesoru. Objekt zapůjčení obsahuje následující atributy: 
* Vlastník: Určuje hostitele, který vlastní zapůjčení
* Pokračování: Určuje pozici (token pokračování) v kanálu pro určitý oddíl změn
* Časové razítko: Čas poslední zapůjčení byla aktualizována; časové razítko slouží ke kontrole, jestli zapůjčení je považována za ukončenou 

**Procesor hostitele:** každého hostitele Určuje, kolik oddíly procesu na základě kolik instancí hostitelů mají aktivní zapůjčení. 
1.  Po spuštění hostitele získá zapůjčení vyrovnávat zatížení ve všech hostitelích. Hostitel pravidelně obnovuje zapůjčení, aby zůstala aktivní zapůjčení. 
2.  Kontrolní body hostitele poslední token pokračování jeho zapůjčení pro každou přečíst. K zajištění bezpečnosti souběžnosti, zkontroluje hostitele ETag pro jednotlivé aktualizace zapůjčení. Jsou podporovány také další strategie kontrolního bodu.  
3.  Při vypnutí počítače Hostitel uvolní všechny zapůjčení, ale zachová pokračování informace, takže ho můžete obnovit čtení z uložené kontrolního bodu později. 

V tuto chvíli nemůže být větší než počet oddílů (zapůjčení) počtu hostitelů.

**Příjemci knihovny:** příjemci nebo pracovníci, jsou vláken, které provádějí změnu kanálu zpracování iniciovaná každého hostitele. Každý hostitel procesoru může mít více příjemců. Jednotliví spotřebitelé přečte změnu kanálu z oddílu, který je přiřazen k a upozorní jeho hostitel změny a platnost zapůjčení.

Abyste pochopili, jak tyto čtyři prvky změnu kanálu procesoru spolupracují, podíváme se na příklad na následujícím diagramu. Monitorované kolekci ukládá dokumenty a používá "city" jako klíč oddílu. Vidíte, že blue oddíl obsahuje dokumentů s poli "city" od "A-E" a tak dále. Existují dva hostitele, každý se dvěma spotřebiteli čtení ze čtyř oddílů souběžně. Šipky zobrazují příjemci čtení z na určité místo v změn informačního kanálu. Do prvního oddílu představuje tmavšího blue nepřečtená změny při světla blue představuje již čtení změny při změně kanálu. Hostitele použít k ukládání hodnotu "pokračování" ke sledování aktuální pozici čtení pro každý příjemce kolekci zapůjčení. 

![Pomocí Azure Cosmos DB změnu kanálu procesor hostitele](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a>Pomocí změnu kanálu procesoru knihovny 
V následující části vysvětluje, jak na používání knihovny změnu kanálu procesoru v kontextu replikace změn z kolekce zdrojové do cílové kolekce. Zde zdrojové kolekci je monitorované kolekci změnu kanálu procesor. 

**Nainstalovat a zahrnout balíček NuGet procesoru kanálu změn** 

Před instalací balíčku NuGet změnu kanálu procesoru, nejprve nainstalujte: 
* Microsoft.Azure.DocumentDB, verze 1.13.1 nebo novější 
* Newtonsoft.Json, verze 9.0.1 nebo vyšší instalace `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` a její zahrnutí jako odkaz.

**Vytvoření monitorovaných, zapůjčení a cílové kolekce** 

Chcete-li použít knihovna procesoru kanálu změn, musí být vytvořen před spuštěním procesor hostitel kolekci zapůjčení. Znovu doporučujeme uložit kolekci zapůjčení na jiný účet s oblastí zápisu blíže k se spuštěným systémem změnu kanálu procesoru. V tomto příkladu přesun dat potřebujeme vytvoření cílové kolekce před spuštěním změnu kanálu procesoru hostitele. Ukázkový kód jsme volat metodu helper k vytvoření monitorovaných, pronajaté, a cílové kolekce, pokud dosud neexistují. 

> [!WARNING]
> Vytvoření kolekce hradí, jako jsou rezervování propustnost pro aplikace komunikovat s Azure Cosmos DB. Další podrobnosti naleznete [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

*Vytvoření procesor hostitele*

`ChangeFeedProcessorHost` Třída poskytuje bezpečné pro přístup z více vláken, více procesů, bezpečné běhového prostředí pro implementace zpracovatelů událostí taky poskytuje možnost vytváření kontrolních bodů a oddíl správu zapůjčení. Použít `ChangeFeedProcessorHost` třídu, můžete implementovat `IChangeFeedObserver`. Toto rozhraní obsahuje tři metody:

* `OpenAsync`: Tato funkce je volána, když je otevřen pozorovatel změnu informačního kanálu. Ho můžete upravit tak, aby odpovídat určité akci, když je otevřen příjemce/pozorovatele.  
* `CloseAsync`: Tato funkce je volána, když je ukončen informačního kanálu pozorovatel změnu. Může být upraven k provedení určité akce při zavření příjemce/pozorovatele.  
* `ProcessChangesAsync`: Tato funkce je volána, když jsou k dispozici na změnu kanálu nové změny dokumentu. Může být upraven k provedení určité akce při každé změně kanálu aktualizace.  

V našem příkladu jsme toto rozhraní implementovat `IChangeFeedObserver` prostřednictvím `DocumentFeedObserver` třídy. Zde `ProcessChangesAsync` funkce upserts (aktualizace) dokumentu z změnu kanálu do cílové kolekce. V tomto příkladu je užitečná pro přesun dat z jedné kolekce do jiného, aby bylo možné změnit klíč oddílu datové sady. 

*Spuštění procesoru hostitele*

Před zahájením zpracování událostí, jak změnit možnosti informačního kanálu a změnit možnosti informačního kanálu hostitele je možné upravit. 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval to 15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
V následujících tabulkách jsou shrnuté konkrétních polí, které se dají přizpůsobit. 

**Změna možností informačního kanálu**:
<table>
    <tr>
        <th>Název vlastnosti</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>MaxItemCount</td>
        <td>Získá nebo nastaví maximální počet položek má být vrácen v operaci výčtu ve službě Azure Cosmos DB databáze.</td>
    </tr>
    <tr>
        <td>PartitionKeyRangeId</td>
        <td>Získá nebo nastaví id klíče rozsahu oddílu pro aktuální požadavek ve službě Azure Cosmos DB databáze.</td>
    </tr>
    <tr>
        <td>RequestContinuation</td>
        <td>Získá nebo nastaví token pokračování požadavku ve službě Azure Cosmos DB databáze.</td>
    </tr>
        <tr>
        <td>SessionToken</td>
        <td>Získá nebo nastaví token relace pro použití s konzistence typu relace ve službě Azure Cosmos DB databáze.</td>
    </tr>
        <tr>
        <td>StartFromBeginning</td>
        <td>Získá nebo nastaví, zda změna kanálu ve službě Azure Cosmos DB databáze by se měl spustit od začátku (true) nebo z aktuální (false). Ve výchozím nastavení spustí z aktuální (false).</td>
    </tr>
</table>

**Změna možností informačního kanálu hostitele**:
<table>
    <tr>
        <th>Název vlastnosti</th>
        <th>Typ</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>LeaseRenewInterval</td>
        <td>Časový interval</td>
        <td>Interval pro všechny zapůjčení pro oddíly, které jsou aktuálně uchovávat ChangeFeedEventHost instance.</td>
    </tr>
    <tr>
        <td>LeaseAcquireInterval</td>
        <td>Časový interval</td>
        <td>Interval, který ji úlohu k výpočtu, zda jsou oddíly rovnoměrně rozdělené mezi instancí známé hostitele.</td>
    </tr>
    <tr>
        <td>LeaseExpirationInterval</td>
        <td>Časový interval</td>
        <td>Interval, pro kterou je zapůjčení pořízené zapůjčení představující oddílu. Pokud během tohoto intervalu neobnovíte zapůjčení, vypršela platnost a vlastnictví oddílu přesune do jiné instance ChangeFeedEventHost.</td>
    </tr>
    <tr>
        <td>FeedPollDelay</td>
        <td>Časový interval</td>
        <td>Zpoždění mezi dotazování oddíl pro nové změny na informačního kanálu, po všechny aktuální změny se nečekaně.</td>
    </tr>
    <tr>
        <td>CheckpointFrequency</td>
        <td>CheckpointFrequency</td>
        <td>Četnost zapůjčení kontrolního bodu.</td>
    </tr>
    <tr>
        <td>MinPartitionCount</td>
        <td>celá čísla</td>
        <td>Minimální počet pro hostitele.</td>
    </tr>
    <tr>
        <td>MaxPartitionCount</td>
        <td>celá čísla</td>
        <td>Maximální počet oddílů, které hostitele může obsluhovat.</td>
    </tr>
    <tr>
        <td>DiscardExistingLeases</td>
        <td>BOOL</td>
        <td>Jestli se při spuštění hostitele, měla by být odstraněna všechna existující zapůjčení a hostitele by měla začít úplně od začátku.</td>
    </tr>
</table>


Pokud chcete zahájit zpracování událostí, vytvořte instanci `ChangeFeedProcessorHost`, poskytnutím příslušných parametrů pro svoji kolekci Azure Cosmos DB. Potom zavolejte `RegisterObserverAsync` k registraci vašeho `IChangeFeedObserver` implementace (DocumentFeedObserver v tomto příkladu) s modulem runtime. V tomto okamžiku hostitel pokusí "zapůjčit" každý oddíl klíče rozsahu v Azure Cosmos DB kolekce s použitím "chamtivého" algoritmu. Tato zapůjčení poslední po stanovenou dobu a následně musí být obnovena. Jako nové uzly a instancí pracovního procesu, v takovém případě režimu online, se umístí své rezervace zapůjčení a průběhu času zatížení posune mezi uzly, protože každý hostitel bude snažit získat další zapůjčení. 

V ukázkovém kódu používáme třídu objektů factory (DocumentFeedObserverFactory.cs) k vytvoření pozorovatele a `RegistObserverFactoryAsync` k registraci pozorovatele. 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter to stop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
Postupem času se dosáhne rovnováhy. Tato dynamická funkce umožňuje využití procesoru na základě automatického škálování má být použita k příjemce pro škálování nahoru a dolů. Pokud jsou k dispozici v Azure Cosmos DB s vyšší rychlostí než mohou příjemci zpracovat změny, zvýšení využití procesoru na spotřebitele slouží k dojít k automatickému škálování počtu instancí pracovních procesů.

## <a name="next-steps"></a>Další kroky
* Zkuste [Azure Cosmos DB změnu kanálu ukázky kódu na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)
* Začínáme s kódování [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) nebo [REST API](/rest/api/documentdb/).
