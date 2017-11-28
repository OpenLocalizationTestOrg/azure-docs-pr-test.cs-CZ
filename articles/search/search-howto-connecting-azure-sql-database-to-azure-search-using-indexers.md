---
title: "tooAzure aaaConnecting databáze SQL Azure Search pomocí indexerů | Microsoft Docs"
description: "Zjistěte, jak index toopull dat z Azure SQL Database tooan Azure Search pomocí indexerů."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/13/2017
ms.author: eugenesh
ms.openlocfilehash: b28a11cf18ef994de99e09af90bbfeb171ef3cde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-sql-database-tooazure-search-using-indexers"></a>Připojení Azure SQL Database tooAzure vyhledávání pomocí indexery

Předtím, než se můžete dotazovat [indexu Azure Search](search-what-is-an-index.md), musí jeho naplnění vaše data. Pokud hello data žije v Azure SQL database, **indexer Azure Search pro databázi SQL Azure** (nebo **indexer Azure SQL** pro zkrácení) můžete automatizovat proces indexování hello, to znamená méně kódu toowrite a méně Infrastruktura toocare o.

Tento článek se zabývá hello mechanismů pomocí [indexery](search-indexer-overview.md), ale také popisuje funkce, které jsou k dispozici jenom s databází Azure SQL (například integrované sledování změn). 

V databázích SQL tooAzure přidání, poskytuje Azure Search indexery pro [Azure Cosmos DB](search-howto-index-documentdb.md), [úložiště objektů Azure Blob](search-howto-indexing-azure-blob-storage.md), a [úložiště tabulek Azure](search-howto-indexing-azure-tables.md). toorequest podpory pro další datové zdroje, zadejte svůj názor na hello [fóru pro zpětnou vazbu Azure Search](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexery a zdroje dat.

A **zdroj dat** Určuje, které tooindex dat, přihlašovací údaje pro přístup k datům a zásady, které efektivně identifikovat změny v datech hello (nové, modified nebo deleted řádky). Je definována jako prostředek nezávisle tak, aby ji můžete použít několik indexerů.

**Indexer** je na prostředek, který připojí jeden zdroj dat s indexem cílové vyhledávání. Indexer se používá v hello následující způsoby:

* Proveďte jednorázové kopii dat toopopulate hello indexu.
* Aktualizujte index změnami ve zdroji dat hello podle plánu.
* Spusťte na vyžádání tooupdate index podle potřeby.

Jeden indexeru můžete využívat pouze jednu tabulku nebo zobrazení, ale můžete vytvořit několik indexerů, pokud chcete, aby toopopulate více indexů vyhledávání. Další informace o konceptech najdete v tématu [Indexer Operations: obvyklý pracovní postup](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

Můžete nastavit a nakonfigurovat indexer Azure SQL pomocí:

* Průvodce importem dat v hello [portálu Azure](https://portal.azure.com)
* Služba Azure Search [sady .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Služba Azure Search [rozhraní REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/indexer-operations)

V tomto článku, použijeme hello REST API toocreate **indexery** a **zdroje dat**.

## <a name="when-toouse-azure-sql-indexer"></a>Když toouse Azure SQL Indexer
V závislosti na několika různými faktory týkající se dat tooyour hello použití indexer Azure SQL může nebo nemusí být vhodné. Pokud se vaše data vejde hello následující požadavky, můžete použít indexer Azure SQL.

| Kritéria | Podrobnosti |
|----------|---------|
| Data pocházejí z jedné tabulky nebo zobrazení | Pokud hello data je rozmístěny na několika tabulkám, můžete vytvořit jediné zobrazení dat hello. Ale pokud používáte zobrazení, nebudete moct toouse SQL Server integrovaného změnu detekce toorefresh index s přírůstkové změny. Další informace najdete v tématu [zaznamenávání změnit a odstranit řádky](#CaptureChangedRows) níže. |
| Datové typy jsou kompatibilní | Většina, ale ne všechny typy hello SQL jsou podporovány v indexu Azure Search. Seznam najdete v tématu [mapování datové typy](#TypeMapping). |
| Synchronizace dat v reálném čase není vyžadováno | Indexer znovu mohou indexu tabulku maximálně každých pět minut. Pokud vaše změny dat často a hello změní nutné toobe promítnuta hello index během několika sekund nebo minut jeden, doporučujeme používat hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) nebo [.NET SDK](search-import-data-dotnet.md) toopush přímo aktualizovat řádky. |
| Přírůstkové indexování je možné | Pokud máte velké sady dat a indexeru hello toorun plán podle plánu, Azure Search musí být schopný tooefficiently identifikovat nové, změněné nebo odstraněné řádků. Bez přírůstkové indexování je povoleno pouze pokud jste indexu na vyžádání (ne podle plánu), nebo indexování méně než 100 000 řádků. Další informace najdete v tématu [zaznamenávání změnit a odstranit řádky](#CaptureChangedRows) níže. |

## <a name="create-an-azure-sql-indexer"></a>Vytvořením indexeru. Azure SQL

1. Vytvoření zdroje dat hello:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of hello table or view that you want tooindex" }
    }
   ```

   Můžete získat připojovací řetězec hello hello [portál Azure](https://portal.azure.com); použít hello `ADO.NET connection string` možnost.

2. Pokud již nemáte, vytvořte index Azure Search cíl hello. Můžete vytvořit index pomocí hello [portál](https://portal.azure.com) nebo hello [vytvořit Index rozhraní API](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Ujistěte se, že hello schéma cílový index je kompatibilní s hello schématu hello zdrojové tabulky – viz [mapování mezi SQL a služba Azure search datové typy](#TypeMapping).

3. Vytvořte hello indexer tak, že ho pojmenujete a odkazování na zdrojové a cílové index datových hello:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Indexer vytvořené v tomto případě nemá plán. Automaticky spustí, až když je vytvořena. Můžete ho spustit znovu v současně pomocí **spustit indexer** žádost:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

Můžete přizpůsobit několik aspektů indexer chování, například velikost dávky a kolik dokumentů mohou být přeskočeny, než se nezdaří spuštění indexeru. Další informace najdete v tématu [vytvoření rozhraní API Indexer](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Může být nutné tooallow služby Azure tooconnect tooyour databáze. V tématu [připojení z Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) pokyny toodo který.

toomonitor hello indexer stav a historie provádění (počet položek indexed, selhání atd.), použijte **indexer stav** žádost:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

Hello odpověď by měla vypadat podobně jako toohello následující:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Historie provádění obsahuje too50 spuštěních hello naposledy byla dokončena, které jsou seřazeny v opačném chronologickém pořadí hello (tak, aby poslední spuštění hello bude první v odpovědi hello).
Další informace o hello odpovědi můžete najít v [získání stavu indexeru](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexery spouštět podle plánu
Můžete také uspořádat hello indexer toorun pravidelně podle plánu. toodo, přidejte hello **plán** vlastnost při vytváření nebo aktualizaci hello indexer. Následující příklad Hello ukazuje PUT požadavek tooupdate hello indexer:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Hello **interval** parametr je povinný. Hello interval odkazuje toohello čas mezi hello začátek spuštěních dvě po sobě jdoucích indexer. Hello Nejmenší povolený interval je 5 minut; Hello nejdelší je jeden den. Musí být naformátovaná jako hodnota "hodnoty doby podle" XSD (omezená podmnožina [ISO 8601 trvání](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) hodnotu). vzor Hello: `P(nD)(T(nH)(nM))`. Příklady: `PT15M` pro každých 15 minut, `PT2H` pro každé 2 hodiny.

Hello volitelné **startTime** Určuje, kdy hello naplánované spuštěních by měla být zahájena. Pokud je vynechaný, použije se aktuální čas UTC hello. Tento čas může být v hello po – v takovém případě hello první spuštění je naplánováno, jako kdyby hello indexeru je spuštěn nepřetržitě od hello startTime.  

Najednou můžete spustit pouze jeden spuštění indexeru. Pokud indexeru je spuštěn, když její spuštění je naplánováno, provádění hello posunut až hello dalším naplánovaném čase.

Pojďme se podívat na příklad toomake to konkrétnější. Předpokládejme, že jsme hello následující hodinový plán nakonfigurovat:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Stane se toto:

1. první spuštění indexeru Hello spustí na nebo přibližně 1. března 2015 12:00 v noci ČAS UTC.
2. Předpokládejme, že spuštění tohoto trvá 20 minut (nebo kdykoli menší než 1 hodina).
3. druhý provádění Hello spustí na nebo přibližně 1. března 2015 1:00 ráno
4. Nyní předpokládejme, že spuštění tohoto trvá víc než jednu hodinu – například 70 minut – tak, aby jeho dokončení přibližně 2:10:00
5. Je teď 2:00:00, čas pro spuštění toostart třetí hello. Ale protože hello druhý provádění z ráno 1 je stále spuštěna, provedení třetí hello je přeskočeno. třetí provádění Hello spustí ve 3 hodiny.

Můžete přidat, změnit nebo odstranit plán pro existujícího indexeru pomocí **PUT indexer** požadavku.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Zachycení nové, změny a odstranění řádků

Služba Azure Search používá **přírůstkové indexování** tooavoid s toore index hello celou tabulku nebo zobrazení pokaždé, když spustí indexer. Vyhledávání systému Azure nabízí že dva změnit detekce zásady toosupport přírůstkové indexování. 

### <a name="sql-integrated-change-tracking-policy"></a>Zásady sledování změn s integrací SQL
Pokud vaše databáze SQL podporuje [sledování změn](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), doporučujeme používat **SQL integrované změnit způsob sledování**. Toto je zásada nejúčinnější hello. Kromě toho umožňuje Azure Search tooidentify odstranit řádky bez nutnosti tooadd tabulkou tooyour sloupec aplikace explicitní "obnovitelného odstranění".

#### <a name="requirements"></a>Požadavky 

+ Požadavky na databázi verze:
  * SQL Server 2012 SP3 nebo novější, pokud používáte systém SQL Server na virtuálních počítačích Azure.
  * Azure SQL Database verze 12, pokud používáte Azure SQL Database.
+ Pouze tabulky (žádná zobrazení). 
+ V databázi hello [povolit sledování změn](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) pro tabulku hello. 
+ Žádný složené primární klíč (primární klíč obsahující více než jeden sloupec) v tabulce hello.  

#### <a name="usage"></a>Využití

toouse tato zásada umožňuje vytvořit nebo aktualizovat zdroj dat takto:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Při použití zásady, sledování změn SQL integrované nezadávejte zásadami detekce odstraňování oddělení dat – tato zásada má integrovanou podporu pro identifikaci odstranit řádky. Však pro hello odstranění toobe zjistil "automagically" hello klíč dokumentu v indexu vyhledávání musí být hello stejné jako primární klíč hello v hello tabulky SQL. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Zásady detekce změn horní meze

Tyto zásady detekce změn spoléhá na sloupec "horní mez" zaznamenávání hello verze nebo čas poslední aktualizace řádek. Pokud používáte zobrazení, musíte použít zásadu horní mez. sloupec horní mez Hello musí splňovat následující požadavky hello.

#### <a name="requirements"></a>Požadavky 

* Vloží všechny zadejte hodnotu pro sloupec hello.
* Všechny položky tooan aktualizace také změnit hodnotu hello hello sloupce.
* Hello hodnotu v tomto sloupci se zvyšuje s každou insert nebo update.
* Dotazy s hello následující WHERE a klauzule ORDER BY se dají efektivně provádět:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Důrazně doporučujeme používat hello [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) datový typ pro sloupec hello horní mez. Pokud se používá jiný typ dat sledování změn není zaručena toocapture všechny změny v hello přítomnost transakce provádění souběžně dotaz indexer. Při použití **rowversion** v konfiguraci s replikami jen pro čtení, musí odkazovat na primární replice hello hello indexer. Scénáře synchronizace dat lze použít pouze primární repliku.

#### <a name="usage"></a>Využití

toouse zásadu horní mez vytvořit nebo aktualizovat zdroj dat takto:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Pokud hello zdrojová tabulka nemá index na sloupci hello horní mez, dotazy, které používá hello SQL indexer vypršet časový limit. Konkrétně hello `ORDER BY [High Water Mark Column]` klauzule vyžaduje indexu toorun efektivně Pokud hello tabulka obsahuje mnoho řádků.
>
>

Pokud dojde k vypršení časového limitu, můžete použít hello `queryTimeout` indexer konfigurace nastavení tooset hello hodnota časového limitu dotazu tooa vyšší než hello výchozí hodnota 5 minut časového limitu. Například tooset hello časový limit too10 minut, vytvořit nebo aktualizovat hello indexer s hello následující konfigurace:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Můžete také zakázat hello `ORDER BY [High Water Mark Column]` klauzule. To však není doporučeno, protože pokud se spuštění indexeru hello přerušený kvůli chybě, hello indexer má toore proces všechny řádky Pokud běží novější - i v případě indexeru hello má již zpracovává téměř všechny řádky hello hello, když byla přerušena. toodisable hello `ORDER BY` klauzule, použijte hello `disableOrderByHighWaterMarkColumn` nastavení v definici indexer hello:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Softwarové zásady odstranit detekce odstranění sloupce
Při odstranění řádků z hello zdrojové tabulky, budete ho zřejmě chtít toodelete z indexu vyhledávání hello také řádky. Pokud používáte integrované hello SQL zásady sledování změn, to se stará za vás. Ale zásady sledování změn horní meze hello není vám pomůžou s odstraněných řádků. Jaké toodo?

Pokud se fyzicky odebrání hello řádků z tabulky hello Azure Search nemá žádný způsob, jak tooinfer hello přítomnosti záznamy, které už existují.  Můžete však použít hello "soft odstranění" technika toologically odstranění řádků bez odstranění z tabulky hello. Přidat sloupec tooyour tabulku nebo zobrazení a označit řádky jako odstranit pomocí sloupce.

Pokud používáte hello konfigurace soft odstranění techniku, můžete zadat hello obnovitelného odstranění zásadu takto při vytváření nebo aktualizaci hello zdroj dat:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[hello value that indicates that a row is deleted]"
        }
    }

Hello **softDeleteMarkerValue** musí být řetězec – použijte hello řetězcovou reprezentaci skutečnou hodnotu. Například pokud máte sloupec celé číslo, které jsou označené odstraněných řádků hello hodnota 1, použijte `"1"`. Pokud máte BIT sloupce, které jsou označené odstraněných řádků hello logickou hodnotu true, použijte `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>Mapování mezi datové typy SQL a Azure Search
| Datový typ SQL. | Cílový index povolené typy polí | Poznámky |
| --- | --- | --- |
| Bit |Edm.Boolean Edm.String | |
| int, smallint, tinyint |Edm.String Edm.Int32, Edm.Int64, | |
| bigint |Edm.Int64 Edm.String | |
| skutečné, float |Edm.Double Edm.String | |
| Smallmoney peníze desítková číslice |Edm.String |Vyhledávání systému Azure nepodporuje převod decimal typy do Edm.Double, protože by to ztratit přesnost |
| Char, nchar, varchar, nvarchar |Edm.String<br/>Collection(Edm.String) |Řetězec SQL může být použité toopopulate Collection(Edm.String) pole, pokud řetězec hello představuje pole JSON řetězců:`["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset Edm.String | |
| uniqueidentifer |Edm.String | |
| Geography |Edm.GeographyPoint |Jsou podporovány pouze geography instance typu bodu s SRID 4326 (což je výchozí hello) |
| ROWVERSION |Není k dispozici |Verze řádku sloupce nelze uložit do indexu vyhledávání hello, ale mohou být použity pro sledování změn |
| Doba, časový interval, binary, varbinary, image, xml, geometry, typy CLR |Není k dispozici |Nepodporuje se |

## <a name="configuration-settings"></a>Nastavení konfigurace
SQL indexer zpřístupňuje několik nastavení konfigurace:

| Nastavení | Datový typ | Účel | Výchozí hodnota |
| --- | --- | --- | --- |
| queryTimeout |Řetězec |Nastaví hello časový limit pro spuštění dotazu SQL |5 minut ("00: 05:00") |
| disableOrderByHighWaterMarkColumn |BOOL |Způsobí, že používané hello horní mez zásad tooomit hello klauzule ORDER by dotazu SQL hello. V tématu [zásad horní mez](#HighWaterMarkPolicy) |False |

Toto nastavení se použije v hello `parameters.configuration` objektu v definici indexer hello. Například tooset hello dotazu časový limit too10 minut vytvořit nebo aktualizovat hello indexer s hello následující konfigurace:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>Nejčastější dotazy

**Otázka: je možné použít indexer Azure SQL s databází SQL spuštěn na virtuální počítače IaaS v Azure?**

Ano. Ale musíte tooallow databázi tooyour tooconnect vaší vyhledávací služby. Další informace najdete v tématu [nakonfigurovat připojení ze Azure Search indexer tooSQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**Otázka: je možné použít indexer Azure SQL s databází SQL místní aplikace?**

Ne přímo. Není doporučujeme nebo podporují přímé připojení, jak to proto by vyžadovaly jste tooopen provozu tooInternet databáze. Zákazníci proběhlo úspěšně tento scénář použití most technologií, jako je Azure Data Factory. Další informace najdete v tématu [nabízené data tooan indexu Azure Search pomocí Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**Otázka: je možné použít indexer Azure SQL s databázemi než SQL Server spuštěnou v IaaS v Azure?**

Ne. Tento scénář nepodporujeme, protože nebyly testovány indexer hello se žádné databáze než SQL Server.  

**Otázka: je možné vytvořit několik indexerů spuštěna podle plánu?**

Ano. Ale pouze jeden indexer může být spuštěn v jednom uzlu najednou. Pokud budete potřebovat k současnému spuštění více indexery, vezměte v úvahu vertikálním navýšení kapacity vaší vyhledávací služby toomore než jedna jednotka vyhledávání.

**Otázka: funguje s vliv indexer pracovním vytížením můj dotaz?**

Ano. Spuštění indexeru na jednom z uzlů hello ve službě vyhledávání a tento uzel prostředky jsou sdílené mezi indexování a obsluhuje přenosy dotazu a další žádosti o rozhraní API. Pokud spustíte zatížení s intenzivním indexování a dotazů a dojde k vysokou míru 503 chyby nebo odezvy roste, vezměte v úvahu [škálování služby vyhledávání](search-capacity-planning.md).

**Otázka: je možné používat sekundární repliku v [clusteru převzetí služeb při selhání](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) jako zdroj dat?**

To záleží na okolnostech. Pro úplnou indexování tabulku nebo zobrazení, můžete použít sekundární repliku. 

Pro přírůstkové indexování změn podporuje Azure Search, dvě zásady detekce: SQL integrované změnit sledováním a horní mez.

U replik, jen pro čtení databáze SQL nepodporuje sledování integrované změn. Proto je nutné použít zásady horní mez. 

Naše standardní doporučení je toouse hello rowversion datový typ pro sloupec hello horní mez. Pomocí rowversion však spoléhá na SQL Database `MIN_ACTIVE_ROWVERSION` funkci, která není podporována u replik, jen pro čtení. Pokud používáte rowversion, proto musí odkazovat hello indexer tooa primární repliky.

Když zkusíte toouse rowversion na repliku jen pro čtení, zobrazí se následující chyba hello: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update hello datasource and specify a connection toohello primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**Otázka: je možné použít jako alternativní, bez rowversion sloupec pro sledování změn horní meze?**

Není doporučeno. Pouze **rowversion** umožňuje pro synchronizaci dat spolehlivé. Nicméně, v závislosti na logice aplikace může být bezpečné pokud:

+ Můžete zajistit, že při spuštění indexeru hello, že nejsou žádné nevyřízené transakce hello tabulky, který je právě indexovaný (například všechny aktualizace tabulky provedena, dávky podle plánu, a plán indexeru hello Azure Search je nastaven tooavoid překrývající se s tabulkou hello plán aktualizace).  

+ Pravidelně provedením úplné nové indexování toopick až zmeškaných řádky. 
