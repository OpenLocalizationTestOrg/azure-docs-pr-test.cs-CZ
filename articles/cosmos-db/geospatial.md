---
title: "Práce s daty geoprostorové v Azure Cosmos DB | Microsoft Docs"
description: "Pochopit, jak vytvářet, index a dotaz prostorových objekty s Azure Cosmos DB a rozhraní API DocumentDB."
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
ms.openlocfilehash: d5785c81fb597e7d30eb7d3a880e7194d8358ed5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a><span data-ttu-id="7d08b-103">Práce s geoprostorové a GeoJSON umístění dat v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7d08b-103">Working with geospatial and GeoJSON location data in Azure Cosmos DB</span></span>
<span data-ttu-id="7d08b-104">Tento článek je úvodem k funkci geoprostorové v [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="7d08b-104">This article is an introduction to the geospatial functionality in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="7d08b-105">Po přečtení to, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="7d08b-105">After reading this, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="7d08b-106">Jak ukládat prostorových dat v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="7d08b-106">How do I store spatial data in Azure Cosmos DB?</span></span>
* <span data-ttu-id="7d08b-107">Jak můžete dotazovat geoprostorové data v Azure DB Cosmos v SQL a LINQ?</span><span class="sxs-lookup"><span data-stu-id="7d08b-107">How can I query geospatial data in Azure Cosmos DB in SQL and LINQ?</span></span>
* <span data-ttu-id="7d08b-108">Jak povolit nebo zakázat prostorových indexování v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="7d08b-108">How do I enable or disable spatial indexing in Azure Cosmos DB?</span></span>

<span data-ttu-id="7d08b-109">Tento článek ukazuje, jak pracovat s prostorových dat s rozhraním API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="7d08b-109">This article shows how to work with spatial data with the DocumentDB API.</span></span> <span data-ttu-id="7d08b-110">Najdete v tématu to [Githubu projektu](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) pro ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="7d08b-110">Please see this [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) for code samples.</span></span>

## <a name="introduction-to-spatial-data"></a><span data-ttu-id="7d08b-111">Úvod do prostorových dat</span><span class="sxs-lookup"><span data-stu-id="7d08b-111">Introduction to spatial data</span></span>
<span data-ttu-id="7d08b-112">Prostorová data popisuje pozice a tvar objektů v prostoru.</span><span class="sxs-lookup"><span data-stu-id="7d08b-112">Spatial data describes the position and shape of objects in space.</span></span> <span data-ttu-id="7d08b-113">Ve většině aplikací se tyto odpovídají objektů na zemském povrchu, tj. geoprostorové data.</span><span class="sxs-lookup"><span data-stu-id="7d08b-113">In most applications, these correspond to objects on the earth, i.e. geospatial data.</span></span> <span data-ttu-id="7d08b-114">Prostorová data můžete používá k reprezentování umístění osoby, místo zájmu nebo hranici města nebo jezero.</span><span class="sxs-lookup"><span data-stu-id="7d08b-114">Spatial data can be used to represent the location of a person, a place of interest, or the boundary of a city, or a lake.</span></span> <span data-ttu-id="7d08b-115">Běžné případy použití často zahrnuje blízkosti dotazy pro například "najít všechny v kavárnách téměř Moje aktuální umístění".</span><span class="sxs-lookup"><span data-stu-id="7d08b-115">Common use cases often involve proximity queries, for e.g., "find all coffee shops near my current location".</span></span> 

### <a name="geojson"></a><span data-ttu-id="7d08b-116">GeoJSON</span><span class="sxs-lookup"><span data-stu-id="7d08b-116">GeoJSON</span></span>
<span data-ttu-id="7d08b-117">Podporuje Azure Cosmos DB indexování a dotazování dat geoprostorové bodu, která je reprezentována pomocí [GeoJSON specifikace](https://tools.ietf.org/html/rfc7946).</span><span class="sxs-lookup"><span data-stu-id="7d08b-117">Azure Cosmos DB supports indexing and querying of geospatial point data that's represented using the [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span></span> <span data-ttu-id="7d08b-118">GeoJSON datové struktury, jsou vždy objekty JSON je platný, a tak, aby se mohou být uloženy a dotazovat pomocí Azure Cosmos DB bez jakýchkoli specializovaných nástrojů nebo knihovny.</span><span class="sxs-lookup"><span data-stu-id="7d08b-118">GeoJSON data structures are always valid JSON objects, so they can be stored and queried using Azure Cosmos DB without any specialized tools or libraries.</span></span> <span data-ttu-id="7d08b-119">Sady SDK Azure Cosmos DB zadejte pomocné třídy a metody, které usnadňují pracovat s prostorovými daty formátu.</span><span class="sxs-lookup"><span data-stu-id="7d08b-119">The Azure Cosmos DB SDKs provide helper classes and methods that make it easy to work with spatial data.</span></span> 

### <a name="points-linestrings-and-polygons"></a><span data-ttu-id="7d08b-120">Body, LineStrings a mnohoúhelníky</span><span class="sxs-lookup"><span data-stu-id="7d08b-120">Points, LineStrings and Polygons</span></span>
<span data-ttu-id="7d08b-121">A **bodu** označuje jeden pozice v prostoru.</span><span class="sxs-lookup"><span data-stu-id="7d08b-121">A **Point** denotes a single position in space.</span></span> <span data-ttu-id="7d08b-122">V datech geoprostorové představuje bod přesné umístění, které by mohly být adresu supermarketu úložiště, celoobrazovkovém režimu, automobilu nebo města.</span><span class="sxs-lookup"><span data-stu-id="7d08b-122">In geospatial data, a Point represents the exact location, which could be a street address of a grocery store, a kiosk, an automobile or a city.</span></span>  <span data-ttu-id="7d08b-123">Bod je reprezentována v páru GeoJSON (a Azure Cosmos DB) pomocí jeho souřadnice nebo zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-123">A point is represented in GeoJSON (and Azure Cosmos DB) using its coordinate pair or longitude and latitude.</span></span> <span data-ttu-id="7d08b-124">Tady je příklad JSON pro bod.</span><span class="sxs-lookup"><span data-stu-id="7d08b-124">Here's an example JSON for a point.</span></span>

<span data-ttu-id="7d08b-125">**Body v Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="7d08b-125">**Points in Azure Cosmos DB**</span></span>

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> <span data-ttu-id="7d08b-126">Specifikace GeoJSON určuje zeměpisnou délku první a zeměpisnou šířku druhý.</span><span class="sxs-lookup"><span data-stu-id="7d08b-126">The GeoJSON specification specifies longitude first and latitude second.</span></span> <span data-ttu-id="7d08b-127">Podobně jako v ostatních aplikacích mapování zeměpisné šířky a délky jsou úhly a uvádí stupňů.</span><span class="sxs-lookup"><span data-stu-id="7d08b-127">Like in other mapping applications, longitude and latitude are angles and represented in terms of degrees.</span></span> <span data-ttu-id="7d08b-128">Hodnoty zeměpisné délky se měří od základního poledníku a jsou v rozmezí od -180 a 180.0 stupňů a zeměpisnou šířku hodnoty jsou v měřeny od rovníku a jsou mezi-90.0 a 90.0 stupňů.</span><span class="sxs-lookup"><span data-stu-id="7d08b-128">Longitude values are measured from the Prime Meridian and are between -180 and 180.0 degrees, and latitude values are measured from the equator and are between -90.0 and 90.0 degrees.</span></span> 
> 
> <span data-ttu-id="7d08b-129">Azure Cosmos DB interpretuje souřadnice reprezentovaný za WGS 84 referenčního systému.</span><span class="sxs-lookup"><span data-stu-id="7d08b-129">Azure Cosmos DB interprets coordinates as represented per the WGS-84 reference system.</span></span> <span data-ttu-id="7d08b-130">Níže naleznete podrobnosti o souřadnic referenčních systémů.</span><span class="sxs-lookup"><span data-stu-id="7d08b-130">Please see below for more details about coordinate reference systems.</span></span>
> 
> 

<span data-ttu-id="7d08b-131">To může být vložen do dokument Azure Cosmos DB, jak je znázorněno v tomto příkladu obsahující data o umístění profilu uživatele:</span><span class="sxs-lookup"><span data-stu-id="7d08b-131">This can be embedded in an Azure Cosmos DB document as shown in this example of a user profile containing location data:</span></span>

<span data-ttu-id="7d08b-132">**Použít profil s umístěním, které jsou uloženy v databázi Cosmos Azure**</span><span class="sxs-lookup"><span data-stu-id="7d08b-132">**Use Profile with Location stored in Azure Cosmos DB**</span></span>

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

<span data-ttu-id="7d08b-133">Kromě bodů GeoJSON také podporuje LineStrings a mnohoúhelníky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-133">In addition to points, GeoJSON also supports LineStrings and Polygons.</span></span> <span data-ttu-id="7d08b-134">**LineStrings** představují řadu dva nebo víc bodů v prostoru i segmenty čáry, která se připojují, je.</span><span class="sxs-lookup"><span data-stu-id="7d08b-134">**LineStrings** represent a series of two or more points in space and the line segments that connect them.</span></span> <span data-ttu-id="7d08b-135">V datech geoprostorové LineStrings běžně se používají k reprezentaci dálnice nebo řeky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-135">In geospatial data, LineStrings are commonly used to represent highways or rivers.</span></span> <span data-ttu-id="7d08b-136">A **mnohoúhelníku** je hranice připojené body, která tvoří uzavřené LineString.</span><span class="sxs-lookup"><span data-stu-id="7d08b-136">A **Polygon** is a boundary of connected points that forms a closed LineString.</span></span> <span data-ttu-id="7d08b-137">Mnohoúhelníky se běžně používají k vyjádření přirozené vytváření jako jezera nebo politickou oblasti jurisdikce jako města a stavy.</span><span class="sxs-lookup"><span data-stu-id="7d08b-137">Polygons are commonly used to represent natural formations like lakes or political jurisdictions like cities and states.</span></span> <span data-ttu-id="7d08b-138">Tady je příklad mnohoúhelníku v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7d08b-138">Here's an example of a Polygon in Azure Cosmos DB.</span></span> 

<span data-ttu-id="7d08b-139">**Mnohoúhelníky v GeoJSON**</span><span class="sxs-lookup"><span data-stu-id="7d08b-139">**Polygons in GeoJSON**</span></span>

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
> <span data-ttu-id="7d08b-140">Specifikace GeoJSON vyžaduje, aby pro platný mnohoúhelníky, poslední souřadnic pár zadat stejný jako první, chcete-li vytvořit uzavřený obrazec.</span><span class="sxs-lookup"><span data-stu-id="7d08b-140">The GeoJSON specification requires that for valid Polygons, the last coordinate pair provided should be the same as the first, to create a closed shape.</span></span>
> 
> <span data-ttu-id="7d08b-141">Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="7d08b-141">Points within a Polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="7d08b-142">Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje inverzní oblasti v něm.</span><span class="sxs-lookup"><span data-stu-id="7d08b-142">A Polygon specified in clockwise order represents the inverse of the region within it.</span></span>
> 
> 

<span data-ttu-id="7d08b-143">Kromě Point, LineString a mnohoúhelníku GeoJSON také určuje vyjádření pro způsob seskupení více Geoprostorové umístění a také jak přidružit informace o zeměpisné poloze jako libovolné vlastnosti **funkce**.</span><span class="sxs-lookup"><span data-stu-id="7d08b-143">In addition to Point, LineString and Polygon, GeoJSON also specifies the representation for how to group multiple geospatial locations, as well as how to associate arbitrary properties with geolocation as a **Feature**.</span></span> <span data-ttu-id="7d08b-144">Vzhledem k tomu, že tyto objekty jsou platný kód JSON, všechny jde uloženy a zpracovány v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7d08b-144">Since these objects are valid JSON, they can all be stored and processed in Azure Cosmos DB.</span></span> <span data-ttu-id="7d08b-145">Ale Azure Cosmos DB podporuje pouze automatické indexování bodů.</span><span class="sxs-lookup"><span data-stu-id="7d08b-145">However Azure Cosmos DB only supports automatic indexing of points.</span></span>

### <a name="coordinate-reference-systems"></a><span data-ttu-id="7d08b-146">Koordinaci referenčních systémů</span><span class="sxs-lookup"><span data-stu-id="7d08b-146">Coordinate reference systems</span></span>
<span data-ttu-id="7d08b-147">Vzhledem k tomu, že tvar zemském povrchu je nestandardní, je reprezentována souřadnice geoprostorové data v mnoha systémy souřadnic odkaz (CRS), každou s vlastní referenční rámce a měrné jednotky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-147">Since the shape of the earth is irregular, coordinates of geospatial data is represented in many coordinate reference systems (CRS), each with their own frames of reference and units of measurement.</span></span> <span data-ttu-id="7d08b-148">Například "National mřížky z Británie" je referenční systém je velmi přesná pro Velkou Británii, ale ne mimo něj.</span><span class="sxs-lookup"><span data-stu-id="7d08b-148">For example, the "National Grid of Britain" is a reference system is very accurate for the United Kingdom, but not outside it.</span></span> 

<span data-ttu-id="7d08b-149">Nejoblíbenější řádku používá dnes je systém geodetické World [WGS 84](http://earth-info.nga.mil/GandG/wgs84/).</span><span class="sxs-lookup"><span data-stu-id="7d08b-149">The most popular CRS in use today is the World Geodetic System  [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span></span> <span data-ttu-id="7d08b-150">GPS zařízení a velký počet mapování služby včetně mapy Google a rozhraní API map Bing pomocí WGS 84.</span><span class="sxs-lookup"><span data-stu-id="7d08b-150">GPS devices, and many mapping services including Google Maps and Bing Maps APIs use WGS-84.</span></span> <span data-ttu-id="7d08b-151">Azure Cosmos DB podporuje indexování a dotazování dat geoprostorové rezervační WGS 84 systém pouze.</span><span class="sxs-lookup"><span data-stu-id="7d08b-151">Azure Cosmos DB supports indexing and querying of geospatial data using the WGS-84 CRS only.</span></span> 

## <a name="creating-documents-with-spatial-data"></a><span data-ttu-id="7d08b-152">Vytváření dokumentů s prostorovými daty</span><span class="sxs-lookup"><span data-stu-id="7d08b-152">Creating documents with spatial data</span></span>
<span data-ttu-id="7d08b-153">Když vytvoříte dokumenty, které obsahují GeoJSON hodnoty, budou se automaticky indexované prostorový index podle zásady indexování kolekce.</span><span class="sxs-lookup"><span data-stu-id="7d08b-153">When you create documents that contain GeoJSON values, they are automatically indexed with a spatial index in accordance to the indexing policy of the collection.</span></span> <span data-ttu-id="7d08b-154">Pokud pracujete se sadou Azure SDK DB Cosmos v jazyce dynamicky zadávaných jako Pythonu nebo Node.js, musíte vytvořit platný GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="7d08b-154">If you're working with an Azure Cosmos DB SDK in a dynamically typed language like Python or Node.js, you must create valid GeoJSON.</span></span>

<span data-ttu-id="7d08b-155">**Vytvořte dokument s daty geoprostorové v Node.js**</span><span class="sxs-lookup"><span data-stu-id="7d08b-155">**Create Document with Geospatial data in Node.js**</span></span>

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

<span data-ttu-id="7d08b-156">Při práci s rozhraními API sady DocumentDB, můžete použít `Point` a `Polygon` tříd v rámci `Microsoft.Azure.Documents.Spatial` obor názvů pro vložení informace o umístění v rámci vašich objektů aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d08b-156">If you're working with the DocumentDB APIs, you can use the `Point` and `Polygon` classes within the `Microsoft.Azure.Documents.Spatial` namespace to embed location information within your application objects.</span></span> <span data-ttu-id="7d08b-157">Tyto třídy zjednodušit serializace a deserializace prostorových dat do GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="7d08b-157">These classes help simplify the serialization and deserialization of spatial data into GeoJSON.</span></span>

<span data-ttu-id="7d08b-158">**Vytvořte dokument s daty geoprostorové v rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="7d08b-158">**Create Document with Geospatial data in .NET**</span></span>

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

<span data-ttu-id="7d08b-159">Pokud nemáte informace o zeměpisné šířky a délky, ale název umístění jako města nebo země nebo fyzické adresy, můžete vyhledat skutečné souřadnice pomocí služeb určování zeměpisných souřadnic jako služby REST Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="7d08b-159">If you don't have the latitude and longitude information, but have the physical addresses or location name like city or country, you can look up the actual coordinates by using a geocoding service like Bing Maps REST Services.</span></span> <span data-ttu-id="7d08b-160">Další informace o určování zeměpisných souřadnic mapy Bing [zde](https://msdn.microsoft.com/library/ff701713.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d08b-160">Learn more about Bing Maps geocoding [here](https://msdn.microsoft.com/library/ff701713.aspx).</span></span>

## <a name="querying-spatial-types"></a><span data-ttu-id="7d08b-161">Dotazování prostorové typy</span><span class="sxs-lookup"><span data-stu-id="7d08b-161">Querying spatial types</span></span>
<span data-ttu-id="7d08b-162">Teď, když bylo podívejte se na tom, jak vkládání dat geoprostorové, Podívejme se na postup dotazování tato data pomocí Azure DB Cosmos pomocí SQL a LINQ.</span><span class="sxs-lookup"><span data-stu-id="7d08b-162">Now that we've taken a look at how to insert geospatial data, let's take a look at how to query this data using Azure Cosmos DB using SQL and LINQ.</span></span>

### <a name="spatial-sql-built-in-functions"></a><span data-ttu-id="7d08b-163">Prostorové integrované funkce SQL</span><span class="sxs-lookup"><span data-stu-id="7d08b-163">Spatial SQL built-in functions</span></span>
<span data-ttu-id="7d08b-164">Azure Cosmos DB podporuje následující předdefinované funkce otevřete geoprostorové Consortium (OGC) pro geoprostorové dotazování.</span><span class="sxs-lookup"><span data-stu-id="7d08b-164">Azure Cosmos DB supports the following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> <span data-ttu-id="7d08b-165">Další informace o kompletní sadu integrovaných funkcí v jazyce SQL, naleznete v [dotazu Azure Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="7d08b-165">For more details on the complete set of built-in functions in the SQL language, please refer to [Query Azure Cosmos DB](documentdb-sql-query.md).</span></span>

<table>
<tr>
  <td><span data-ttu-id="7d08b-166"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="7d08b-166"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="7d08b-167"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="7d08b-167"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="7d08b-168">ST_DISTANCE (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="7d08b-168">ST_DISTANCE (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="7d08b-169">Vrací vzdálenost mezi dvěma GeoJSON bodu, mnohoúhelníku nebo LineString výrazy.</span><span class="sxs-lookup"><span data-stu-id="7d08b-169">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="7d08b-170">ST_WITHIN (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="7d08b-170">ST_WITHIN (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="7d08b-171">Vrací výraz logická hodnota určující, zda je první objekt GeoJSON (bod, mnohoúhelníku nebo LineString) je v rámci druhý objekt GeoJSON (bod, mnohoúhelníku nebo LineString).</span><span class="sxs-lookup"><span data-stu-id="7d08b-171">Returns a Boolean expression indicating whether the first GeoJSON object (Point, Polygon, or LineString) is within the second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="7d08b-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="7d08b-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="7d08b-173">Vrátí hodnotu označující, zda dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</span><span class="sxs-lookup"><span data-stu-id="7d08b-173">Returns a Boolean expression indicating whether the two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="7d08b-174">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="7d08b-174">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="7d08b-175">Vrátí logickou hodnotu udávající, zda je zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString platný.</span><span class="sxs-lookup"><span data-stu-id="7d08b-175">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="7d08b-176">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="7d08b-176">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="7d08b-177">Vrátí hodnotu hodnotu JSON obsahující logickou hodnotu, pokud zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString je platný a pokud neplatný, dále z důvodu jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="7d08b-177">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="7d08b-178">Prostorové funkcí lze provádět dotazy blízkosti proti prostorová data.</span><span class="sxs-lookup"><span data-stu-id="7d08b-178">Spatial functions can be used to perform proximity queries against spatial data.</span></span> <span data-ttu-id="7d08b-179">Tady je příklad dotaz, který vrátí všechny rodiny dokumenty, které jsou v rámci 30 km v zadaném umístění pomocí předdefinované funkci ST_DISTANCE.</span><span class="sxs-lookup"><span data-stu-id="7d08b-179">For example, here's a query that returns all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="7d08b-180">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="7d08b-180">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="7d08b-181">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-181">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="7d08b-182">Pokud zahrnete prostorových indexování v zásady indexování, pak "vzdálenost dotazy" se zpracuje efektivně prostřednictvím index.</span><span class="sxs-lookup"><span data-stu-id="7d08b-182">If you include spatial indexing in your indexing policy, then "distance queries" will be served efficiently through the index.</span></span> <span data-ttu-id="7d08b-183">Další informace o prostorových indexování najdete v následující části.</span><span class="sxs-lookup"><span data-stu-id="7d08b-183">For more details on spatial indexing, please see the section below.</span></span> <span data-ttu-id="7d08b-184">Pokud nemáte prostorový index pro zadané cesty, stále můžete provádět prostorových dotazů zadáním `x-ms-documentdb-query-enable-scan` hlavička požadavku s nastavenou hodnotu "true".</span><span class="sxs-lookup"><span data-stu-id="7d08b-184">If you don't have a spatial index for the specified paths, you can still perform spatial queries by specifying `x-ms-documentdb-query-enable-scan` request header with the value set to "true".</span></span> <span data-ttu-id="7d08b-185">V rozhraní .NET, to lze provést pomocí předání nepovinný **FeedOptions** argument pro dotazy s [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) nastaven na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7d08b-185">In .NET, this can be done by passing the optional **FeedOptions** argument to queries with [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) set to true.</span></span> 

<span data-ttu-id="7d08b-186">ST_WITHIN slouží ke kontrole, pokud bod leží uvnitř mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="7d08b-186">ST_WITHIN can be used to check if a point lies within a Polygon.</span></span> <span data-ttu-id="7d08b-187">Mnohoúhelníky se běžně používají k vyjádření hranice jako PSČ, hranice stavu nebo přírodní vytváření.</span><span class="sxs-lookup"><span data-stu-id="7d08b-187">Commonly Polygons are used to represent boundaries like zip codes, state boundaries, or natural formations.</span></span> <span data-ttu-id="7d08b-188">Znovu Pokud zahrnete prostorových indexování v zásady indexování, pak "v" dotazy se zpracuje efektivně prostřednictvím index.</span><span class="sxs-lookup"><span data-stu-id="7d08b-188">Again if you include spatial indexing in your indexing policy, then "within" queries will be served efficiently through the index.</span></span> 

<span data-ttu-id="7d08b-189">Argumenty mnohoúhelníku v ST_WITHIN může obsahovat pouze jedno zazvonění, tj. polygonů nesmí obsahovat mezery v nich.</span><span class="sxs-lookup"><span data-stu-id="7d08b-189">Polygon arguments in ST_WITHIN can contain only a single ring, i.e. the Polygons must not contain holes in them.</span></span> 

<span data-ttu-id="7d08b-190">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="7d08b-190">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

<span data-ttu-id="7d08b-191">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-191">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> <span data-ttu-id="7d08b-192">Podobně jako funguje jak neodpovídající typy v Azure Cosmos DB dotazu, pokud je hodnota umístění zadaná v buď argument je chybný nebo není platný, pak bude vyhodnocena jako **nedefinované** a vyhodnotí dokumentu, který má být přeskočeno z dotazu výsledky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-192">Similar to how mismatched types works in Azure Cosmos DB query, if the location value specified in either argument is malformed or invalid, then it will evaluate to **undefined** and the evaluated document to be skipped from the query results.</span></span> <span data-ttu-id="7d08b-193">Pokud dotaz vrátí žádné výsledky, spusťte ST_ISVALIDDETAILED k ladění proč typ spatail je neplatný.</span><span class="sxs-lookup"><span data-stu-id="7d08b-193">If your query returns no results, run ST_ISVALIDDETAILED To debug why the spatail type is invalid.</span></span>     
> 
> 

<span data-ttu-id="7d08b-194">Azure Cosmos DB také podporuje provádění inverzní dotazy, tj. můžete index mnohoúhelníky nebo řádků v databázi Cosmos Azure a pak dotazu pro oblasti, které obsahují zadaný bod.</span><span class="sxs-lookup"><span data-stu-id="7d08b-194">Azure Cosmos DB also supports performing inverse queries, i.e. you can index Polygons or lines in Azure Cosmos DB, then query for the areas that contain a specified point.</span></span> <span data-ttu-id="7d08b-195">Tento vzor se obvykle používá v logistiky k identifikaci například když vůz zadá nebo opustí určené oblasti.</span><span class="sxs-lookup"><span data-stu-id="7d08b-195">This pattern is commonly used in logistics to identify e.g. when a truck enters or leaves a designated area.</span></span> 

<span data-ttu-id="7d08b-196">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="7d08b-196">**Query**</span></span>

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


<span data-ttu-id="7d08b-197">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-197">**Results**</span></span>

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

<span data-ttu-id="7d08b-198">ST_ISVALID a ST_ISVALIDDETAILED slouží ke kontrole, jestli je objekt prostorových platný.</span><span class="sxs-lookup"><span data-stu-id="7d08b-198">ST_ISVALID and ST_ISVALIDDETAILED can be used to check if a spatial object is valid.</span></span> <span data-ttu-id="7d08b-199">Například následující dotaz kontroluje platnost bod s out hodnoty zeměpisné šířky rozsah (-132.8).</span><span class="sxs-lookup"><span data-stu-id="7d08b-199">For example, the following query checks the validity of a point with an out of range latitude value (-132.8).</span></span> <span data-ttu-id="7d08b-200">ST_ISVALID vrací pouze logickou hodnotu, a ST_ISVALIDDETAILED vrátí řetězec obsahující důvod, proč má se za neplatný a logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7d08b-200">ST_ISVALID returns just a Boolean value, and ST_ISVALIDDETAILED returns the Boolean and a string containing the reason why it is considered invalid.</span></span>

<span data-ttu-id="7d08b-201">** Dotaz **</span><span class="sxs-lookup"><span data-stu-id="7d08b-201">** Query **</span></span>

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

<span data-ttu-id="7d08b-202">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-202">**Results**</span></span>

    [{
      "$1": false
    }]

<span data-ttu-id="7d08b-203">Tyto funkce lze také ověřit mnohoúhelníky.</span><span class="sxs-lookup"><span data-stu-id="7d08b-203">These functions can also be used to validate Polygons.</span></span> <span data-ttu-id="7d08b-204">Například tady používáme ST_ISVALIDDETAILED ověření mnohoúhelníku, který není uzavřený.</span><span class="sxs-lookup"><span data-stu-id="7d08b-204">For example, here we use ST_ISVALIDDETAILED to validate a Polygon that is not closed.</span></span> 

<span data-ttu-id="7d08b-205">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="7d08b-205">**Query**</span></span>

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

<span data-ttu-id="7d08b-206">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-206">**Results**</span></span>

    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]

### <a name="linq-querying-in-the-net-sdk"></a><span data-ttu-id="7d08b-207">LINQ dotazování v .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7d08b-207">LINQ Querying in the .NET SDK</span></span>
<span data-ttu-id="7d08b-208">Sadu DocumentDB .NET SDK také poskytovatelé zóny se zakázaným inzerováním metody `Distance()` a `Within()` pro použití v rámci LINQ – výrazy.</span><span class="sxs-lookup"><span data-stu-id="7d08b-208">The DocumentDB .NET SDK also providers stub methods `Distance()` and `Within()` for use within LINQ expressions.</span></span> <span data-ttu-id="7d08b-209">Zprostředkovatel DocumentDB LINQ překládá těchto volání metod na ekvivalentní integrovaná funkce volání SQL (ST_DISTANCE a ST_WITHIN v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="7d08b-209">The DocumentDB LINQ provider translates these method calls to the equivalent SQL built-in function calls (ST_DISTANCE and ST_WITHIN respectively).</span></span> 

<span data-ttu-id="7d08b-210">Tady je příklad dotazu LINQ, který najde všechny dokumenty v kolekci Azure Cosmos DB, jehož hodnota "umístění" je v rámci okruhu 30km zadaného bodu pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="7d08b-210">Here's an example of a LINQ query that finds all documents in the Azure Cosmos DB collection whose "location" value is within a radius of 30km of the specified point using LINQ.</span></span>

<span data-ttu-id="7d08b-211">**Dotaz LINQ pro vzdálenost**</span><span class="sxs-lookup"><span data-stu-id="7d08b-211">**LINQ query for Distance**</span></span>

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

<span data-ttu-id="7d08b-212">Podobně je zde dotazu pro vyhledání všech dokumentů, jejichž "umístění" je v rámci zadaného pole nebo mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="7d08b-212">Similarly, here's a query for finding all the documents whose "location" is within the specified box/Polygon.</span></span> 

<span data-ttu-id="7d08b-213">**LINQ dotazu v rámci**</span><span class="sxs-lookup"><span data-stu-id="7d08b-213">**LINQ query for Within**</span></span>

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


<span data-ttu-id="7d08b-214">Teď, když bylo podívejte se na postup dotazování dokumentů pomocí LINQ a SQL, Podívejme se na tom, jak nakonfigurovat databázi Cosmos Azure pro prostorových indexování.</span><span class="sxs-lookup"><span data-stu-id="7d08b-214">Now that we've taken a look at how to query documents using LINQ and SQL, let's take a look at how to configure Azure Cosmos DB for spatial indexing.</span></span>

## <a name="indexing"></a><span data-ttu-id="7d08b-215">Indexování</span><span class="sxs-lookup"><span data-stu-id="7d08b-215">Indexing</span></span>
<span data-ttu-id="7d08b-216">Jsme popsané v [schématu bez ohledu na indexování s Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) dokumentu, jsme určený databázový stroj databázi Azure Cosmos jako skutečně nezávislá na schéma a poskytovat prvotřídní podporu pro formát JSON.</span><span class="sxs-lookup"><span data-stu-id="7d08b-216">As we described in the [Schema Agnostic Indexing with Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paper, we designed Azure Cosmos DB’s database engine to be truly schema agnostic and provide first class support for JSON.</span></span> <span data-ttu-id="7d08b-217">Databázový stroj zápisu optimalizované Azure Cosmos databáze nativně rozumí prostorových dat (body, mnohoúhelníků a čar), které jsou v GeoJSON standard.</span><span class="sxs-lookup"><span data-stu-id="7d08b-217">The write optimized database engine of Azure Cosmos DB natively understands spatial data (points, Polygons and lines) represented in the GeoJSON standard.</span></span>

<span data-ttu-id="7d08b-218">Stručně řečeno, je geometrie projektovat z geodetické souřadnice na 2D rovinu pak progresivně rozdělené do buňky pomocí **quadtree**.</span><span class="sxs-lookup"><span data-stu-id="7d08b-218">In a nutshell, the geometry is projected from geodetic coordinates onto a 2D plane then divided progressively into cells using a **quadtree**.</span></span> <span data-ttu-id="7d08b-219">Tyto buněk jsou namapované na 1D na základě umístění buňky v rámci **Hilbertův místo naplnění křivky**, který zachovává polohu bodů.</span><span class="sxs-lookup"><span data-stu-id="7d08b-219">These cells are mapped to 1D based on the location of the cell within a **Hilbert space filling curve**, which preserves locality of points.</span></span> <span data-ttu-id="7d08b-220">Dále pokud je indexovaný data o umístění, prochází skrz tento proces se označuje jako **teselace**, tj. všechny buňky, které intersect umístění jsou identifikovány a uloží jako klíče v Azure Cosmos DB indexu.</span><span class="sxs-lookup"><span data-stu-id="7d08b-220">Additionally when location data is indexed, it goes through a process known as **tessellation**, i.e. all the cells that intersect a location are identified and stored as keys in the Azure Cosmos DB index.</span></span> <span data-ttu-id="7d08b-221">V době dotazů argumenty jako body a mnohoúhelníky jsou také teselace sestavy k extrakci oblasti relevantní buněk ID a potom použít k načtení dat z indexu.</span><span class="sxs-lookup"><span data-stu-id="7d08b-221">At query time, arguments like points and Polygons are also tessellated to extract the relevant cell ID ranges, then used to retrieve data from the index.</span></span>

<span data-ttu-id="7d08b-222">Pokud zadáte zásady indexování, zahrnující prostorový index pro / * (všechny cesty), pak všechny body nalezené v rámci kolekce jsou indexované pro efektivní prostorových dotazů (ST_WITHIN a ST_DISTANCE).</span><span class="sxs-lookup"><span data-stu-id="7d08b-222">If you specify an indexing policy that includes spatial index for /* (all paths), then all points found within the collection are indexed for efficient spatial queries (ST_WITHIN and ST_DISTANCE).</span></span> <span data-ttu-id="7d08b-223">Prostorové indexy nemáte hodnotu přesnost a vždy použít výchozí hodnotu přesnost.</span><span class="sxs-lookup"><span data-stu-id="7d08b-223">Spatial indexes do not have a precision value, and always use a default precision value.</span></span>

> [!NOTE]
> <span data-ttu-id="7d08b-224">Azure Cosmos DB podporuje automatické indexování bodů, mnohoúhelníky a LineStrings</span><span class="sxs-lookup"><span data-stu-id="7d08b-224">Azure Cosmos DB supports automatic indexing of Points, Polygons, and LineStrings</span></span>
> 
> 

<span data-ttu-id="7d08b-225">Následující fragment kódu JSON zobrazí zásady indexování s prostorových indexování povolena, tj. kdykoli GeoJSON v rámci dokumenty nalezen prostorových dotazování indexu.</span><span class="sxs-lookup"><span data-stu-id="7d08b-225">The following JSON snippet shows an indexing policy with spatial indexing enabled, i.e. index any GeoJSON point found within documents for spatial querying.</span></span> <span data-ttu-id="7d08b-226">Chcete-li změnit zásady indexování pomocí portálu Azure, můžete zadat následující JSON pro zásady indexování povolit prostorových indexování do kolekce.</span><span class="sxs-lookup"><span data-stu-id="7d08b-226">If you are modifying the indexing policy using the Azure Portal, you can specify the following JSON for indexing policy to enable spatial indexing on your collection.</span></span>

<span data-ttu-id="7d08b-227">**Kolekce JSON zásady indexování s Spatial povoleno pro body a mnohoúhelníky**</span><span class="sxs-lookup"><span data-stu-id="7d08b-227">**Collection Indexing Policy JSON with Spatial enabled for points and Polygons**</span></span>

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

<span data-ttu-id="7d08b-228">Zde je fragment kódu v rozhraní .NET, který ukazuje, jak můžete vytvořit kolekci s prostorových indexování zapnuta pro všechny cesty obsahující body.</span><span class="sxs-lookup"><span data-stu-id="7d08b-228">Here's a code snippet in .NET that shows how to create a collection with spatial indexing turned on for all paths containing points.</span></span> 

<span data-ttu-id="7d08b-229">**Vytvořte kolekci s prostorových indexování**</span><span class="sxs-lookup"><span data-stu-id="7d08b-229">**Create a collection with spatial indexing**</span></span>

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

<span data-ttu-id="7d08b-230">A tady je, jak můžete upravit existující kolekci využít prostorových indexování přes všechny body, které jsou uloženy v rámci dokumenty.</span><span class="sxs-lookup"><span data-stu-id="7d08b-230">And here's how you can modify an existing collection to take advantage of spatial indexing over any points that are stored within documents.</span></span>

<span data-ttu-id="7d08b-231">**Upravit existující kolekci s prostorových indexování**</span><span class="sxs-lookup"><span data-stu-id="7d08b-231">**Modify an existing collection with spatial indexing**</span></span>

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> <span data-ttu-id="7d08b-232">Pokud umístění GeoJSON hodnotu v tomto dokumentu je chybný nebo není platný, pak jej nebude získat indexovaný prostorových dotazování.</span><span class="sxs-lookup"><span data-stu-id="7d08b-232">If the location GeoJSON value within the document is malformed or invalid, then it will not get indexed for spatial querying.</span></span> <span data-ttu-id="7d08b-233">Můžete ověřit pomocí ST_ISVALID a ST_ISVALIDDETAILED hodnoty umístění.</span><span class="sxs-lookup"><span data-stu-id="7d08b-233">You can validate location values using ST_ISVALID and ST_ISVALIDDETAILED.</span></span>
> 
> <span data-ttu-id="7d08b-234">Pokud svou definici. kolekce obsahuje klíč oddílu, není hlášena indexování průběh transformace.</span><span class="sxs-lookup"><span data-stu-id="7d08b-234">If your collection definition includes a partition key, indexing transformation progress is not reported.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7d08b-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d08b-235">Next steps</span></span>
<span data-ttu-id="7d08b-236">Teď, když jste můžete dozvědět o tom, jak začít pracovat s geoprostorové podpory v Azure Cosmos DB, můžete:</span><span class="sxs-lookup"><span data-stu-id="7d08b-236">Now that you've learnt about how to get started with geospatial support in Azure Cosmos DB, you can:</span></span>

* <span data-ttu-id="7d08b-237">Psaní s [ukázky kódu .NET geoprostorové na Githubu](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="7d08b-237">Start coding with the [Geospatial .NET code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span></span>
* <span data-ttu-id="7d08b-238">Získat rukou na s geoprostorové dotazování na [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span><span class="sxs-lookup"><span data-stu-id="7d08b-238">Get hands on with geospatial querying at the [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span></span>
* <span data-ttu-id="7d08b-239">Další informace o [Azure Cosmos DB dotazu](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="7d08b-239">Learn more about [Azure Cosmos DB Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="7d08b-240">Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="7d08b-240">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>

