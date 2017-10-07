---
title: "aaaWorking s daty geoprostorové v Azure Cosmos DB | Microsoft Docs"
description: "Pochopit, jak toocreate, index a dotaz prostorových objekty s Azure Cosmos DB a hello DocumentDB rozhraní API."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 82ce2898-a9f9-4acf-af4d-8ca4ba9c7b8f
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/22/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1e40b78cb4595631d845d46c21d07a30c8b972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Práce s geoprostorové a GeoJSON umístění dat v Azure Cosmos DB
Tento článek slouží Úvod toohello geoprostorové funkce v [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Po přečtení to, budete moct tooanswer hello následující otázky:

* Jak ukládat prostorových dat v Azure Cosmos DB?
* Jak můžete dotazovat geoprostorové data v Azure DB Cosmos v SQL a LINQ?
* Jak povolit nebo zakázat prostorových indexování v Azure Cosmos DB?

Tento článek ukazuje, jak toowork s prostorovými daty formátu s hello DocumentDB rozhraní API. Najdete v tématu to [Githubu projektu](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) pro ukázky kódu.

## <a name="introduction-toospatial-data"></a>Úvod toospatial dat
Prostorová data popisuje hello pozice a tvar objektů v prostoru. Ve většině aplikací se tyto odpovídají tooobjects na zemi, hello, tj. geoprostorové data. Prostorová data můžou být použité toorepresent hello umístění osoby, místo zájmu nebo hranic hello města nebo jezero. Běžné případy použití často zahrnuje blízkosti dotazy pro například "najít všechny v kavárnách téměř Moje aktuální umístění". 

### <a name="geojson"></a>GeoJSON
Podporuje Azure Cosmos DB indexování a dotazování dat geoprostorové bodu, která je reprezentována pomocí hello [GeoJSON specifikace](https://tools.ietf.org/html/rfc7946). GeoJSON datové struktury, jsou vždy objekty JSON je platný, a tak, aby se mohou být uloženy a dotazovat pomocí Azure Cosmos DB bez jakýchkoli specializovaných nástrojů nebo knihovny. Hello SDK služby Azure Cosmos DB zadejte pomocné rutiny třídy a metody, které umožňují snadno toowork s prostorovými daty formátu. 

### <a name="points-linestrings-and-polygons"></a>Body, LineStrings a mnohoúhelníky
A **bodu** označuje jeden pozice v prostoru. V datech geoprostorové představuje bod hello přesné umístění, které by mohly být adresu supermarketu úložiště, celoobrazovkovém režimu, automobilu nebo města.  Bod je reprezentována v páru GeoJSON (a Azure Cosmos DB) pomocí jeho souřadnice nebo zeměpisné šířky a délky. Tady je příklad JSON pro bod.

**Body v Azure Cosmos DB**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> Určuje Hello GeoJSON specifikace délky první a zeměpisnou šířku druhý. Podobně jako v ostatních aplikacích mapování zeměpisné šířky a délky jsou úhly a uvádí stupňů. Hodnoty zeměpisné délky se měří od základního poledníku hello a jsou v rozmezí od -180 a 180.0 stupňů a zeměpisnou šířku hodnoty jsou v měřeny od rovníku hello a jsou mezi-90.0 a 90.0 stupňů. 
> 
> Azure Cosmos DB interpretuje souřadnice reprezentovaný za hello WGS 84 referenční systém. Níže naleznete podrobnosti o souřadnic referenčních systémů.
> 
> 

To může být vložen do dokument Azure Cosmos DB, jak je znázorněno v tomto příkladu obsahující data o umístění profilu uživatele:

**Použít profil s umístěním, které jsou uloženy v databázi Cosmos Azure**

```json
{
    "id":"documentdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

Kromě toho toopoints, GeoJSON také podporuje LineStrings a mnohoúhelníky. **LineStrings** představují řadu dva nebo víc bodů v prostoru a hello segmenty čáry, která se připojují, je. V datech geoprostorové LineStrings jsou běžně používané toorepresent dálnice nebo řek. A **mnohoúhelníku** je hranice připojené body, která tvoří uzavřené LineString. Mnohoúhelníky jsou běžně používané toorepresent přirozené vytváření jako jezera nebo politickou oblasti jurisdikce jako města a stavy. Tady je příklad mnohoúhelníku v Azure Cosmos DB. 

**Mnohoúhelníky v GeoJSON**

```json
{
    "type":"Polygon",
    "coordinates":[
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ]
}
```

> [!NOTE]
> Hello GeoJSON specifikace vyžaduje, aby pro platný mnohoúhelníky hello poslední souřadnic pár poskytuje hello stejné jako první, toocreate hello uzavřený obrazec.
> 
> Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček. Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje hello inverzní oblasti hello v něm.
> 
> 

V přidání tooPoint, LineString a mnohoúhelníku, GeoJSON také určuje vyjádření hello jak toogroup víc Geoprostorové umístění, jak dobře tooassociate libovolné vlastnosti s informace o zeměpisné poloze jako **funkce**. Vzhledem k tomu, že tyto objekty jsou platný kód JSON, všechny jde uloženy a zpracovány v Azure Cosmos DB. Ale Azure Cosmos DB podporuje pouze automatické indexování bodů.

### <a name="coordinate-reference-systems"></a>Koordinaci referenčních systémů
Vzhledem k tomu, že tvar hello hello earth je nestandardní, je reprezentována souřadnice geoprostorové data v mnoha systémy souřadnic odkaz (CRS), každou s vlastní referenční rámce a měrné jednotky. Například hello "National mřížky Británie" je referenční systém je velmi přesná hello Spojené království, ale ne mimo něj. 

Hello nejoblíbenější řádku používá v současné době jsou hello World geodetické systému [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS zařízení a velký počet mapování služby včetně mapy Google a rozhraní API map Bing pomocí WGS 84. Azure Cosmos DB podporuje indexování a dotazování dat geoprostorové pouze hello WGS 84 řádku. 

## <a name="creating-documents-with-spatial-data"></a>Vytváření dokumentů s prostorovými daty
Při vytváření dokumentů, které obsahují GeoJSON hodnoty jsou automaticky indexovány s prostorového indexu v souladu zásady indexování toohello hello kolekce. Pokud pracujete se sadou Azure SDK DB Cosmos v jazyce dynamicky zadávaných jako Pythonu nebo Node.js, musíte vytvořit platný GeoJSON.

**Vytvořte dokument s daty geoprostorové v Node.js**

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within hello callback
});
```

Pokud pracujete s hello DocumentDB rozhraní API, můžete použít hello `Point` a `Polygon` tříd v rámci hello `Microsoft.Azure.Documents.Spatial` informace o umístění tooembed oboru názvů v rámci vašich objektů aplikace. Tyto třídy zjednodušit hello serializace a deserializace prostorových dat do GeoJSON.

**Vytvořte dokument s daty geoprostorové v rozhraní .NET**

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "documentdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

Pokud nemáte hello zeměpisnou šířku a délku informace, ale název umístění jako města nebo země nebo hello fyzické adresy, můžete vyhledat skutečné souřadnice hello pomocí služeb určování zeměpisných souřadnic jako služby REST Bing Maps. Další informace o určování zeměpisných souřadnic mapy Bing [zde](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Dotazování prostorové typy
Teď, podívejte se na to, jak bylo tooinsert geoprostorové data, Podívejme se na to, jak tooquery tato data pomocí Azure DB Cosmos pomocí SQL a LINQ.

### <a name="spatial-sql-built-in-functions"></a>Prostorové integrované funkce SQL
Azure Cosmos DB podporuje následující otevřete geoprostorové Consortium (OGC) integrované funkce pro dotazování geoprostorové hello. Další informace o hello kompletní sadu integrovaných funkcí v hello jazyk SQL naleznete příliš[dotazu Azure Cosmos DB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Využití</strong></td>
  <td><strong>Popis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (spatial_expr, spatial_expr)</td>
  <td>Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.</td>
</tr>
<tr>
  <td>ST_WITHIN (spatial_expr, spatial_expr)</td>
  <td>Vrací výraz logická hodnota určující, zda text hello první GeoJSON objekt (bod, mnohoúhelníku nebo LineString) je v rámci hello druhý GeoJSON objekt (bod, mnohoúhelníku nebo LineString).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Vrátí hodnotu označující, zda text hello dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</td>
</tr>
</table>

Prostorové funkce můžou být použité tooperform blízkosti dotazy pro prostorová data. Tady je příklad dotaz, který vrátí že všechny rodiny dokumenty, v rámci 30 km hello zadané umístění používáte hello ST_DISTANCE integrovaná funkce. 

**Dotaz**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Pokud zahrnete prostorových indexování v zásady indexování, pak "vzdálenost dotazy" se zpracuje efektivně prostřednictvím hello index. Další informace o prostorových indexování najdete v tématu hello části. Pokud nemáte prostorový index pro hello zadané cesty, můžete přesto provést prostorových dotazů zadáním `x-ms-documentdb-query-enable-scan` hlavička požadavku s hodnotou hello nastavit příliš "true". V rozhraní .NET, to lze provést pomocí předání hello volitelné **FeedOptions** tooqueries argument s [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) nastavit tootrue. 

ST_WITHIN může být použité toocheck, pokud bod leží uvnitř mnohoúhelníku. Mnohoúhelníky jsou běžně používané toorepresent hranice jako PSČ, hranice stavu nebo přírodní vytváření. Znovu Pokud zahrnete prostorových indexování v zásady indexování, pak "v" dotazy se zpracuje efektivně prostřednictvím hello index. 

Argumenty mnohoúhelníku v ST_WITHIN může obsahovat pouze jedno zazvonění, tj. hello mnohoúhelníky nesmí obsahovat mezery v nich. 

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Výsledky**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Podobné typy toohow neshoda funguje v Azure Cosmos DB dotazu, pokud hodnota hello umístění v zadána buď argument je chybný nebo není platný, pak vyhodnotí příliš**nedefinované** a toobe dokumentu hello vyhodnotit přeskočena z hello výsledky dotazu. Pokud dotaz vrátí žádné výsledky, spusťte ST_ISVALIDDETAILED toodebug proč hello spatail typ je neplatný.     
> 
> 

Azure Cosmos DB také podporuje provádění inverzní dotazy, tj. můžete index mnohoúhelníky nebo řádků v databázi Cosmos Azure a pak dotazu pro hello oblasti, které obsahují zadaný bod. Tento vzor se často používá v logistiky tooidentify například když vůz zadá nebo opustí určené oblasti. 

**Dotaz**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Výsledky**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID a ST_ISVALIDDETAILED můžou být použité toocheck, pokud prostorový objekt není platný. Například hello následující dotaz ověří platnost hello bod s out hodnoty zeměpisné šířky rozsah (-132.8). ST_ISVALID vrací pouze logickou hodnotu, a vrátí ST_ISVALIDDETAILED hello řetězec obsahující hello důvod, proč má se za neplatný a logickou hodnotu.

** Dotaz **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Výsledky**

    [{
      "$1": false
    }]

Tyto funkce lze také použít toovalidate mnohoúhelníky. Například tady používáme ST_ISVALIDDETAILED toovalidate mnohoúhelníku, který není uzavřený. 

**Dotaz**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Výsledky**

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a>Dotazy LINQ v hello .NET SDK
Hello DocumentDB .NET SDK také poskytovatelé zóny se zakázaným inzerováním metody `Distance()` a `Within()` pro použití v rámci LINQ – výrazy. Hello DocumentDB LINQ zprostředkovatele znamená, že je tato metoda volání toohello ekvivalentní integrovaná funkce volání SQL (ST_DISTANCE a ST_WITHIN v uvedeném pořadí). 

Zde uvádíme příklad dotazu LINQ, který vyhledá všechny dokumenty v kolekci Azure Cosmos DB hello, jehož hodnota "umístění" je v rámci okruhu 30km hello zadána bodu pomocí LINQ.

**Dotaz LINQ pro vzdálenost**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Podobně je zde dotazu pro vyhledání všech hello dokumentů, jejichž "umístění" je v rámci hello zadaná pole nebo mnohoúhelníku. 

**LINQ dotazu v rámci**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Teď, podívejte se na to, jak bylo tooquery dokumentů pomocí LINQ a SQL, Podívejme se na to, jak tooconfigure Azure Cosmos DB pro prostorových indexování.

## <a name="indexing"></a>Indexování
Jsme popsané v hello [schématu bez ohledu na indexování s Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papír, jsme chtěli toobe modul databáze Azure Cosmos DB skutečně nezávislá na schéma a poskytovat prvotřídní podporu pro formát JSON. databázový stroj zápisu optimalizované Hello Azure Cosmos databáze nativně rozumí prostorových dat (body, mnohoúhelníků a čar) v hello GeoJSON standard.

Stručně řečeno, je hello geometrie projektovat z geodetické souřadnice na 2D rovinu pak progresivně rozdělené do buňky pomocí **quadtree**. Tyto buňky jsou namapované too1D podle umístění hello hello buňky v rámci **Hilbertův místo naplnění křivky**, který zachovává polohu bodů. Dále pokud je indexovaný data o umístění, prochází skrz tento proces se označuje jako **teselace**, tj. všechny hello buněk, které intersect umístění jsou identifikovat a uloží jako klíče v Azure Cosmos DB indexu hello. V době dotazů argumenty jako body a mnohoúhelníky jsou také teselace sestavy tooextract hello oblastí relevantní buněk ID a potom použít tooretrieve data z indexu hello.

Pokud zadáte zásady indexování, zahrnující prostorový index pro / * (všechny cesty), pak všechny body v rámci kolekce hello nalezen jsou indexované pro efektivní prostorových dotazů (ST_WITHIN a ST_DISTANCE). Prostorové indexy nemáte hodnotu přesnost a vždy použít výchozí hodnotu přesnost.

> [!NOTE]
> Azure Cosmos DB podporuje automatické indexování bodů, mnohoúhelníky a LineStrings
> 
> 

Hello následující fragment kódu JSON zobrazí zásady indexování s prostorových indexování povolena, tj. kdykoli GeoJSON v rámci dokumenty nalezen prostorových dotazování indexu. Chcete-li změnit hello indexování zásady pomocí hello portálu Azure, můžete zadat hello následující JSON pro indexování zásad tooenable prostorových indexování do kolekce.

**Kolekce JSON zásady indexování s Spatial povoleno pro body a mnohoúhelníky**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Zde je fragment kódu v rozhraní .NET, který ukazuje, jak toocreate kolekce se prostorových indexování zapnuté pro všechny cesty obsahující body. 

**Vytvořte kolekci s prostorových indexování**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

A tady je, jak můžete upravit existující kolekci tootake výhodou prostorových indexování přes všechny body, které jsou uloženy v rámci dokumenty.

**Upravit existující kolekci s prostorových indexování**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing toocomplete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> Pokud umístění hello GeoJSON hodnotu v dokumentu hello je chybný nebo není platný, pak jej nebude získat indexovaný prostorových dotazování. Můžete ověřit pomocí ST_ISVALID a ST_ISVALIDDETAILED hodnoty umístění.
> 
> Pokud svou definici. kolekce obsahuje klíč oddílu, není hlášena indexování průběh transformace. 
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když jste dozvědí o tom, jak začít tooget s podporou geoprostorové v Azure Cosmos DB, můžete:

* Psaní s hello [ukázky kódu .NET geoprostorové na Githubu](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* Získat rukou na s geoprostorové dotazování na hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)
* Další informace o [Azure Cosmos DB dotazu](documentdb-sql-query.md)
* Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)

