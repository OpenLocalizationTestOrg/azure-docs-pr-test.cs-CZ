---
title: "Indexování zdroji dat rozhraní API služby Azure Cosmos databáze SQL pro službu Azure Search | Microsoft Docs"
description: "Tento článek ukazuje, jak vytvořit indexer Azure Search s datovým zdrojem Azure Cosmos databáze (SQL API)."
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
ms.date: 01/08/2018
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: e449f13adcd1a3651e1cac852b23f21d0227038a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Připojování Cosmos databáze s Azure Search pomocí indexerů

[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuované a více modelech databáze společnosti Microsoft. S jeho [rozhraní SQL API](../cosmos-db/sql-api-introduction.md), Azure Cosmos DB poskytuje bohatý a známých možnosti dotazu SQL s trvale nízké latence přes data JSON bez schématu. Vyhledávání systému Azure se bezproblémově integruje s rozhraní SQL API. Dokumenty JSON může vyžádat přímo do indexu Azure Search pomocí [indexer Azure Search](search-indexer-overview.md), je navržený speciálně pro rozhraní API pro Azure Cosmos databáze SQL. 

V tomto článku se dozvíte, jak:

> [!div class="checklist"]
> * Nakonfigurujte použít jako zdroj dat databázi rozhraní API pro Azure Cosmos databáze SQL Azure Search. Volitelně můžete zadejte dotaz vybrat podmnožinu.
> * Vytvořte index vyhledávání s JSON kompatibilní datové typy.
> * Nakonfigurujte indexer na vyžádání a opakovaného indexování.
> * Přírůstkově aktualizujte index na základě změn v základních datech.

> [!NOTE]
> Rozhraní API SQL DB Cosmos Azure je nová generace DocumentDB. I když se změnil název produktu, `documentdb` syntaxe ve službě Azure Search indexery stále existuje pro zpětné kompatibility v Azure rozhraní API pro vyhledávání a stránky portálu. Při konfiguraci indexery, nezapomeňte zadat `documentdb` syntaxe podle pokynů v tomto článku.

<a name="supportedAPIs"></a>

## <a name="supported-api-types"></a>Podporované typy rozhraní API

I když Azure Cosmos DB podporuje různé datové modely a rozhraní API, podpora indexeru rozšiřuje rozhraní SQL API. 

Podpora pro další rozhraní API je chystaný. Ke stanovení priorit ty, které chcete podporovat nejprve přetypujte na webu hlas uživatele:

* [Podpora zdroje dat tabulky rozhraní API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Podpora zdroje dat rozhraní Graph API](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [Podpora zdroje dat MongoDB rozhraní API](https://feedback.azure.com/forums/263029-azure-search/suggestions/18861421-documentdb-indexer-should-be-able-to-index-mongodb)
* [Rozhraní API Cassandra Apache podpora zdroje dat](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Požadavky

Chcete-li nastavit indexer Azure Cosmos DB, musíte mít [služby Azure Search](search-create-service-portal.md)a vytvořit index, zdroj dat a nakonec indexeru. Můžete vytvořit tyto objekty pomocí [portál](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), nebo [REST API](/rest/api/searchservice/) pro všechny jazyky,-rozhraní .NET. 

Pokud se přihlásíte pro tento portál [Průvodce importem dat](search-import-data-portal.md) provede vás procesem vytvoření všechny tyto prostředky, včetně index.

> [!TIP]
> Z řídicího panelu služby Azure Cosmos DB můžete spustit průvodce **Importem dat** a zjednodušit tak indexování zdroje dat. Začněte tak, že v levém navigačním panelu přejdete do **Collections** (Kolekce) > **Add Azure Search** (Přidat službu Azure Search).

<a name="Concepts"></a>

## <a name="azure-search-indexer-concepts"></a>Koncepty indexer Azure Search
Azure Search podporuje vytváření a správu datové zdroje (včetně rozhraní API pro Azure Cosmos DB SQL) a indexery, které provozují těchto zdrojů.

A **zdroj dat** data určená k indexu, pověření a zásady pro identifikaci změny v datech (například dokumenty modified nebo deleted uvnitř vaší kolekce). Zdroj dat je definován jako nezávislý prostředek, aby se můžete použít několik indexerů.

**Indexer** popisuje, jak tok dat ze zdroje dat do indexu vyhledávání cíl. Indexer umožňuje:

* Proveďte jednorázové kopii dat k naplnění indexu.
* Synchronizujte indexu se změnami ve zdroji dat podle plánu. Plán je součástí definice indexer.
* Vyvolání aktualizace na vyžádání do indexu podle potřeby.

<a name="CreateDataSource"></a>

## <a name="step-1-create-a-data-source"></a>Krok 1: Vytvoření zdroje dat
Chcete-li vytvořit zdroj dat, proveďte příspěvku na:

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

Text žádosti obsahuje definice zdroje dat, která by měla obsahovat následující pole:

* **název**: Vyberte libovolný název představují vaší databáze.
* **typ**: musí být `documentdb`.
* **přihlašovací údaje**:
  
  * **connectionString**: vyžaduje. Zadejte informace o připojení k databázi Azure Cosmos DB v následujícím formátu:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kontejner**:
  
  * **název**: vyžaduje. Zadejte id kolekce databáze pro indexování.
  * **dotaz**: volitelné. Můžete zadat dotaz na narovnání libovolný dokument JSON na ploché schéma, které mohou indexu Azure Search.
* **dataChangeDetectionPolicy**: doporučené. V tématu [indexování dokumentů změnit](#DataChangeDetectionPolicy) části.
* **dataDeletionDetectionPolicy**: volitelné. V tématu [indexování dokumentů odstranit](#DataDeletionDetectionPolicy) části.

### <a name="using-queries-to-shape-indexed-data"></a>Pomocí dotazů na obrazec indexované dat
Můžete zadat příkazu jazyka SQL zploštění vnořené vlastnosti nebo pole, vlastnosti projektu JSON a filtrování dat pro indexování. 

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
Pokud již nemáte, vytvořte cílový index Azure Search. Můžete vytvořit index pomocí [portál Azure uživatelského rozhraní](search-create-index-portal.md), [vytvořit Index REST API](/rest/api/searchservice/create-index) nebo [Index – třída](/dotnet/api/microsoft.azure.search.models.index).

Následující příklad vytvoří index s polem id a popis:

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

Zajistěte, aby schéma cílový index kompatibilní s schéma dokumentů JSON zdroj nebo výstup projekce vašeho vlastního dotazu.

> [!NOTE]
> Pro dělené kolekce je výchozí dokument klíč Azure Cosmos DB `_rid` vlastností, která se získá přejmenován na `rid` ve službě Azure Search. Navíc Cosmos databázi Azure na `_rid` hodnoty obsahovat znaky, které jsou v Azure Search klíče neplatné. Z tohoto důvodu `_rid` hodnoty jsou kódováním Base64.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datové typy a typy dat vyhledávání systému Azure
| JSON datového typu | Typy polí kompatibilní cílový index |
| --- | --- |
| BOOL |Edm.Boolean Edm.String |
| Čísla, která vypadat podobně jako celá čísla |Edm.String Edm.Int32, Edm.Int64, |
| Čísla této vypadají plovoucí body |Edm.Double Edm.String |
| Řetězec |Edm.String |
| Pole jednoduchých typů, například ["a", "b", "c"] |Collection(Edm.String) |
| Řetězce, které vypadají podobně jako kalendářní data |Edm.DateTimeOffset Edm.String |
| GeoJSON objekty, například {"typ": "Místo", "coordinates": [dlouhý a lat]} |Edm.GeographyPoint |
| Jiné objekty JSON |neuvedeno |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>Krok 3: Vytvořením indexeru.

Jakmile byly vytvořeny index a zdroj dat, jste připraveni vytvořit indexeru:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Indexer se spustí každé dvě hodiny (interval plánování je nastavena na "PT2H"). Pokud chcete spustit indexer každých 30 minut, nastavte interval, který "PT30M". Nejkratší podporovaný interval je 5 minut. Plán je volitelné - li tento parametr vynechán, indexer se spustí pouze po jeho vytvoření. Indexer na vyžádání je však můžete spustit kdykoli.   

Další informace o rozhraní API Indexer vytvořit, podívejte se na [vytvořit Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Spuštění indexeru na vyžádání
Kromě spuštění pravidelně podle plánu, lze indexer také vyvolat na vyžádání:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Při spuštění rozhraní API vrátí úspěšně, bylo naplánováno vyvolání indexeru, ale vlastní zpracování probíhá asynchronně. 

Můžete sledovat stav indexer v portálu nebo pomocí získat Indexer stav rozhraní API, které jsme popisují další. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Získání stavu indexeru
Můžete načíst historii stavu a spuštění indexeru:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Odpověď obsahuje celkový stav indexer, indexer poslední (nebo probíhající) volání a historii poslední indexer volání.

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

Historie provádění obsahuje až 50 poslední dokončené spuštěních, které jsou seřazeny v opačném chronologickém pořadí (takže nejnovější provádění bude první v odpovědi).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indexování změněné dokumenty
Účelem zásady detekce změn dat je efektivně identifikaci položek změněná data. V současné době je pouze podporovaných zásad `High Water Mark` pomocí zásad `_ts` vlastnost (časové razítko) poskytované Cosmos databáze Azure, které se určuje takto:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Pomocí této zásady je vhodné zajistit indexer dobrý výkon. 

Pokud používáte vlastní dotaz, ujistěte se, že `_ts` vlastnost se promítá v dotazu.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Přírůstkové průběh a vlastní dotazy
Přírůstkové průběh během indexování zajistí, že pokud indexer provádění přeruší přechodných chyb nebo provádění časový limit, indexeru můžete vyzvedávat kde bylo přerušeno při příštím spuštění, aniž by museli znovu indexu celou kolekci od začátku. To je zvlášť důležité při indexování rozsáhlých kolekcí. 

Povolit přírůstkové průběh při použití vlastního dotazu, ujistěte se, že váš dotaz řadí výsledky podle `_ts` sloupce. To umožňuje pravidelné kontroly směřující která Azure Search používá k zajištění přírůstkové průběh v případě selhání.   

V některých případech, i pokud dotaz obsahuje `ORDER BY [collection alias]._ts` klauzuli vyhledávání systému Azure nemusí odvození že dotaz je seřazené podle `_ts`. Se dá zjistit Azure Search, výsledky jsou seřazené podle pomocí `assumeOrderByHighWaterMarkColumn` vlastnosti konfigurace. Pokud chcete zadat Tento pomocný, vytvořit nebo aktualizovat indexer následujícím způsobem: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indexování odstranit dokumenty
Při odstranění řádků z kolekce, obvykle chcete odstranit tyto řádky z také indexu vyhledávání. Účelem zásady detekce odstranění dat je efektivně identifikaci položek odstraněná data. V současné době je pouze podporovaných zásad `Soft Delete` zásad (odstranění označeno příznak nějaká), která je určena následujícím způsobem:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Pokud používáte vlastní dotaz, ujistěte se, že vlastnost odkazuje `softDeleteColumnName` se promítá v dotazu.

Následující příklad vytvoří zdroj dat s zásadu obnovitelného odstranění:

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
Blahopřejeme! Jste se naučili postup při integraci s Azure Search pomocí indexeru pro procházení a odesílat dokumenty z modelu dat SQL Azure Cosmos DB.

* Další informace o databázi Cosmos Azure, najdete v článku [stránku služby Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).
* Další informace o službě Azure Search najdete v tématu [stránku služby vyhledávání](https://azure.microsoft.com/services/search/).
