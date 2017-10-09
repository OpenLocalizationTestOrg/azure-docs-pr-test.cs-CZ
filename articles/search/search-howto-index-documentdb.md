---
title: "aaaIndexing zdroj dat Cosmos DB pro službu Azure Search | Microsoft Docs"
description: "Tento článek ukazuje, jak toocreate indexer Azure Search s DB Cosmos jako zdroj dat."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 08/10/2017
ms.author: eugenesh
ms.openlocfilehash: 195c9bc026ee1591679dc425ef083a32a3c86be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Připojování Cosmos databáze s Azure Search pomocí indexerů

Pokud chcete tooimplement prostředí skvělé vyhledávání přes Cosmos DB data, můžete Azure Search indexer toopull data do indexu Azure Search. V tomto článku jsme ukazují, jak toointegrate DB Cosmos Azure s Azure Search bez nutnosti toowrite jakékoliv infrastruktury indexování toomaintain kódu.

tooset až indexer Cosmos DB, musíte mít [služby Azure Search](search-create-service-portal.md)a vytvořte index, zdroj dat a nakonec hello indexer. Můžete vytvořit tyto objekty pomocí hello [portál](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), nebo [REST API](/rest/api/searchservice/) pro všechny jazyky,-rozhraní .NET. 

Pokud se přihlásíte hello portálu, hello [Průvodce importem dat](search-import-data-portal.md) provede vás procesem vytvoření hello těchto prostředků.

> [!NOTE]
> Cosmos DB je hello nové generace DocumentDB. I když se změnil hello název produktu, syntaxe je hello stejné jako předtím. Pokračovat v toospecify `documentdb` podle pokynů v tomto článku indexer. 

> [!TIP]
> Můžete spustit hello **importovat data** z hello Cosmos DB řídicí panel toosimplify indexování pro tento zdroj dat průvodce. V levém navigačním panelu přejděte příliš**kolekce** > **přidat Azure Search** tooget spuštěna.

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Koncepty indexer Azure Search
Azure Search podporuje hello vytváření a Správa zdrojů dat (včetně Cosmos DB) a indexery, které pracují těchto zdrojů.

A **zdroj dat** určuje hello data tooindex, pověření a zásady pro identifikaci změny v datech hello (jako jsou dokumenty modified nebo deleted uvnitř vaší kolekce). zdroj dat Hello je definován jako nezávislý prostředek, aby se můžete použít několik indexerů.

**Indexer** popisuje, jak hello data proudí ze zdroje dat do indexu vyhledávání cíl. Indexer umožňuje:

* Proveďte jednorázové kopii dat toopopulate hello indexu.
* Synchronizujte indexu se změnami ve zdroji dat hello podle plánu. plán Hello je součástí definice indexer hello.
* Vyvolání aktualizace na vyžádání tooan index podle potřeby.

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Krok 1: Vytvořte zdroj dat.
toocreate zdroje dat, proveďte POST:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

Hello textu hello požadavku obsahuje definice zdroje dat hello, která by měla obsahovat hello následující pole:

* **název**: Zvolte jakékoli toorepresent název databáze Cosmos DB.
* **typ**: musí být `documentdb`.
* **přihlašovací údaje**:
  
  * **connectionString**: vyžaduje. Zadejte hello připojení informace tooyour Azure Cosmos DB databázi v hello následující formát:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kontejner**:
  
  * **název**: vyžaduje. Zadejte id hello hello Cosmos DB kolekce toobe indexovat.
  * **dotaz**: volitelné. Zadaný dotaz tooflatten libovolný dokument JSON do plochých schéma, které mohou indexu Azure Search.
* **dataChangeDetectionPolicy**: doporučené. V tématu [indexování dokumentů změnit](#DataChangeDetectionPolicy) části.
* **dataDeletionDetectionPolicy**: volitelné. V tématu [indexování dokumentů odstranit](#DataDeletionDetectionPolicy) části.

### <a name="using-queries-tooshape-indexed-data"></a>Pomocí dotazů tooshape indexované dat
Můžete zadat tooflatten dotazu Cosmos DB vnořené vlastnosti nebo pole, projektu vlastnostech formátu JSON a filtrovat toobe data hello indexované. 

Příklad dokumentu:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Dotaz filtru:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Vyrovnání dotazu:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projekce dotazu:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Pole sloučení dotaz:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Krok 2: Vytvoření indexu
Pokud již nemáte, vytvořte cílový index Azure Search. Můžete vytvořit index pomocí hello [portál Azure uživatelského rozhraní](search-create-index-portal.md), hello [vytvořit Index REST API](/rest/api/searchservice/create-index) nebo [Index – třída](/dotnet/api/microsoft.azure.search.models.index).

Hello následující ukázka vytvoří index s na id a popis pole:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Ujistěte se, že hello schéma cílový index je kompatibilní s hello schéma dokumentů JSON zdroj hello nebo výstup hello projekce vašeho vlastního dotazu.

> [!NOTE]
> Pro dělené kolekce je klíč dokumentu výchozí hello Cosmos DB `_rid` vlastností, která se získá přejmenovat příliš`rid` ve službě Azure Search. Navíc Cosmos DB na `_rid` hodnoty obsahovat znaky, které jsou v Azure Search klíče neplatné. Z tohoto důvodu hello `_rid` hodnoty jsou kódováním Base64.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datové typy a typy dat vyhledávání systému Azure
| JSON DATOVÉHO TYPU | TYPY POLÍ KOMPATIBILNÍ CÍLOVÝ INDEX |
| --- | --- |
| BOOL |Edm.Boolean Edm.String |
| Čísla, která vypadat podobně jako celá čísla |Edm.String Edm.Int32, Edm.Int64, |
| Čísla této vypadají plovoucí body |Edm.Double Edm.String |
| Řetězec |Edm.String |
| Pole jednoduchých typů, například ["a", "b", "c"] |Collection(Edm.String) |
| Řetězce, které vypadají podobně jako kalendářní data |Edm.DateTimeOffset Edm.String |
| GeoJSON objekty, například {"typ": "Místo", "coordinates": [dlouhý a lat]} |Edm.GeographyPoint |
| Jiné objekty JSON |Není k dispozici |

<a name="CreateIndexer"></a>
## <a name="step-3-create-an-indexer"></a>Krok 3: Vytvořením indexeru.

Po vytvoření hello index a zdroj dat jste připravené toocreate hello indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Indexer spouští každé dvě hodiny (interval plánování je nastavena příliš "PT2H"). toorun indexer každých 30 minut nastavit hello interval příliš "PT30M". Hello nejkratší podporovaný interval je 5 minut. Hello plán je volitelné - li tento parametr vynechán, indexer se spustí pouze po jeho vytvoření. Indexer na vyžádání je však můžete spustit kdykoli.   

Další informace o hello vytvoření rozhraní API Indexer, podívejte se na [vytvořit Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Spuštění indexeru na vyžádání
V přidání toorunning pravidelně podle plánu lze indexer také vyvolat na vyžádání:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Při spuštění rozhraní API vrátí úspěšně, bylo naplánováno hello indexer volání, ale vlastní zpracování hello probíhá asynchronně. 

Můžete sledovat stav indexer hello v hello portálu nebo pomocí hello získat Indexer stav rozhraní API, které jsme popisují další. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Získání stavu indexeru
Můžete načíst hello stavu a provádění historii indexer:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Hello odpovědi obsahuje celkový stav indexer, hello indexer poslední (nebo probíhající) volání a hello historii poslední indexer volání.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Historie provádění obsahuje toohello 50 poslední dokončené spuštěních, které jsou seřazeny v opačném chronologickém pořadí (takže hello nejnovější spuštění bude první v odpovědi hello).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indexování změněné dokumenty
účel Hello dat. změnit zásady detekce je tooefficiently identifikaci položek změněná data. V současné době hello podporovány pouze zásady je hello `High Water Mark` zásady pomocí hello `_ts` vlastnost (časové razítko) poskytované Cosmos databáze, které se určuje takto:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Pomocí této zásady je důrazně doporučujeme tooensure indexer dobrý výkon. 

Pokud používáte vlastní dotaz, ujistěte se, že hello `_ts` vlastnost se promítá hello dotazu.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Přírůstkové průběh a vlastní dotazy
Přírůstkové průběh během indexování zajistí, že pokud indexer provádění přeruší přechodných chyb nebo provádění časový limit, hello indexer můžete vyzvedávat kde bylo přerušeno příště se spustí, místo nutnosti toore index hello celou kolekci od začátku. To je zvlášť důležité při indexování rozsáhlých kolekcí. 

přírůstkové průběh tooenable při použití vlastního dotazu, ujistěte se, že váš dotaz objednávky hello výsledky podle hello `_ts` sloupce. To umožňuje pravidelné kontroly ukazující, že Azure Search používá tooprovide přírůstkové průběh v hello přítomnost chyb.   

V některých případech, i pokud dotaz obsahuje `ORDER BY [collection alias]._ts` klauzuli vyhledávání systému Azure nemusí odvození takový dotaz hello je seřazené podle hello `_ts`. Se dá zjistit Azure Search, výsledky jsou seřazené podle pomocí hello `assumeOrderByHighWaterMarkColumn` vlastnosti konfigurace. toospecify Tento pomocný vytvořit nebo aktualizovat indexer následujícím způsobem: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indexování odstranit dokumenty
Při odstranění řádků z kolekce hello obvykle chcete toodelete z indexu vyhledávání hello také řádky. účelem Hello zásady detekce odstranění dat je tooefficiently identifikaci položek odstraněná data. V současné době hello podporovány pouze zásady je hello `Soft Delete` zásad (odstranění označeno příznak nějaká), která je určena následujícím způsobem:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "hello value that identifies a document as deleted"
    }

Pokud používáte vlastní dotaz, ujistěte se, že hello vlastnost odkazuje `softDeleteColumnName` se promítá hello dotazu.

Hello následující ukázka vytvoří zdroj dat s zásadu obnovitelného odstranění:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Další kroky
Blahopřejeme! Jste se naučili, jak toointegrate Azure Cosmos DB s Azure Search pomocí hello indexeru pro Cosmos DB.

* toolearn jak informace o databázi Cosmos Azure, najdete v části hello [stránku služby Cosmos DB](https://azure.microsoft.com/services/documentdb/).
* toolearn jak informace o službě Azure Search najdete v části hello [stránku služby vyhledávání](https://azure.microsoft.com/services/search/).
