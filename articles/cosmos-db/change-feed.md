---
title: "aaaWorking s hello změnu kanálu podpory v Azure Cosmos DB | Microsoft Docs"
description: "Použití Azure Cosmos DB změnit informačního kanálu podporu tootrack změny v dokumentech a provádět na základě událostí zpracování jako aktivační události a průběžná aktualizace mezipaměti a analýzy systémy."
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
ms.openlocfilehash: a4dcf4ceb476e3e08266dbcdcbee1d75e1d1eed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a>Práce s hello změnu informačního kanálu podpory v Azure Cosmos DB
[Azure Cosmos DB](../cosmos-db/introduction.md) je rychlého a flexibilní globálně replikované databáze služby, který slouží k ukládání velkých objemů dat transakcí a funkční s latencí předvídatelný jednociferné milisekund pro čtení a zápisu. Díky tomu dobře hodí pro IoT, hry, maloobchodní a provozní protokolování aplikace. Běžné vzoru návrhu v těchto aplikacích je tootrack změny provedené tooAzure dat Cosmos databáze a aktualizovat materializovaným zobrazením, provádět analýzu v reálném čase, archivace dat toocold úložiště a aktivovat oznámení na určité události na základě těchto změn. Hello **změnu kanálu podporu** v Azure Cosmos DB vám umožní toobuild efektivní a škálovatelné řešení pro každou z těchto vzorků.

Změny kanálu podpory poskytuje Azure Cosmos DB seřazený seznam dokumenty v kolekci Azure Cosmos DB v hello pořadí, ve kterém byly upraveny. Tento informační kanál lze použít toolisten pro úpravy toodata v rámci kolekce hello a provádět akce, jako například:

* Aktivovat tooan volání rozhraní API, kdy je dokument vložit nebo úpravě
* Na aktualizace provést zpracování v reálném čase (proud)
* Synchronizaci dat s mezipaměti, vyhledávací web nebo datového skladu

Změny v Azure Cosmos DB jsou nastavené jako trvalé může být zpracována asynchronně a distribuovaná do jednoho nebo více spotřebitelů pro paralelní zpracování. Podívejme se na hello rozhraní API pro změnu kanálu a jak lze využít toobuild škálovatelné aplikace v reálném čase. Tento článek ukazuje, jak změnit toowork s Azure Cosmos DB informační kanál a hello DocumentDB rozhraní API. 

![Pomocí Azure Cosmos DB změnu kanálu toopower analýzu v reálném čase a událostmi řízené výpočetní scénáře](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Změna kanálu podpora je k dispozici pouze pro hello DocumentDB rozhraní API v tuto chvíli; Hello rozhraní Graph API a rozhraní API tabulky nejsou aktuálně podporovány.

## <a name="use-cases-and-scenarios"></a>Případy použití a scénáře
Změna kanálu umožňuje efektivní zpracování rozsáhlých datových sad k velkému počtu zápisy a nabízí tooidentify alternativní tooquerying celé datové sady, co se změnilo. Například můžete provádět následující úlohy efektivně hello:

* Aktualizace mezipaměti, index vyhledávání nebo datového skladu s daty uloženými v databázi Azure Cosmos.
* Využití dat vrstvení a archivaci úrovni aplikace, tedy ukládání "horkých dat." v Azure Cosmos DB a po určité době odstraněny "pomaleji přístupná data" příliš[Azure Blob Storage](../storage/common/storage-introduction.md) nebo [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementace batch analytics na data pomocí [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Implementace [lambda kanály v Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) s Azure Cosmos DB. Azure Cosmos DB poskytuje řešení škálovatelná databáze, které může zpracovat přijímání a dotazů a implementovat lambda architektury s nízkou celkové náklady na vlastnictví. 
* Proveďte nulové době migrace tooanother účet Azure Cosmos DB jiné schéma rozdělení oddílů.

**Lambda kanálů s Azure DB Cosmos pro přijímání a dotazů:**

![Azure Cosmos DB na základě lambda kanálu pro přijímání a dotazů](./media/change-feed/lambda.png)

Můžete použít Azure Cosmos DB tooreceive a ukládání dat události ze zařízení, senzorů, infrastruktury a aplikace a zpracování těchto událostí v reálném čase pomocí [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), nebo [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

V rámci vaší [bez serveru](http://azure.com/serverless) webových a mobilních aplikací, můžete sledovat událostmi, jako je například zákazníka tooyour změny profilu, předvolby nebo umístění tootrigger určité akce, jako je odesílání nabízených oznámení tootheir zařízení pomocí [Azure Functions](../azure-functions/functions-bindings-documentdb.md) nebo [aplikační služby](https://azure.microsoft.com/services/app-service/). Pokud používáte Azure Cosmos DB toobuild hry, můžete vytvořit, například změna tooimplement informačního kanálu v reálném čase tabulky podle skóre z dokončené hry.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Jak funguje změnu informační kanál v Azure Cosmos DB
Azure Cosmos DB poskytuje možnost tooincrementally hello číst aktualizace provedené tooan Azure Cosmos DB kolekce. Tento informační kanál změnu má hello následující vlastnosti:

* Změny jsou trvalé v Azure Cosmos DB a může být zpracována asynchronně.
* Toodocuments změny v kolekci jsou k dispozici okamžitě v kanálu změnu hello.
* Každý dokument tooa změn se zobrazí právě jednou v kanálu změnu hello a klienti spravovat své logiku vytváření kontrolních bodů. Knihovna informačního kanálu procesoru změn Hello poskytuje automatické vytváření kontrolních bodů a "alespoň jednou" sémantiku.
* Protokol změn hello je součástí pouze poslední změny hello u daného dokumentu. Přechodných změn nemusí být k dispozici.
* informační kanál změnu Hello je řazen u úpravy v rámci každé hodnotu klíče oddílu. Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu.
* Změny mohou být synchronizovány z jakékoli bodu v čase, tedy neexistuje žádné období uchovávání pevné dat, pro které jsou k dispozici změny.
* Změny jsou k dispozici v bloky rozsahy klíčů oddílů. Tato možnost umožňuje změny z toobe rozsáhlých kolekcí, které jsou zpracovávány paralelně více příjemci nebo serverů.
* Aplikace můžete žádost o změnu více informačních kanálů současně na hello stejné kolekci.

Informační kanál Azure Cosmos DB změnu je povoleno ve výchozím nastavení pro všechny účty. Můžete použít vaše [zřízené propustnosti](request-units.md) ve vaší oblasti zápisu ani žádné [číst oblast](distribute-data-globally.md) tooread z hello změnit informačního kanálu, stejně jako všechny ostatní operace z Azure Cosmos DB. informační kanál změnu Hello zahrnuje vložení a operace aktualizace provedené toodocuments v rámci kolekce hello. Můžete zaznamenat odstranění nastavením příznak "soft odstranění" v rámci dokumentů místo odstranění. Alternativně můžete nastavit konečný platnosti pro dokumentů prostřednictvím hello [TTL schopností](time-to-live.md)pro příklad, 24 hodin a použití hello hodnota této vlastnosti toocapture odstraní. Toto řešení máte tooprocess změny v časovém intervalu kratší než období platnosti TTL hello. informační kanál změnu Hello je k dispozici pro každý oddíl klíče rozsahem v rámci kolekce dokumentů hello a proto mohou být distribuovány na jeden nebo více příjemce pro paralelní zpracování. 

![Distribuované zpracování změn Azure Cosmos DB kanálu](./media/change-feed/changefeedvisual.png)

Máte několik možností v tom, jak implementovat změnu kanálu v klientském kódu. Hello částech, která okamžitě postupujte podle popisují, jak tooimplement hello změnu kanálu pomocí hello REST API služby Azure Cosmos DB a hello DocumentDB SDK. Ale pro aplikace .NET, doporučujeme používat hello nové [změnu kanálu procesoru knihovna](#change-feed-processor) pro zpracování událostí z hello změnit kanálu jako jeho zjednodušuje čtení změny mezi oddílů a umožňuje více podprocesů práce paralelní. 

## <a id="rest-apis"></a>Práce s hello REST API a DocumentDB SDK
Azure Cosmos DB poskytuje elastické kontejnerů úložiště a propustnost názvem **kolekce**. Data v rámci kolekce je logicky seskupeny pomocí [oddílu klíče](partition-data.md) a výkon a škálovatelnost. Azure Cosmos DB poskytuje různé rozhraní API pro přístup k těmto datům, včetně vyhledávání podle ID (pro čtení nebo získat), dotazů a číst informační kanály (kontroly). Hello změnu kanálu je možné získat naplnění dva nové žádosti o hlavičky toohello DocumentDB `ReadDocumentFeed` rozhraní API, může zpracovat paralelní napříč rozsahy klíčů oddílů.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed rozhraní API
Podívejme se stručný v tom, jak funguje ReadDocumentFeed. Azure Cosmos DB podporuje čtení informačního kanálu dokumentů v rámci kolekce prostřednictvím hello `ReadDocumentFeed` rozhraní API. Například hello následující požadavek vrátí stránku dokumenty uvnitř hello `serverlogs` kolekce. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Výsledky mohou být omezeny pomocí hello `x-ms-max-item-count` záhlaví a čtení lze obnovit pomocí opětovným odesláním požadavku hello s `x-ms-continuation` záhlaví, vrátí se v předchozí odpovědi hello. Při provádění z jednoho klienta, `ReadDocumentFeed` iteruje výsledky napříč oddíly sériově. 

**Sériové čtení dokumentu kanálu**

Můžete také načíst informační kanál hello dokumentů pomocí jedné z hello podporované [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md). Například hello následující fragment kódu ukazuje, jak toouse hello [ReadDocumentFeedAsync metoda](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) v rozhraní .NET.

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a>Distribuované provádění ReadDocumentFeed
Pro kolekce, které obsahují terabajtů dat nebo víc nebo ingestování velký objem aktualizací nemusí být praktické sériové provádění čtení kanálu z jednoho klientského počítače. V pořadí toosupport tyto scénáře velkých objemů dat, Azure Cosmos DB poskytuje rozhraní API toodistribute `ReadDocumentFeed` volání transparentně napříč více klienta čtečky na příjemců. 

**Informační kanál pro distribuované čtení dokumentu**

tooprovide škálovatelné zpracování přírůstkové změny, Azure Cosmos DB podporuje model Škálováním na více systémů pro změnu hello kanálu rozhraní API založené na rozsahy klíčů oddílů.

* Můžete získat seznam oddílu rozsahy klíčů pro kolekci provádění `ReadPartitionKeyRanges` volání. 
* Pro každý rozsah klíče oddílu můžete provádět `ReadDocumentFeed` tooread dokumentů pomocí klíčů oddílů v tomto rozsahu.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Načítání oblastí klíče oddílu pro kolekci
Rozsahy klíčů hello oddílu můžete načíst pomocí požadavku hello `pkranges` prostředků v rámci kolekce. Například hello následující požadavek načte seznam hello rozsahy klíčů oddílu pro hello `serverlogs` kolekce:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

Tento požadavek vrátí hello následující odpověď obsahující metadata o rozsahy klíčů oddílu hello:

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


**Oddílu Vlastnosti klíčové oblasti**: každý oddíl klíče rozsah obsahuje vlastnosti metadat hello v hello následující tabulka:

<table>
    <tr>
        <th>Název hlavičky</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>Hello ID pro rozsah klíče oddílu hello. Toto je stabilní a jedinečné ID v každé kolekci.</p>
            <p>Musí být použít v hello následující volání tooread změny podle rozsahu klíče oddílu.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>Hodnota klíče hash Hello maximální oddílu pro rozsah klíče oddílu hello. Pro interní použití.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>Hello oddíl minimální hodnota hash klíče hodnotu rozsahu klíče oddílu hello. Pro interní použití.</td>
    </tr>       
</table>

To provedete pomocí jedné z hello podporované [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md). Například hello následující fragment kódu ukazuje, jak klíč oddílu tooretrieve rozsahy v rozhraní .NET pomocí hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metoda.

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

Azure Cosmos DB podporuje načtení dokumentů na rozsah klíče oddílu podle nastavení hello volitelné `x-ms-documentdb-partitionkeyrangeid` záhlaví. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Provádění přírůstkové ReadDocumentFeed
ReadDocumentFeed podporuje následující scénáře a úkoly pro přírůstkové zpracování změny v kolekcích Azure Cosmos DB hello:

* Číst všechny změny toodocuments od začátku hello, to znamená, od vytvoření kolekce.
* Číst všechny změny toofuture aktualizace toodocuments z aktuální čas nebo změny od čas zadaného uživatelem.
* Číst všechny změny toodocuments z verze logické kolekce hello (ETag). Můžete kontrolního bodu uživatele podle hello vrácená značka ETag přírůstkové požadavky kanálu pro čtení.

Hello změny zahrnují toodocuments operace INSERT a Update. Odstraní toocapture, musíte použít vlastnost "obnovitelného odstranění" v rámci dokumentů, nebo použijte hello [předdefinované vlastnosti TTL](time-to-live.md) toosignal čeká na odstranění ve hello změnit informačního kanálu.

Hello následující tabulky seznamy hello [požadavku](/rest/api/documentdb/common-documentdb-rest-request-headers.md) a [hlavičky odpovědi](/rest/api/documentdb/common-documentdb-rest-response-headers.md) pro ReadDocumentFeed operace.

**Hlavičky požadavku pro přírůstkové ReadDocumentFeed**:

<table>
    <tr>
        <th>Název hlavičky</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>A ZASÍLÁNÍ RYCHLÝCH ZPRÁV</td>
        <td>Musí být nastaven příliš "Přírůstkové kanálu", nebo tento parametr vynechán jinak</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Žádné záhlaví: vrátí všechny změny z hello od (vytvoření kolekce)</p>
            <p>"*": vrátí všechny nové toodata změny v kolekci hello</p>         
            <p>&lt;Značka Etag&gt;: Pokud nastavit tooa kolekce značka ETag, vrátí všechny změny provedené od této logické časové razítko</p>
        </td>
    </tr>
    <tr>    
        <td>Pokud upravit – od</td> 
        <td>Formát času RFC 1123; Pokud je zadána If-None-Match ignorována</td> 
    </tr> 
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>ID klíče rozsahu oddílu Hello pro čtení dat</td>
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
            <p>Hello logické pořadové číslo (položky LSN) poslední dokumentu vrácený v odpovědi hello.</p>
            <p>Přírůstkové ReadDocumentFeed můžete obnovit pomocí opětovným odesláním tuto hodnotu v If-None-Match.</p>
        </td>
    </tr>
</table>

Tady je tooreturn požadavku ukázkové všechny přírůstkové změny v kolekci z ETag logické verze hello `28535` a rozdělit na oddíly klíče rozsah = `16`:

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

Změny jsou seřazené podle času v rámci každé hodnotu klíče oddílu v rozsahu klíče oddílu hello. Neexistuje žádné zaručenou objednávka napříč hodnoty klíč oddílu. Pokud existují další výsledky, než lze zobrazit v jediné stránce, si můžete přečíst další stránky výsledků hello podle opětovným odesláním požadavku hello s hello `If-None-Match` hlavička s hodnota rovna toohello `etag` z předchozí odpovědi hello. Pokud více dokumentů se přidají nebo aktualizují transakčně v rámci uložené procedury nebo aktivační událost, budou všechny vrátí v rámci hello stejné stránky odpovědi.

> [!NOTE]
> S změnu kanálu, může získat více položek vrátí na stránce, než je zadáno v `x-ms-max-item-count` v případě hello více dokumentů, které se přidají nebo aktualizují v rámci uložené procedury nebo aktivační události. 

Pokud používáte hello .NET SDK (1.17.0), nastavte pole hello `StartTime` v `ChangeFeedOptions` toodirectly návratový změnit dokumenty od `StartTime` při volání metody `CreateDocumentChangeFeedQuery`. Zadáním `If-Modified-Since` pomocí hello REST API, vaši žádost o vrátí není hello dokumenty sami, ale spíš token pokračování hello nebo `etag` v hello hlavičky odpovědi. dokumenty hello tooreturn upravené hello zadaný čas, token pokračování hello `etag` pak použije v hello další žádosti se `If-None-Match` tooreturn hello skutečné dokumenty. 

Hello .NET SDK poskytuje hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) a [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) pomocné rutiny třídy kolekce tooa tooaccess změny. Hello následující fragment kódu ukazuje, jak tooretrieve všechny změny ze začátku hello pomocí .NET SDK hello z jednoho klienta.

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
A hello následující fragment kódu ukazuje, jak tooprocess změny v reálném čase s Azure DB Cosmos pomocí hello změnu kanálu podporu a hello předcházející funkce. první volání Hello vrátí všechny dokumenty hello v kolekci hello a hello druhý pouze vrátí hello dva dokumenty vytvořené, které byly vytvořeny od posledního kontrolního bodu hello.

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

Můžete také filtrovat hello změnu kanálu pomocí události procesu tooselectively logiku na straně klienta. Například zde je fragment, používající klienta straně LINQ tooprocess jen teploty události změn ze senzorů zařízení.

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <a id="change-feed-processor"></a>Knihovna kanálu procesoru změn
Další možností je toouse hello [knihovny Azure Cosmos DB změnit kanálu procesoru](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), který vám může pomoct snadno distribuovat zpracování z důvodu změny kanálu napříč více příjemců událostí. Knihovna Hello je skvěle hodí k vytváření změnu kanálu čtečky na platformě .NET hello. Některé pracovní postupy, které by zjednodušit pomocí knihovny změnu kanálu procesoru hello prostřednictvím metody hello součástí hello dalších sadách SDK DB Cosmos patří: 

* Vyžádání aktualizace z změnu informačního kanálu, když data budou uložená napříč více oddílů
* Přesunutí nebo replikace dat z jedné kolekce tooanother
* Paralelní provádění akcí aktivovány toodata aktualizace a změny kanálu 

Při použití hello rozhraní API v hello Cosmos SDK poskytuje přesné přístup toochange kanálu aktualizace v každém oddílu, pomocí knihovny změnu kanálu procesoru hello zjednodušuje čtení změny mezi oddílů a paralelně fungujících více vláken. Místo ručně čtení změny z každý kontejner a ukládání token pokračování pro každý oddíl hello změnu kanálu procesoru automaticky spravuje čtení změny napříč oddíly používající mechanismus zapůjčení.

Knihovna Hello je k dispozici jako balíčku NuGet: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) a ze zdrojového kódu jako Githubu [ukázka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor). 

### <a name="understanding-change-feed-processor-library"></a>Principy změn kanálu procesoru knihovny 

Existují čtyři hlavní součásti implementace hello změnu kanálu procesoru: hello monitorovat kolekce, kolekce hello zapůjčení, hello procesoru hostitele a spotřebitelé hello. 

**Monitorované kolekci:** hello monitorovat kolekce je hello dat, ze které hello se generuje změnu informačního kanálu. Jakékoli kolekce toohello monitorovat vložení a změny se projeví v kanálu změnu hello hello kolekce. 

**Kolekce zapůjčení:** hello souřadnice kolekce zapůjčení zpracování změn hello kanálu napříč více pracovníků. Samostatné kolekce je použité toostore hello zapůjčení jeden zapůjčení na jeden oddíl. Je výhodné toostore zápisu této kolekce zapůjčení na jiný účet s hello oblast blíže toowhere hello, které běží změnu kanálu procesoru. Objekt zapůjčení obsahuje hello následující atributy: 
* Vlastník: Určuje hello hostitele, který vlastní zapůjčení hello
* Pokračování: Určuje pozici hello (token pokračování) v kanálu pro určitý oddíl změn hello
* Časové razítko: Čas poslední zapůjčení byla aktualizována; časové razítko Hello může být použité toocheck, zda text hello zapůjčení je považována za ukončenou 

**Procesor hostitele:** každého hostitele Určuje, kolik tooprocess oddíly na základě kolik instancí hostitelů mají aktivní zapůjčení. 
1.  Po spuštění hostitele získá zapůjčení toobalance hello zatížení ve všech hostitelích. Hostitel pravidelně obnovuje zapůjčení, aby zůstala aktivní zapůjčení. 
2.  Hostitele kontrolní body hello poslední pokračovací token tooits zapůjčení pro každý čtení. tooensure souběžnosti zabezpečení, hostitel ověří hello ETag pro jednotlivé aktualizace zapůjčení. Jsou podporovány také další strategie kontrolního bodu.  
3.  Při vypnutí počítače Hostitel uvolní všechny zapůjčení ale udržuje hello pokračování informace, takže ho můžete obnovit čtení z uložené kontrolního bodu hello později. 

V tuto chvíli nemůže být větší než hello počet oddílů (zapůjčení) hello počtu hostitelů.

**Příjemci knihovny:** příjemci nebo pracovníci, jsou vláken, které provádějí změnu hello kanálu zpracování iniciovaná každého hostitele. Každý hostitel procesoru může mít více příjemců. Každý příjemce čte hello změnu kanálu z hello oddílu je přiřazen tooand upozorní jeho hostitel změny a platnost zapůjčení.

toofurther pochopit, jak tyto čtyři prvky změnu kanálu procesoru spolupracují, podíváme se na příklad v hello následující diagram. Hello monitorované kolekci úložiště dokumentů a jako klíč oddílu hello používá města"hello". Vidíte, že hello blue oddíl obsahuje dokumentů s hello pole "city" z "A-E" a tak dále. Existují dva hostitele, každý se dvěma spotřebiteli čtení z hello čtyřmi oddíly paralelně. Hello šipky zobrazují hello příjemci čtení z na určité místo v hello změnit informačního kanálu. V první oddíl hello představuje tmavšího blue hello nepřečtená změny při světla blue hello představuje hello již číst změny na změnu hello informačního kanálu. hello zapůjčení kolekce toostore zaznamenávat pro hodnotu tookeep "pokračování" hello aktuální pozici pro každý příjemce čtení nastavení používají hostitelé Hello. 

![Pomocí Azure Cosmos DB změnu hello kanálu procesor hostitele](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a>Pomocí změnu kanálu procesoru knihovny 
Hello následující část popisuje, jak toouse hello knihovny změnu kanálu procesoru v kontextu hello replikace změn z zdroj kolekce tooa cílové kolekce. Zde hello zdrojové kolekci je kolekce hello monitorovat procesor změnu informačního kanálu. 

**Nainstalovat a zahrnout balíček NuGet procesoru kanálu změnu hello** 

Před instalací balíčku NuGet změnu kanálu procesoru, nejprve nainstalujte: 
* Microsoft.Azure.DocumentDB, verze 1.13.1 nebo novější 
* Newtonsoft.Json, verze 9.0.1 nebo vyšší instalace `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` a její zahrnutí jako odkaz.

**Vytvoření monitorovaných, zapůjčení a cílové kolekce** 

V pořadí toouse hello knihovna procesoru kanálu změn musí kolekce zapůjčení hello toobe vytvořit před spuštěním hello procesor hostitel. Znovu doporučujeme uložit kolekci zapůjčení na jiný účet s co nejblíže toowhere hello oblast zápisu, že změna kanálu procesor běží. V tomto příkladu přesun dat potřebujeme toocreate hello cílové kolekce před spuštěním hello změnu kanálu procesoru hostitele. V ukázkovém kódu hello říkáme hello Pomocná metoda toocreate monitorovat pronajaté a cílové kolekce, pokud dosud neexistují. 

> [!WARNING]
> Vytvoření kolekce hradí, jako jsou rezervování propustnost pro hello toocommunicate aplikací s Azure Cosmos DB. Další podrobnosti naleznete na adrese hello [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

*Vytvoření procesor hostitele*

Hello `ChangeFeedProcessorHost` třída poskytuje bezpečné pro přístup z více vláken, více procesů, bezpečné běhového prostředí pro implementace zpracovatelů událostí taky poskytuje možnost vytváření kontrolních bodů a oddíl správu zapůjčení. toouse hello `ChangeFeedProcessorHost` třídu, můžete implementovat `IChangeFeedObserver`. Toto rozhraní obsahuje tři metody:

* `OpenAsync`: Tato funkce je volána, když je otevřen pozorovatel změnu informačního kanálu. Po otevření příjemce/pozorovatel může být upravený tooperform určité akce.  
* `CloseAsync`: Tato funkce je volána, když je ukončen informačního kanálu pozorovatel změnu. Může být upravený tooperform určité akce při zavření příjemce/pozorovatele.  
* `ProcessChangesAsync`: Tato funkce je volána, když jsou k dispozici na změnu kanálu nové změny dokumentu. Může to být upravené tooperform určité akce při každé aktualizaci změnu informačního kanálu.  

V našem příkladu jsme implementovat rozhraní hello `IChangeFeedObserver` prostřednictvím hello `DocumentFeedObserver` třídy. Zde hello `ProcessChangesAsync` funkce upserts (aktualizace) dokumentu z změnu kanálu do hello cílové kolekce. V tomto příkladu je užitečná pro přesun dat z jedné kolekce tooanother v pořadí toochange hello klíč oddílu datové sady. 

*Spuštění hello procesor hostitele*

Před zahájením zpracování událostí, jak změnit možnosti informačního kanálu a změnit možnosti informačního kanálu hostitele je možné upravit. 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval too15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
Hello konkrétních polí, které se dají přizpůsobit jsou shrnuté v následujících tabulkách hello. 

**Změna možností informačního kanálu**:
<table>
    <tr>
        <th>Název vlastnosti</th>
        <th>Popis</th>
    </tr>
    <tr>
        <td>MaxItemCount</td>
        <td>Získá nebo nastaví maximální počet položek toobe, vrátí se v hello výčtu operace v hello služba databáze Azure Cosmos DB hello.</td>
    </tr>
    <tr>
        <td>PartitionKeyRangeId</td>
        <td>Získá nebo nastaví id klíče rozsah hello oddílu pro aktuální požadavek hello v hello Azure Cosmos DB databáze služby.</td>
    </tr>
    <tr>
        <td>RequestContinuation</td>
        <td>Získá nebo nastaví token pokračování požadavku hello v hello Azure Cosmos DB databáze služby.</td>
    </tr>
        <tr>
        <td>SessionToken</td>
        <td>Získá nebo nastaví token hello relace pro použití s konzistence typu relace v hello služba databáze Azure Cosmos DB.</td>
    </tr>
        <tr>
        <td>StartFromBeginning</td>
        <td>Získá nebo nastaví, zda změna kanálu v hello Azure Cosmos DB databáze služby by se měl spustit z hello od (true) nebo aktuální (false). Ve výchozím nastavení spustí z aktuální (false).</td>
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
        <td>Hello interval pro všechny zapůjčení pro oddíly, které jsou aktuálně držené hello ChangeFeedEventHost instance.</td>
    </tr>
    <tr>
        <td>LeaseAcquireInterval</td>
        <td>Časový interval</td>
        <td>interval tookick vypnout toocompute úloh Hello, zda oddíly jsou rovnoměrně rozdělené mezi instancí známé hostitele.</td>
    </tr>
    <tr>
        <td>LeaseExpirationInterval</td>
        <td>Časový interval</td>
        <td>Hello interval, pro které hello zapůjčení pořízené zapůjčení představující oddílu. Pokud během tohoto intervalu neobnovíte hello zapůjčení, vypršela platnost a vlastnictví hello oddílu přesune tooanother ChangeFeedEventHost instance.</td>
    </tr>
    <tr>
        <td>FeedPollDelay</td>
        <td>Časový interval</td>
        <td>Hello zpoždění mezi dotazování oddíl pro nové změny na hello informačního kanálu, až se nečekaně všechny aktuální změny.</td>
    </tr>
    <tr>
        <td>CheckpointFrequency</td>
        <td>CheckpointFrequency</td>
        <td>Hello frekvence toocheckpoint zapůjčení.</td>
    </tr>
    <tr>
        <td>MinPartitionCount</td>
        <td>celá čísla</td>
        <td>počet oddílů minimální Hello hello hostitele.</td>
    </tr>
    <tr>
        <td>MaxPartitionCount</td>
        <td>celá čísla</td>
        <td>může sloužit Hello maximální počet oddílů hello hostitele.</td>
    </tr>
    <tr>
        <td>DiscardExistingLeases</td>
        <td>BOOL</td>
        <td>Jestli na hello spusťte hello hostitele všechny existující zapůjčení, měl by být odstraněn a hello hostitele by měla začít úplně od začátku.</td>
    </tr>
</table>


zpracování událostí toostart doložit `ChangeFeedProcessorHost`, poskytuje hello příslušné parametry pro Azure Cosmos DB kolekce. Potom zavolejte `RegisterObserverAsync` tooregister vaše `IChangeFeedObserver` implementace (DocumentFeedObserver v tomto příkladu) s hello runtime. V tomto okamžiku hello hostitele pokusí tooacquire zapůjčení každých klíče rozsahu oddílu v kolekci Azure Cosmos DB hello použitím "chamtivého" algoritmu. Tato zapůjčení poslední po stanovenou dobu a následně musí být obnovena. Jako nové uzly a instancí pracovního procesu, v takovém případě režimu online, se umístí své rezervace zapůjčení a časem hello zatížení posune mezi uzly, protože každý hostitel pokusí tooacquire další zapůjčení. 

V ukázkovém kódu hello, budeme používat toocreate – třída (DocumentFeedObserverFactory.cs) objektu pro vytváření pozorovatel a hello `RegistObserverFactoryAsync` pozorovatel tooregister hello. 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter toostop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
Postupem času se dosáhne rovnováhy. Tato dynamická funkce umožňuje na základě procesoru automatické škálování toobe použít tooconsumers pro škálování nahoru a dolů. Pokud jsou k dispozici v Azure Cosmos DB s vyšší rychlostí než mohou příjemci zpracovat změny, může být hello zvýšení využití procesoru na spotřebitele toocause použité k automatickému škálování počtu instancí pracovních procesů.

## <a name="next-steps"></a>Další kroky
* Zkuste hello [Azure Cosmos DB změnu kanálu ukázky kódu na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)
* Začínáme s hello kódování [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md) nebo hello [REST API](/rest/api/documentdb/).
