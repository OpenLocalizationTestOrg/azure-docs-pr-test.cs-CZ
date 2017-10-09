---
title: "aaaSQL dotazu metriky pro rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
description: "Informace o způsobu tooinstrument a ladění hello výkon dotazů SQL Azure Cosmos DB požadavků."
keywords: "syntaxe SQL, dotaz sql, sql dotazy, json dotazovací jazyk, databázových koncepcí a sql, agregační funkce"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: b2fa8e8f-7291-45a3-9bd1-7284ed9077f8
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 2fee3786b7d48d254162699471943e316764b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Ladění výkonu dotazů s Azure Cosmos DB
Poskytuje Azure Cosmos DB [SQL rozhraní API pro dotazování na data](documentdb-sql-query.md), aniž byste museli schématu nebo sekundární indexy. Tento článek obsahuje následující informace pro vývojáře hello:

* Nejdůležitější podrobnosti o způsobu fungování spuštění dotazu SQL Azure Cosmos DB
* Informace o hlavičkách žádostí a odpovědí dotazu a možnosti klienta SDK
* Tipy a osvědčené postupy pro výkon dotazů
* Příklady, jak tooutilize SQL provádění statistiky toodebug dotazování výkonu

## <a name="about-sql-query-execution"></a>O spuštění dotazu SQL

V Azure Cosmos DB, ukládat data do kontejnerů, které můžou růst tooany [velikost nebo žádostí o propustnost úložiště](partition-data.md). Azure Cosmos DB bezproblémově škáluje data mezi fyzické oddíly pod nárůst dat toohandle zahrnuje hello nebo zvýšení zřízené propustnosti. Můžete vydat kontejneru tooany dotazy SQL pomocí hello REST API nebo jeden z hello podporované [DocumentDB SDK](documentdb-sdk-dotnet.md).

Stručný přehled vytváření oddílů: definování klíč oddílu, jako je "city", která určuje, jak je rozdělit data do fyzického oddíly. Data patřící tooa jednoho oddílu klíč (například "city" == "Seattle") je uložen v rámci fyzické oddílu, ale obvykle jednoho oddílu fyzického má více klíčů oddílů. V případě oddíl dosáhne velikosti úložiště, hello služba bezproblémově rozdělí hello oddílu na dva nové oddíly a rovnoměrně rozděluje klíč oddílu hello mezi tyto oddíly. Vzhledem k tomu, že oddíly jsou přechodné, hello rozhraní API pomocí abstrakci "oddílu klíče rozsahu", který označuje hello rozsahy hodnoty hash klíče oddílu. 

Pokud vydáte dotazu tooAzure Cosmos DB, hello SDK provádí tyto logických kroků:

* Analyzovat plán spuštění dotazu toodetermine hello SQL dotaz hello. 
* Pokud dotaz hello obsahuje filtr proti hello klíč oddílu, jako například `SELECT * FROM c WHERE c.city = "Seattle"`, jedná směrované tooa jeden oddíl. Pokud dotaz hello nemá filtr na klíč oddílu, pak se spouštějí v všechny oddíly a výsledky sloučení na straně klienta.
* Hello dotazu je provést v rámci každý oddíl v řadě nebo paralelní, v závislosti na konfiguraci klienta. V rámci každý oddíl hello dotaz může si ho nebo další zpátečních cest v závislosti na složitosti hello dotazu, nakonfigurovaná velikost stránky a zřízené propustnosti hello kolekce. Každé spuštění vrátí počet hello [požadované jednotky](request-units.md) spotřebované spuštění dotazu a volitelně statistik provádění dotazů. 
* Hello SDK provede shrnutí výsledků dotazu hello napříč oddíly. Například pokud dotaz hello zahrnuje ORDER BY napříč oddíly, pak výsledky z jednotlivých oddílů jsou výsledky seřadit sloučení tooreturn globálně seřazená. Pokud se dotaz hello agregace jako `COUNT`, počty hello z jednotlivých oddílů se sečtené tooproduce hello celkový počet.

Hello sady SDK poskytují různé možnosti pro spuštění dotazu. Například v rozhraní .NET tyto možnosti jsou dostupné v hello `FeedOptions` třídy. Hello následující tabulka popisuje tyto možnosti a jak budou mít vliv doba provádění dotazu. 

| Možnost | Popis |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Musí být nastavena tootrue pro jakýkoli dotaz, který vyžaduje toobe provést napříč více než jeden oddíl. Jedná se explicitní příznak tooenable jste toomake kompromisy vědomá toho výkonu během doby vývoje. |
| `EnableScanInQuery` | Musí být nastavena tootrue, pokud Nesouhlasili jste s indexování, ale přesto chcete dotaz hello toorun prostřednictvím kontrolu. Pouze použito pouze v případě indexování pro hello požadovaného filtru cesta je zakázána. | 
| `MaxItemCount` | Hello maximální počet položek tooreturn na server toohello odezvy. Nastavením příliš-1, můžete je nechat server hello spravovat hello počet položek. Nebo můžete snížit tooretrieve tato hodnota pouze malý počet položek na dobu odezvy. 
| `MaxBufferedItemCount` | Toto je možnost na straně klienta a použít spotřebu paměti hello toolimit při provádění cross-partition ORDER BY. Vyšší hodnota pomáhá snižovat latenci hello řazení mezi oddílu. |
| `MaxDegreeOfParallelism` | Získá nebo nastaví hello počet souběžných operací spustit na straně klienta během provádění paralelního dotazu v hello databáze služby Azure DocumentDB. Hodnotu vlastnosti kladné omezí hello počet souběžných operací toohello nastavte hodnotu. Pokud je nastaveno tooless než 0, hello systému automaticky rozhoduje hello počet souběžných operací toorun. |
| `PopulateQueryMetrics` | Doba načítání umožní podrobné protokolování statistiky času stráveného v různých fázích provádění dotazu jako čas kompilace, index smyčky čas a dokumentu. Výstup z Statistika dotazu můžete sdílet s problémy s výkonem dotazu toodiagnose podporu Azure. |
| `RequestContinuation` | Provádění dotazů můžete obnovit předáním v token neprůhledného pokračování hello vrácený jakýkoli dotaz. token pro pokračování Hello zapouzdří všechny stavy, které jsou potřebné pro spuštění dotazu. |
| `ResponseContinuationTokenLimitInKb` | Můžete omezit hello maximální velikost token pokračování hello vrácená serverem hello. Může tuto funkci potřebujete tooset Pokud hostitele vaší aplikace má omezení velikosti hlavičky odpovědi. Toto nastavení může zvýšit hello celkové doby trvání a RUs využité pro hello dotazu.  |

Například příklad dotazu podívejme na požadovanou na kolekci s klíčem oddílu `/city` jako hello oddíl klíče a s propustností 100 000 RU/s přiděleným. Požádáte o dotaz pomocí `CreateDocumentQuery<T>` v rozhraní .NET jako hello následující:

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

Dobrý den výše uvedeném fragmentu sady SDK, odpovídá toohello následující požadavku REST API:

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

Každé stránce provádění dotazu odpovídá tooa REST API `POST` s hello `Accept: application/query+json` záhlaví a hello dotazu SQL v textu hello. Každý dotaz je jeden nebo více zaokrouhlit služebních cest toohello server s hello `x-ms-continuation` token opakována mezi tooresume provádění hello klienta a serveru. Možnosti konfigurace Hello v FeedOptions jsou předávány serveru toohello v podobě hello hlaviček požadavku. Například `MaxItemCount` odpovídá příliš`x-ms-max-item-count`. 

Hello požadavku vrátí následující hello (zkrácená čitelnější) odpovědi:

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

hlavičky klíče odpovědi Hello vrácená z dotazu hello zahrnout hello následující:

| Možnost | Popis |
| ------ | ----------- |
| `x-ms-item-count` | Hello počet položek, vrátí se v reakci hello. Toto je závislá na hello zadaný `x-ms-max-item-count`, hello počet položek, které můžete přizpůsobit velikost datové části hello maximální odpovědi, hello zřízené propustnosti a doba provádění dotazu. |  
| `x-ms-continuation:` | Hello pokračování tokenu tooresume spuštění dotazu hello, pokud jsou k dispozici další výsledky. | 
| `x-ms-documentdb-query-metrics` | Hello Statistika dotazu pro provedení hello. Toto je oddělený řetězec obsahující statistiky čas věnovaný hello fáze spuštění dotazu. Vrácené v případě `x-ms-documentdb-populatequerymetrics` je nastaven příliš`True`. | 
| `x-ms-request-charge` | Hello počet [požadované jednotky](request-units.md) spotřebovávají hello dotazu. | 

Podrobnosti o hello hlavičky požadavku REST API a možnosti najdete v tématu [dotaz na prostředky pomocí hello DocumentDB REST API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).

## <a name="best-practices-for-query-performance"></a>Osvědčené postupy pro výkon dotazů
Hello následují hello nejběžnější faktory, které mají vliv na výkon dotazu Azure Cosmos DB. Jsme podrobněji prozkoumat každý z těchto témat v tomto článku.

| Koeficient | Tip | 
| ------ | -----| 
| Zřízená propustnost | Měření RU na jeden dotaz a zajistěte, abyste měli hello požadované zřízené propustnosti pro své dotazy. | 
| Dělení a klíče oddílů | Upřednostnit dotazy s hello hodnotu klíče oddílu v klauzuli filtru hello pro s nízkou latencí. |
| Možnosti sady SDK a dotazů | Doporučené postupy SDK jako přímé připojení a ladit možnosti provedení dotazu na straně klienta. |
| Latence sítě | Účet pro síť režie v měření a použít více funkci tooread rozhraní API z hello nejbližší oblast. |
| Zásady indexování | Ujistěte se, že máte požadované hello cesty nebo zásady indexování pro dotaz hello. |
| Metriky spuštění dotazu | Analýza hello dotazu provádění metriky tooidentify potenciální přepisů dotaz a datové tvarů.  |

### <a name="provisioned-throughput"></a>Zřízená propustnost
V systému Cosmos databáze můžete vytvořit kontejnery dat, každý s vyhrazenou propustností vyjádřené v žádosti o jednotkách (RU) za sekundu. Čtení 1 KB dokumentu je 1 RU a každé operace (včetně dotazů) je normalizovaný tooa pevně daný počet RUs podle jeho složitost. Například pokud máte 1000 zřízení RU/s pro váš kontejner, a máte dotaz jako `SELECT * FROM c WHERE c.city = 'Seattle'` , který využívá 5 RUs a potom můžete provést (1000 RU/s) / (5 RU/dotazu) = 200 dotazu nebo s takové dotazy za sekundu. 

Pokud odešlete více než 200 dotazů za sekundu hello služba spustí omezení rychlosti příchozí požadavky nad 200/s. Hello sady SDK automaticky zpracovávat tento případ provedením omezení rychlosti nebo opakování, proto možná jste si všimli vyšší latence pro tyto dotazy. Zvýšení hello zřízené propustnosti toohello požadované zvyšuje hodnotu dotazu latence a propustnosti. 

toolearn Další informace o jednotek žádosti, najdete v části [požadované jednotky](request-units.md).

### <a name="partitioning-and-partition-keys"></a>Dělení a klíče oddílů
S Azure DB Cosmos obvykle dotazy provádět v hello následující pořadí z nejrychlejší nebo většinu efektivní tooslower nebo méně efektivní. 

* ZÍSKAT klíč jednoho oddílu a klíč položky
* Dotaz s klauzulí filtru pro jeden oddíl klíč
* Dotazování bez klauzule filtru rovnosti nebo rozsah na žádnou vlastnost
* Dotazování bez filtry

Dotazuje této nutné tooconsult všechny oddíly potřeba vyšší latence a může využívat vyšší RUs. Vzhledem k tomu, že každý oddíl má automatické indexování pro všechny vlastnosti, hello dotazu je možné dodávat efektivně z hello indexu v tomto případě. Můžete provést dotazy, které jsou rozmístěny oddíly rychlejší s použitím možností paralelismus hello.

toolearn Další informace o vytváření oddílů a klíče oddílů, najdete v části [vytváření oddílů v Azure Cosmos DB](partition-data.md).

### <a name="sdk-and-query-options"></a>Možnosti sady SDK a dotazů
V tématu [tipy pro zvýšení výkonu](performance-tips.md) a [testování výkonu](performance-testing.md) pro jak tooget hello nejlépe výkonu na straně klienta z Azure Cosmos DB. To zahrnuje použití hello nejnovější SDK, konfigurace specifických pro platformy jako výchozí počet připojení, frekvenci uvolňování paměti, konfiguraci a použití možnosti lightweight připojení jako přímé/TCP. 


#### <a name="max-item-count"></a>Počet položek. maximální počet
Pro dotazy, hello hodnota `MaxItemCount` může mít významný dopad na dobu začátku do konce dotazu. Každý odezvy toohello server vrátí maximálně hello počet položek v `MaxItemCount` (výchozí 100 položek). Nastavení této hodnoty vyšší tooa (-1 je maximální a doporučené), který umožní zvýšení celkové doby trvání dotazu omezením hello počet zpátečních cest mezi serverem a klientem, zejména pro dotazy s velké množství výsledků.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Maximální počet stupně paralelního zpracování
Pro dotazy, ladit hello `MaxDegreeOfParallelism` tooidentify hello doporučené konfigurace pro aplikace, zejména v případě, že můžete provádět dotazy cross-partition (bez filtru na hodnotě hello klíč oddílu). `MaxDegreeOfParallelism`ovládací prvky hello maximální počet paralelních úkolů, tedy hello maximální počet oddílů toobe navštívené paralelně. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

Předpokládejme, že
* D = výchozí maximální počet paralelních úloh (celkový počet procesorů v klientském počítači hello =)
* P = zadán uživatel maximální počet paralelních úloh
* N = počet oddílů, které potřebuje toobe navštívené odpovědí na dotazy

Toto jsou důsledky chování hello paralelní dotazy pro různé hodnoty P.
* (P == 0) = > sériové režimu
* (P == 1) = > maximálně jeden úkol
* (P > 1) = > Min (P, N) paralelní úlohy 
* (P < 1) = > Min (N, D) paralelní úlohy

Poznámky k verzi sady SDK a najdete v části Podrobnosti o implementované třídy a metody [DocumentDB SDK](documentdb-sdk-dotnet.md)

### <a name="network-latency"></a>Latence sítě
V tématu [globální distribuční databázi Cosmos Azure](tutorial-global-distribution-documentdb.md) jak tooset až globální distribuční a připojit toohello nejbližší oblast. Latence sítě nemá významný dopad na výkon dotazů, pokud potřebujete toomake vícenásobný nebo načíst velké sadu výsledků hello dotazu. 

Hello na metriky provádění dotazu se dozvíte, jak tooretrieve hello serveru doba provádění dotazů ( `totalExecutionTimeInMs`), takže můžete rozlišit mezi času stráveného při provádění dotazu a čas strávený v síti přenosu.

### <a name="indexing-policy"></a>Zásady indexování
V tématu [Konfigurace zásady indexování](indexing-policies.md) pro indexování cesty, typy a režimy, a jak by se projevily při provádění dotazu. Ve výchozím hello indexování zásad používá Hash indexování pro řetězce, který je efektivní dotazy na rovnost, ale ne pro rozsah dotazy nebo order by – dotazy. Pokud potřebujete dotazy na rozsah pro řetězce, doporučujeme zadání hello index typu rozsah pro všechny řetězce. 

## <a name="query-execution-metrics"></a>Metriky spuštění dotazu
Můžete získat podrobné metriky pro spuštění dotazu předáním v hello volitelné `x-ms-documentdb-populatequerymetrics` záhlaví (`FeedOptions.PopulateQueryMetrics` v hello .NET SDK). Hodnota vrácená v Hello `x-ms-documentdb-query-metrics` má hello následující páry klíč hodnota určená pro pokročilé řešení problémů s spuštění dotazu. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| Metrika | Jednotka | Popis | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | milisekundy | Doba provádění dotazu | 
| `queryCompileTimeInMs` | milisekundy | Doba kompilace dotazu  | 
| `queryLogicalPlanBuildTimeInMs` | milisekundy | Plán logického dotazu toobuild čas | 
| `queryPhysicalPlanBuildTimeInMs` | milisekundy | Plán fyzické dotazu toobuild čas | 
| `queryOptimizationTimeInMs` | milisekundy | Čas strávený v optimalizaci dotazu | 
| `VMExecutionTimeInMs` | milisekundy | Čas strávený v dotazu runtime | 
| `indexLookupTimeInMs` | milisekundy | Čas strávený v indexu fyzické vrstvě | 
| `documentLoadTimeInMs` | milisekundy | Čas strávený v nahrávání dokumentů  | 
| `systemFunctionExecuteTimeInMs` | milisekundy | Celkový čas strávený provádění (Předdefinované) funkce systému v milisekundách  | 
| `userFunctionExecuteTimeInMs` | milisekundy | Celkový čas strávený spouštění uživatelsky definované funkce v milisekundách | 
| `retrievedDocumentCount` | milisekundy | Celkový počet načtených dokumentů  | 
| `retrievedDocumentSize` | Bajty | Celková velikost načtené dokumenty v bajtech  | 
| `outputDocumentCount` | Počet | Počet výstupních dokumentů | 
| `writeOutputTimeInMs` | milisekundy | Doba provádění dotazu v milisekundách | 
| `indexUtilizationRatio` | poměr (< = 1) | Načíst poměr počtu dokumenty odpovídající hello filtru toohello number dokumentů  | 

Hello klienta SDK může interně být více dotaz operations tooserve hello dotazu v rámci každého oddílu. Hello klient zadává víc než jedno volání oddílů pokud celkový počet výsledků hello překročí `x-ms-max-item-count`, pokud dotaz hello překračuje hello zřízená propustnost pro hello oddílu, nebo pokud hello datovou část dotazu dosáhne hello maximální velikosti na stránce nebo pokud hello dotazu dosáhne hello systém přidělené časový limit. Každé spuštění částečné dotaz vrátí `x-ms-documentdb-query-metrics` pro tuto stránku. 

Tady jsou některé ukázkové dotazy a jak toointerpret některé hello metrik vrácená z dotazu spuštění: 

| Dotaz | Ukázka metrika | Popis | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | Hello počet dokumentů, načíst je 100 + 1 klauzule TOP toomatch hello. Doba dotazu je většinou věnovaný `WriteOutputTime` a `DocumentLoadTime` vzhledem k tomu, že je kontrolu. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | RetrievedDocumentCount je nyní vyšší (500 + 1 toomatch hello klauzule TOP). | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | O 0.9 ms je věnovaný IndexLookupTime pro vyhledávání klíčů, protože je indexu vyhledávání `/N/?`. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Něco delší dobu (1.7 ms) věnovaný IndexLookupTime přes rozsah kontroly, protože je indexu vyhledávání `/N/?`. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | Stejný čas strávený `DocumentLoadTime` jako předchozí dotazy, ale nižší `WriteOutputTime` protože jsme se projekce pouze jednu vlastnost. | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | O 213 ms je věnovaný `UserDefinedFunctionExecutionTime` provádění hello UDF na každou hodnotu `c.N`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Je věnovaný přibližně 0,6 ms `IndexLookupTime` na `/Name/?`. Většina hello dotaz. čas provedení (ms ~ 7) v `SystemFunctionExecutionTime`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Dotaz se provádí jako kontrolu, protože používá `LOWER`, a jsou vráceny 500 mimo 2491 načtené dokumenty. |


## <a name="next-steps"></a>Další kroky
* toolearn o operátory dotazu SQL hello podporována a klíčová slova, najdete v části [dotazu SQL](documentdb-sql-query.md). 
* toolearn o jednotkách žádosti, najdete v části [požadované jednotky](request-units.md).
* toolearn o zásady indexování, najdete v části [indexování zásad](indexing-policies.md) 


