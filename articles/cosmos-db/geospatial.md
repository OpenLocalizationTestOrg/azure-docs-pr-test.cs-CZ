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
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a><span data-ttu-id="af5d6-103">Práce s geoprostorové a GeoJSON umístění dat v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="af5d6-103">Working with geospatial and GeoJSON location data in Azure Cosmos DB</span></span>
<span data-ttu-id="af5d6-104">Tento článek slouží Úvod toohello geoprostorové funkce v [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="af5d6-104">This article is an introduction toohello geospatial functionality in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="af5d6-105">Po přečtení to, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="af5d6-105">After reading this, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="af5d6-106">Jak ukládat prostorových dat v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="af5d6-106">How do I store spatial data in Azure Cosmos DB?</span></span>
* <span data-ttu-id="af5d6-107">Jak můžete dotazovat geoprostorové data v Azure DB Cosmos v SQL a LINQ?</span><span class="sxs-lookup"><span data-stu-id="af5d6-107">How can I query geospatial data in Azure Cosmos DB in SQL and LINQ?</span></span>
* <span data-ttu-id="af5d6-108">Jak povolit nebo zakázat prostorových indexování v Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="af5d6-108">How do I enable or disable spatial indexing in Azure Cosmos DB?</span></span>

<span data-ttu-id="af5d6-109">Tento článek ukazuje, jak toowork s prostorovými daty formátu s hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="af5d6-109">This article shows how toowork with spatial data with hello DocumentDB API.</span></span> <span data-ttu-id="af5d6-110">Najdete v tématu to [Githubu projektu](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) pro ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="af5d6-110">Please see this [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) for code samples.</span></span>

## <a name="introduction-toospatial-data"></a><span data-ttu-id="af5d6-111">Úvod toospatial dat</span><span class="sxs-lookup"><span data-stu-id="af5d6-111">Introduction toospatial data</span></span>
<span data-ttu-id="af5d6-112">Prostorová data popisuje hello pozice a tvar objektů v prostoru.</span><span class="sxs-lookup"><span data-stu-id="af5d6-112">Spatial data describes hello position and shape of objects in space.</span></span> <span data-ttu-id="af5d6-113">Ve většině aplikací se tyto odpovídají tooobjects na zemi, hello, tj. geoprostorové data.</span><span class="sxs-lookup"><span data-stu-id="af5d6-113">In most applications, these correspond tooobjects on hello earth, i.e. geospatial data.</span></span> <span data-ttu-id="af5d6-114">Prostorová data můžou být použité toorepresent hello umístění osoby, místo zájmu nebo hranic hello města nebo jezero.</span><span class="sxs-lookup"><span data-stu-id="af5d6-114">Spatial data can be used toorepresent hello location of a person, a place of interest, or hello boundary of a city, or a lake.</span></span> <span data-ttu-id="af5d6-115">Běžné případy použití často zahrnuje blízkosti dotazy pro například "najít všechny v kavárnách téměř Moje aktuální umístění".</span><span class="sxs-lookup"><span data-stu-id="af5d6-115">Common use cases often involve proximity queries, for e.g., "find all coffee shops near my current location".</span></span> 

### <a name="geojson"></a><span data-ttu-id="af5d6-116">GeoJSON</span><span class="sxs-lookup"><span data-stu-id="af5d6-116">GeoJSON</span></span>
<span data-ttu-id="af5d6-117">Podporuje Azure Cosmos DB indexování a dotazování dat geoprostorové bodu, která je reprezentována pomocí hello [GeoJSON specifikace](https://tools.ietf.org/html/rfc7946).</span><span class="sxs-lookup"><span data-stu-id="af5d6-117">Azure Cosmos DB supports indexing and querying of geospatial point data that's represented using hello [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span></span> <span data-ttu-id="af5d6-118">GeoJSON datové struktury, jsou vždy objekty JSON je platný, a tak, aby se mohou být uloženy a dotazovat pomocí Azure Cosmos DB bez jakýchkoli specializovaných nástrojů nebo knihovny.</span><span class="sxs-lookup"><span data-stu-id="af5d6-118">GeoJSON data structures are always valid JSON objects, so they can be stored and queried using Azure Cosmos DB without any specialized tools or libraries.</span></span> <span data-ttu-id="af5d6-119">Hello SDK služby Azure Cosmos DB zadejte pomocné rutiny třídy a metody, které umožňují snadno toowork s prostorovými daty formátu.</span><span class="sxs-lookup"><span data-stu-id="af5d6-119">hello Azure Cosmos DB SDKs provide helper classes and methods that make it easy toowork with spatial data.</span></span> 

### <a name="points-linestrings-and-polygons"></a><span data-ttu-id="af5d6-120">Body, LineStrings a mnohoúhelníky</span><span class="sxs-lookup"><span data-stu-id="af5d6-120">Points, LineStrings and Polygons</span></span>
<span data-ttu-id="af5d6-121">A **bodu** označuje jeden pozice v prostoru.</span><span class="sxs-lookup"><span data-stu-id="af5d6-121">A **Point** denotes a single position in space.</span></span> <span data-ttu-id="af5d6-122">V datech geoprostorové představuje bod hello přesné umístění, které by mohly být adresu supermarketu úložiště, celoobrazovkovém režimu, automobilu nebo města.</span><span class="sxs-lookup"><span data-stu-id="af5d6-122">In geospatial data, a Point represents hello exact location, which could be a street address of a grocery store, a kiosk, an automobile or a city.</span></span>  <span data-ttu-id="af5d6-123">Bod je reprezentována v páru GeoJSON (a Azure Cosmos DB) pomocí jeho souřadnice nebo zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="af5d6-123">A point is represented in GeoJSON (and Azure Cosmos DB) using its coordinate pair or longitude and latitude.</span></span> <span data-ttu-id="af5d6-124">Tady je příklad JSON pro bod.</span><span class="sxs-lookup"><span data-stu-id="af5d6-124">Here's an example JSON for a point.</span></span>

<span data-ttu-id="af5d6-125">**Body v Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="af5d6-125">**Points in Azure Cosmos DB**</span></span>

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> <span data-ttu-id="af5d6-126">Určuje Hello GeoJSON specifikace délky první a zeměpisnou šířku druhý.</span><span class="sxs-lookup"><span data-stu-id="af5d6-126">hello GeoJSON specification specifies longitude first and latitude second.</span></span> <span data-ttu-id="af5d6-127">Podobně jako v ostatních aplikacích mapování zeměpisné šířky a délky jsou úhly a uvádí stupňů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-127">Like in other mapping applications, longitude and latitude are angles and represented in terms of degrees.</span></span> <span data-ttu-id="af5d6-128">Hodnoty zeměpisné délky se měří od základního poledníku hello a jsou v rozmezí od -180 a 180.0 stupňů a zeměpisnou šířku hodnoty jsou v měřeny od rovníku hello a jsou mezi-90.0 a 90.0 stupňů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-128">Longitude values are measured from hello Prime Meridian and are between -180 and 180.0 degrees, and latitude values are measured from hello equator and are between -90.0 and 90.0 degrees.</span></span> 
> 
> <span data-ttu-id="af5d6-129">Azure Cosmos DB interpretuje souřadnice reprezentovaný za hello WGS 84 referenční systém.</span><span class="sxs-lookup"><span data-stu-id="af5d6-129">Azure Cosmos DB interprets coordinates as represented per hello WGS-84 reference system.</span></span> <span data-ttu-id="af5d6-130">Níže naleznete podrobnosti o souřadnic referenčních systémů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-130">Please see below for more details about coordinate reference systems.</span></span>
> 
> 

<span data-ttu-id="af5d6-131">To může být vložen do dokument Azure Cosmos DB, jak je znázorněno v tomto příkladu obsahující data o umístění profilu uživatele:</span><span class="sxs-lookup"><span data-stu-id="af5d6-131">This can be embedded in an Azure Cosmos DB document as shown in this example of a user profile containing location data:</span></span>

<span data-ttu-id="af5d6-132">**Použít profil s umístěním, které jsou uloženy v databázi Cosmos Azure**</span><span class="sxs-lookup"><span data-stu-id="af5d6-132">**Use Profile with Location stored in Azure Cosmos DB**</span></span>

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

<span data-ttu-id="af5d6-133">Kromě toho toopoints, GeoJSON také podporuje LineStrings a mnohoúhelníky.</span><span class="sxs-lookup"><span data-stu-id="af5d6-133">In addition toopoints, GeoJSON also supports LineStrings and Polygons.</span></span> <span data-ttu-id="af5d6-134">**LineStrings** představují řadu dva nebo víc bodů v prostoru a hello segmenty čáry, která se připojují, je.</span><span class="sxs-lookup"><span data-stu-id="af5d6-134">**LineStrings** represent a series of two or more points in space and hello line segments that connect them.</span></span> <span data-ttu-id="af5d6-135">V datech geoprostorové LineStrings jsou běžně používané toorepresent dálnice nebo řek.</span><span class="sxs-lookup"><span data-stu-id="af5d6-135">In geospatial data, LineStrings are commonly used toorepresent highways or rivers.</span></span> <span data-ttu-id="af5d6-136">A **mnohoúhelníku** je hranice připojené body, která tvoří uzavřené LineString.</span><span class="sxs-lookup"><span data-stu-id="af5d6-136">A **Polygon** is a boundary of connected points that forms a closed LineString.</span></span> <span data-ttu-id="af5d6-137">Mnohoúhelníky jsou běžně používané toorepresent přirozené vytváření jako jezera nebo politickou oblasti jurisdikce jako města a stavy.</span><span class="sxs-lookup"><span data-stu-id="af5d6-137">Polygons are commonly used toorepresent natural formations like lakes or political jurisdictions like cities and states.</span></span> <span data-ttu-id="af5d6-138">Tady je příklad mnohoúhelníku v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="af5d6-138">Here's an example of a Polygon in Azure Cosmos DB.</span></span> 

<span data-ttu-id="af5d6-139">**Mnohoúhelníky v GeoJSON**</span><span class="sxs-lookup"><span data-stu-id="af5d6-139">**Polygons in GeoJSON**</span></span>

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
> <span data-ttu-id="af5d6-140">Hello GeoJSON specifikace vyžaduje, aby pro platný mnohoúhelníky hello poslední souřadnic pár poskytuje hello stejné jako první, toocreate hello uzavřený obrazec.</span><span class="sxs-lookup"><span data-stu-id="af5d6-140">hello GeoJSON specification requires that for valid Polygons, hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span>
> 
> <span data-ttu-id="af5d6-141">Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="af5d6-141">Points within a Polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="af5d6-142">Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje hello inverzní oblasti hello v něm.</span><span class="sxs-lookup"><span data-stu-id="af5d6-142">A Polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>
> 
> 

<span data-ttu-id="af5d6-143">V přidání tooPoint, LineString a mnohoúhelníku, GeoJSON také určuje vyjádření hello jak toogroup víc Geoprostorové umístění, jak dobře tooassociate libovolné vlastnosti s informace o zeměpisné poloze jako **funkce**.</span><span class="sxs-lookup"><span data-stu-id="af5d6-143">In addition tooPoint, LineString and Polygon, GeoJSON also specifies hello representation for how toogroup multiple geospatial locations, as well as how tooassociate arbitrary properties with geolocation as a **Feature**.</span></span> <span data-ttu-id="af5d6-144">Vzhledem k tomu, že tyto objekty jsou platný kód JSON, všechny jde uloženy a zpracovány v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="af5d6-144">Since these objects are valid JSON, they can all be stored and processed in Azure Cosmos DB.</span></span> <span data-ttu-id="af5d6-145">Ale Azure Cosmos DB podporuje pouze automatické indexování bodů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-145">However Azure Cosmos DB only supports automatic indexing of points.</span></span>

### <a name="coordinate-reference-systems"></a><span data-ttu-id="af5d6-146">Koordinaci referenčních systémů</span><span class="sxs-lookup"><span data-stu-id="af5d6-146">Coordinate reference systems</span></span>
<span data-ttu-id="af5d6-147">Vzhledem k tomu, že tvar hello hello earth je nestandardní, je reprezentována souřadnice geoprostorové data v mnoha systémy souřadnic odkaz (CRS), každou s vlastní referenční rámce a měrné jednotky.</span><span class="sxs-lookup"><span data-stu-id="af5d6-147">Since hello shape of hello earth is irregular, coordinates of geospatial data is represented in many coordinate reference systems (CRS), each with their own frames of reference and units of measurement.</span></span> <span data-ttu-id="af5d6-148">Například hello "National mřížky Británie" je referenční systém je velmi přesná hello Spojené království, ale ne mimo něj.</span><span class="sxs-lookup"><span data-stu-id="af5d6-148">For example, hello "National Grid of Britain" is a reference system is very accurate for hello United Kingdom, but not outside it.</span></span> 

<span data-ttu-id="af5d6-149">Hello nejoblíbenější řádku používá v současné době jsou hello World geodetické systému [WGS 84](http://earth-info.nga.mil/GandG/wgs84/).</span><span class="sxs-lookup"><span data-stu-id="af5d6-149">hello most popular CRS in use today is hello World Geodetic System  [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span></span> <span data-ttu-id="af5d6-150">GPS zařízení a velký počet mapování služby včetně mapy Google a rozhraní API map Bing pomocí WGS 84.</span><span class="sxs-lookup"><span data-stu-id="af5d6-150">GPS devices, and many mapping services including Google Maps and Bing Maps APIs use WGS-84.</span></span> <span data-ttu-id="af5d6-151">Azure Cosmos DB podporuje indexování a dotazování dat geoprostorové pouze hello WGS 84 řádku.</span><span class="sxs-lookup"><span data-stu-id="af5d6-151">Azure Cosmos DB supports indexing and querying of geospatial data using hello WGS-84 CRS only.</span></span> 

## <a name="creating-documents-with-spatial-data"></a><span data-ttu-id="af5d6-152">Vytváření dokumentů s prostorovými daty</span><span class="sxs-lookup"><span data-stu-id="af5d6-152">Creating documents with spatial data</span></span>
<span data-ttu-id="af5d6-153">Při vytváření dokumentů, které obsahují GeoJSON hodnoty jsou automaticky indexovány s prostorového indexu v souladu zásady indexování toohello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="af5d6-153">When you create documents that contain GeoJSON values, they are automatically indexed with a spatial index in accordance toohello indexing policy of hello collection.</span></span> <span data-ttu-id="af5d6-154">Pokud pracujete se sadou Azure SDK DB Cosmos v jazyce dynamicky zadávaných jako Pythonu nebo Node.js, musíte vytvořit platný GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="af5d6-154">If you're working with an Azure Cosmos DB SDK in a dynamically typed language like Python or Node.js, you must create valid GeoJSON.</span></span>

<span data-ttu-id="af5d6-155">**Vytvořte dokument s daty geoprostorové v Node.js**</span><span class="sxs-lookup"><span data-stu-id="af5d6-155">**Create Document with Geospatial data in Node.js**</span></span>

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

<span data-ttu-id="af5d6-156">Pokud pracujete s hello DocumentDB rozhraní API, můžete použít hello `Point` a `Polygon` tříd v rámci hello `Microsoft.Azure.Documents.Spatial` informace o umístění tooembed oboru názvů v rámci vašich objektů aplikace.</span><span class="sxs-lookup"><span data-stu-id="af5d6-156">If you're working with hello DocumentDB APIs, you can use hello `Point` and `Polygon` classes within hello `Microsoft.Azure.Documents.Spatial` namespace tooembed location information within your application objects.</span></span> <span data-ttu-id="af5d6-157">Tyto třídy zjednodušit hello serializace a deserializace prostorových dat do GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="af5d6-157">These classes help simplify hello serialization and deserialization of spatial data into GeoJSON.</span></span>

<span data-ttu-id="af5d6-158">**Vytvořte dokument s daty geoprostorové v rozhraní .NET**</span><span class="sxs-lookup"><span data-stu-id="af5d6-158">**Create Document with Geospatial data in .NET**</span></span>

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

<span data-ttu-id="af5d6-159">Pokud nemáte hello zeměpisnou šířku a délku informace, ale název umístění jako města nebo země nebo hello fyzické adresy, můžete vyhledat skutečné souřadnice hello pomocí služeb určování zeměpisných souřadnic jako služby REST Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="af5d6-159">If you don't have hello latitude and longitude information, but have hello physical addresses or location name like city or country, you can look up hello actual coordinates by using a geocoding service like Bing Maps REST Services.</span></span> <span data-ttu-id="af5d6-160">Další informace o určování zeměpisných souřadnic mapy Bing [zde](https://msdn.microsoft.com/library/ff701713.aspx).</span><span class="sxs-lookup"><span data-stu-id="af5d6-160">Learn more about Bing Maps geocoding [here](https://msdn.microsoft.com/library/ff701713.aspx).</span></span>

## <a name="querying-spatial-types"></a><span data-ttu-id="af5d6-161">Dotazování prostorové typy</span><span class="sxs-lookup"><span data-stu-id="af5d6-161">Querying spatial types</span></span>
<span data-ttu-id="af5d6-162">Teď, podívejte se na to, jak bylo tooinsert geoprostorové data, Podívejme se na to, jak tooquery tato data pomocí Azure DB Cosmos pomocí SQL a LINQ.</span><span class="sxs-lookup"><span data-stu-id="af5d6-162">Now that we've taken a look at how tooinsert geospatial data, let's take a look at how tooquery this data using Azure Cosmos DB using SQL and LINQ.</span></span>

### <a name="spatial-sql-built-in-functions"></a><span data-ttu-id="af5d6-163">Prostorové integrované funkce SQL</span><span class="sxs-lookup"><span data-stu-id="af5d6-163">Spatial SQL built-in functions</span></span>
<span data-ttu-id="af5d6-164">Azure Cosmos DB podporuje následující otevřete geoprostorové Consortium (OGC) integrované funkce pro dotazování geoprostorové hello.</span><span class="sxs-lookup"><span data-stu-id="af5d6-164">Azure Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> <span data-ttu-id="af5d6-165">Další informace o hello kompletní sadu integrovaných funkcí v hello jazyk SQL naleznete příliš[dotazu Azure Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="af5d6-165">For more details on hello complete set of built-in functions in hello SQL language, please refer too[Query Azure Cosmos DB](documentdb-sql-query.md).</span></span>

<table>
<tr>
  <td><span data-ttu-id="af5d6-166"><strong>Využití</strong></span><span class="sxs-lookup"><span data-stu-id="af5d6-166"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="af5d6-167"><strong>Popis</strong></span><span class="sxs-lookup"><span data-stu-id="af5d6-167"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="af5d6-168">ST_DISTANCE (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="af5d6-168">ST_DISTANCE (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="af5d6-169">Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-169">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="af5d6-170">ST_WITHIN (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="af5d6-170">ST_WITHIN (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="af5d6-171">Vrací výraz logická hodnota určující, zda text hello první GeoJSON objekt (bod, mnohoúhelníku nebo LineString) je v rámci hello druhý GeoJSON objekt (bod, mnohoúhelníku nebo LineString).</span><span class="sxs-lookup"><span data-stu-id="af5d6-171">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="af5d6-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="af5d6-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="af5d6-173">Vrátí hodnotu označující, zda text hello dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</span><span class="sxs-lookup"><span data-stu-id="af5d6-173">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="af5d6-174">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="af5d6-174">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="af5d6-175">Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.</span><span class="sxs-lookup"><span data-stu-id="af5d6-175">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="af5d6-176">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="af5d6-176">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="af5d6-177">Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="af5d6-177">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="af5d6-178">Prostorové funkce můžou být použité tooperform blízkosti dotazy pro prostorová data.</span><span class="sxs-lookup"><span data-stu-id="af5d6-178">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="af5d6-179">Tady je příklad dotaz, který vrátí že všechny rodiny dokumenty, v rámci 30 km hello zadané umístění používáte hello ST_DISTANCE integrovaná funkce.</span><span class="sxs-lookup"><span data-stu-id="af5d6-179">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="af5d6-180">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="af5d6-180">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="af5d6-181">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-181">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="af5d6-182">Pokud zahrnete prostorových indexování v zásady indexování, pak "vzdálenost dotazy" se zpracuje efektivně prostřednictvím hello index.</span><span class="sxs-lookup"><span data-stu-id="af5d6-182">If you include spatial indexing in your indexing policy, then "distance queries" will be served efficiently through hello index.</span></span> <span data-ttu-id="af5d6-183">Další informace o prostorových indexování najdete v tématu hello části.</span><span class="sxs-lookup"><span data-stu-id="af5d6-183">For more details on spatial indexing, please see hello section below.</span></span> <span data-ttu-id="af5d6-184">Pokud nemáte prostorový index pro hello zadané cesty, můžete přesto provést prostorových dotazů zadáním `x-ms-documentdb-query-enable-scan` hlavička požadavku s hodnotou hello nastavit příliš "true".</span><span class="sxs-lookup"><span data-stu-id="af5d6-184">If you don't have a spatial index for hello specified paths, you can still perform spatial queries by specifying `x-ms-documentdb-query-enable-scan` request header with hello value set too"true".</span></span> <span data-ttu-id="af5d6-185">V rozhraní .NET, to lze provést pomocí předání hello volitelné **FeedOptions** tooqueries argument s [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) nastavit tootrue.</span><span class="sxs-lookup"><span data-stu-id="af5d6-185">In .NET, this can be done by passing hello optional **FeedOptions** argument tooqueries with [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) set tootrue.</span></span> 

<span data-ttu-id="af5d6-186">ST_WITHIN může být použité toocheck, pokud bod leží uvnitř mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="af5d6-186">ST_WITHIN can be used toocheck if a point lies within a Polygon.</span></span> <span data-ttu-id="af5d6-187">Mnohoúhelníky jsou běžně používané toorepresent hranice jako PSČ, hranice stavu nebo přírodní vytváření.</span><span class="sxs-lookup"><span data-stu-id="af5d6-187">Commonly Polygons are used toorepresent boundaries like zip codes, state boundaries, or natural formations.</span></span> <span data-ttu-id="af5d6-188">Znovu Pokud zahrnete prostorových indexování v zásady indexování, pak "v" dotazy se zpracuje efektivně prostřednictvím hello index.</span><span class="sxs-lookup"><span data-stu-id="af5d6-188">Again if you include spatial indexing in your indexing policy, then "within" queries will be served efficiently through hello index.</span></span> 

<span data-ttu-id="af5d6-189">Argumenty mnohoúhelníku v ST_WITHIN může obsahovat pouze jedno zazvonění, tj. hello mnohoúhelníky nesmí obsahovat mezery v nich.</span><span class="sxs-lookup"><span data-stu-id="af5d6-189">Polygon arguments in ST_WITHIN can contain only a single ring, i.e. hello Polygons must not contain holes in them.</span></span> 

<span data-ttu-id="af5d6-190">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="af5d6-190">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

<span data-ttu-id="af5d6-191">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-191">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> <span data-ttu-id="af5d6-192">Podobné typy toohow neshoda funguje v Azure Cosmos DB dotazu, pokud hodnota hello umístění v zadána buď argument je chybný nebo není platný, pak vyhodnotí příliš**nedefinované** a toobe dokumentu hello vyhodnotit přeskočena z hello výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="af5d6-192">Similar toohow mismatched types works in Azure Cosmos DB query, if hello location value specified in either argument is malformed or invalid, then it will evaluate too**undefined** and hello evaluated document toobe skipped from hello query results.</span></span> <span data-ttu-id="af5d6-193">Pokud dotaz vrátí žádné výsledky, spusťte ST_ISVALIDDETAILED toodebug proč hello spatail typ je neplatný.</span><span class="sxs-lookup"><span data-stu-id="af5d6-193">If your query returns no results, run ST_ISVALIDDETAILED toodebug why hello spatail type is invalid.</span></span>     
> 
> 

<span data-ttu-id="af5d6-194">Azure Cosmos DB také podporuje provádění inverzní dotazy, tj. můžete index mnohoúhelníky nebo řádků v databázi Cosmos Azure a pak dotazu pro hello oblasti, které obsahují zadaný bod.</span><span class="sxs-lookup"><span data-stu-id="af5d6-194">Azure Cosmos DB also supports performing inverse queries, i.e. you can index Polygons or lines in Azure Cosmos DB, then query for hello areas that contain a specified point.</span></span> <span data-ttu-id="af5d6-195">Tento vzor se často používá v logistiky tooidentify například když vůz zadá nebo opustí určené oblasti.</span><span class="sxs-lookup"><span data-stu-id="af5d6-195">This pattern is commonly used in logistics tooidentify e.g. when a truck enters or leaves a designated area.</span></span> 

<span data-ttu-id="af5d6-196">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="af5d6-196">**Query**</span></span>

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


<span data-ttu-id="af5d6-197">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-197">**Results**</span></span>

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

<span data-ttu-id="af5d6-198">ST_ISVALID a ST_ISVALIDDETAILED můžou být použité toocheck, pokud prostorový objekt není platný.</span><span class="sxs-lookup"><span data-stu-id="af5d6-198">ST_ISVALID and ST_ISVALIDDETAILED can be used toocheck if a spatial object is valid.</span></span> <span data-ttu-id="af5d6-199">Například hello následující dotaz ověří platnost hello bod s out hodnoty zeměpisné šířky rozsah (-132.8).</span><span class="sxs-lookup"><span data-stu-id="af5d6-199">For example, hello following query checks hello validity of a point with an out of range latitude value (-132.8).</span></span> <span data-ttu-id="af5d6-200">ST_ISVALID vrací pouze logickou hodnotu, a vrátí ST_ISVALIDDETAILED hello řetězec obsahující hello důvod, proč má se za neplatný a logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="af5d6-200">ST_ISVALID returns just a Boolean value, and ST_ISVALIDDETAILED returns hello Boolean and a string containing hello reason why it is considered invalid.</span></span>

<span data-ttu-id="af5d6-201">** Dotaz **</span><span class="sxs-lookup"><span data-stu-id="af5d6-201">** Query **</span></span>

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

<span data-ttu-id="af5d6-202">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-202">**Results**</span></span>

    [{
      "$1": false
    }]

<span data-ttu-id="af5d6-203">Tyto funkce lze také použít toovalidate mnohoúhelníky.</span><span class="sxs-lookup"><span data-stu-id="af5d6-203">These functions can also be used toovalidate Polygons.</span></span> <span data-ttu-id="af5d6-204">Například tady používáme ST_ISVALIDDETAILED toovalidate mnohoúhelníku, který není uzavřený.</span><span class="sxs-lookup"><span data-stu-id="af5d6-204">For example, here we use ST_ISVALIDDETAILED toovalidate a Polygon that is not closed.</span></span> 

<span data-ttu-id="af5d6-205">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="af5d6-205">**Query**</span></span>

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

<span data-ttu-id="af5d6-206">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-206">**Results**</span></span>

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a><span data-ttu-id="af5d6-207">Dotazy LINQ v hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="af5d6-207">LINQ Querying in hello .NET SDK</span></span>
<span data-ttu-id="af5d6-208">Hello DocumentDB .NET SDK také poskytovatelé zóny se zakázaným inzerováním metody `Distance()` a `Within()` pro použití v rámci LINQ – výrazy.</span><span class="sxs-lookup"><span data-stu-id="af5d6-208">hello DocumentDB .NET SDK also providers stub methods `Distance()` and `Within()` for use within LINQ expressions.</span></span> <span data-ttu-id="af5d6-209">Hello DocumentDB LINQ zprostředkovatele znamená, že je tato metoda volání toohello ekvivalentní integrovaná funkce volání SQL (ST_DISTANCE a ST_WITHIN v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="af5d6-209">hello DocumentDB LINQ provider translates these method calls toohello equivalent SQL built-in function calls (ST_DISTANCE and ST_WITHIN respectively).</span></span> 

<span data-ttu-id="af5d6-210">Zde uvádíme příklad dotazu LINQ, který vyhledá všechny dokumenty v kolekci Azure Cosmos DB hello, jehož hodnota "umístění" je v rámci okruhu 30km hello zadána bodu pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="af5d6-210">Here's an example of a LINQ query that finds all documents in hello Azure Cosmos DB collection whose "location" value is within a radius of 30km of hello specified point using LINQ.</span></span>

<span data-ttu-id="af5d6-211">**Dotaz LINQ pro vzdálenost**</span><span class="sxs-lookup"><span data-stu-id="af5d6-211">**LINQ query for Distance**</span></span>

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

<span data-ttu-id="af5d6-212">Podobně je zde dotazu pro vyhledání všech hello dokumentů, jejichž "umístění" je v rámci hello zadaná pole nebo mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="af5d6-212">Similarly, here's a query for finding all hello documents whose "location" is within hello specified box/Polygon.</span></span> 

<span data-ttu-id="af5d6-213">**LINQ dotazu v rámci**</span><span class="sxs-lookup"><span data-stu-id="af5d6-213">**LINQ query for Within**</span></span>

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


<span data-ttu-id="af5d6-214">Teď, podívejte se na to, jak bylo tooquery dokumentů pomocí LINQ a SQL, Podívejme se na to, jak tooconfigure Azure Cosmos DB pro prostorových indexování.</span><span class="sxs-lookup"><span data-stu-id="af5d6-214">Now that we've taken a look at how tooquery documents using LINQ and SQL, let's take a look at how tooconfigure Azure Cosmos DB for spatial indexing.</span></span>

## <a name="indexing"></a><span data-ttu-id="af5d6-215">Indexování</span><span class="sxs-lookup"><span data-stu-id="af5d6-215">Indexing</span></span>
<span data-ttu-id="af5d6-216">Jsme popsané v hello [schématu bez ohledu na indexování s Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papír, jsme chtěli toobe modul databáze Azure Cosmos DB skutečně nezávislá na schéma a poskytovat prvotřídní podporu pro formát JSON.</span><span class="sxs-lookup"><span data-stu-id="af5d6-216">As we described in hello [Schema Agnostic Indexing with Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paper, we designed Azure Cosmos DB’s database engine toobe truly schema agnostic and provide first class support for JSON.</span></span> <span data-ttu-id="af5d6-217">databázový stroj zápisu optimalizované Hello Azure Cosmos databáze nativně rozumí prostorových dat (body, mnohoúhelníků a čar) v hello GeoJSON standard.</span><span class="sxs-lookup"><span data-stu-id="af5d6-217">hello write optimized database engine of Azure Cosmos DB natively understands spatial data (points, Polygons and lines) represented in hello GeoJSON standard.</span></span>

<span data-ttu-id="af5d6-218">Stručně řečeno, je hello geometrie projektovat z geodetické souřadnice na 2D rovinu pak progresivně rozdělené do buňky pomocí **quadtree**.</span><span class="sxs-lookup"><span data-stu-id="af5d6-218">In a nutshell, hello geometry is projected from geodetic coordinates onto a 2D plane then divided progressively into cells using a **quadtree**.</span></span> <span data-ttu-id="af5d6-219">Tyto buňky jsou namapované too1D podle umístění hello hello buňky v rámci **Hilbertův místo naplnění křivky**, který zachovává polohu bodů.</span><span class="sxs-lookup"><span data-stu-id="af5d6-219">These cells are mapped too1D based on hello location of hello cell within a **Hilbert space filling curve**, which preserves locality of points.</span></span> <span data-ttu-id="af5d6-220">Dále pokud je indexovaný data o umístění, prochází skrz tento proces se označuje jako **teselace**, tj. všechny hello buněk, které intersect umístění jsou identifikovat a uloží jako klíče v Azure Cosmos DB indexu hello.</span><span class="sxs-lookup"><span data-stu-id="af5d6-220">Additionally when location data is indexed, it goes through a process known as **tessellation**, i.e. all hello cells that intersect a location are identified and stored as keys in hello Azure Cosmos DB index.</span></span> <span data-ttu-id="af5d6-221">V době dotazů argumenty jako body a mnohoúhelníky jsou také teselace sestavy tooextract hello oblastí relevantní buněk ID a potom použít tooretrieve data z indexu hello.</span><span class="sxs-lookup"><span data-stu-id="af5d6-221">At query time, arguments like points and Polygons are also tessellated tooextract hello relevant cell ID ranges, then used tooretrieve data from hello index.</span></span>

<span data-ttu-id="af5d6-222">Pokud zadáte zásady indexování, zahrnující prostorový index pro / * (všechny cesty), pak všechny body v rámci kolekce hello nalezen jsou indexované pro efektivní prostorových dotazů (ST_WITHIN a ST_DISTANCE).</span><span class="sxs-lookup"><span data-stu-id="af5d6-222">If you specify an indexing policy that includes spatial index for /* (all paths), then all points found within hello collection are indexed for efficient spatial queries (ST_WITHIN and ST_DISTANCE).</span></span> <span data-ttu-id="af5d6-223">Prostorové indexy nemáte hodnotu přesnost a vždy použít výchozí hodnotu přesnost.</span><span class="sxs-lookup"><span data-stu-id="af5d6-223">Spatial indexes do not have a precision value, and always use a default precision value.</span></span>

> [!NOTE]
> <span data-ttu-id="af5d6-224">Azure Cosmos DB podporuje automatické indexování bodů, mnohoúhelníky a LineStrings</span><span class="sxs-lookup"><span data-stu-id="af5d6-224">Azure Cosmos DB supports automatic indexing of Points, Polygons, and LineStrings</span></span>
> 
> 

<span data-ttu-id="af5d6-225">Hello následující fragment kódu JSON zobrazí zásady indexování s prostorových indexování povolena, tj. kdykoli GeoJSON v rámci dokumenty nalezen prostorových dotazování indexu.</span><span class="sxs-lookup"><span data-stu-id="af5d6-225">hello following JSON snippet shows an indexing policy with spatial indexing enabled, i.e. index any GeoJSON point found within documents for spatial querying.</span></span> <span data-ttu-id="af5d6-226">Chcete-li změnit hello indexování zásady pomocí hello portálu Azure, můžete zadat hello následující JSON pro indexování zásad tooenable prostorových indexování do kolekce.</span><span class="sxs-lookup"><span data-stu-id="af5d6-226">If you are modifying hello indexing policy using hello Azure Portal, you can specify hello following JSON for indexing policy tooenable spatial indexing on your collection.</span></span>

<span data-ttu-id="af5d6-227">**Kolekce JSON zásady indexování s Spatial povoleno pro body a mnohoúhelníky**</span><span class="sxs-lookup"><span data-stu-id="af5d6-227">**Collection Indexing Policy JSON with Spatial enabled for points and Polygons**</span></span>

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

<span data-ttu-id="af5d6-228">Zde je fragment kódu v rozhraní .NET, který ukazuje, jak toocreate kolekce se prostorových indexování zapnuté pro všechny cesty obsahující body.</span><span class="sxs-lookup"><span data-stu-id="af5d6-228">Here's a code snippet in .NET that shows how toocreate a collection with spatial indexing turned on for all paths containing points.</span></span> 

<span data-ttu-id="af5d6-229">**Vytvořte kolekci s prostorových indexování**</span><span class="sxs-lookup"><span data-stu-id="af5d6-229">**Create a collection with spatial indexing**</span></span>

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

<span data-ttu-id="af5d6-230">A tady je, jak můžete upravit existující kolekci tootake výhodou prostorových indexování přes všechny body, které jsou uloženy v rámci dokumenty.</span><span class="sxs-lookup"><span data-stu-id="af5d6-230">And here's how you can modify an existing collection tootake advantage of spatial indexing over any points that are stored within documents.</span></span>

<span data-ttu-id="af5d6-231">**Upravit existující kolekci s prostorových indexování**</span><span class="sxs-lookup"><span data-stu-id="af5d6-231">**Modify an existing collection with spatial indexing**</span></span>

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
> <span data-ttu-id="af5d6-232">Pokud umístění hello GeoJSON hodnotu v dokumentu hello je chybný nebo není platný, pak jej nebude získat indexovaný prostorových dotazování.</span><span class="sxs-lookup"><span data-stu-id="af5d6-232">If hello location GeoJSON value within hello document is malformed or invalid, then it will not get indexed for spatial querying.</span></span> <span data-ttu-id="af5d6-233">Můžete ověřit pomocí ST_ISVALID a ST_ISVALIDDETAILED hodnoty umístění.</span><span class="sxs-lookup"><span data-stu-id="af5d6-233">You can validate location values using ST_ISVALID and ST_ISVALIDDETAILED.</span></span>
> 
> <span data-ttu-id="af5d6-234">Pokud svou definici. kolekce obsahuje klíč oddílu, není hlášena indexování průběh transformace.</span><span class="sxs-lookup"><span data-stu-id="af5d6-234">If your collection definition includes a partition key, indexing transformation progress is not reported.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="af5d6-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af5d6-235">Next steps</span></span>
<span data-ttu-id="af5d6-236">Teď, když jste dozvědí o tom, jak začít tooget s podporou geoprostorové v Azure Cosmos DB, můžete:</span><span class="sxs-lookup"><span data-stu-id="af5d6-236">Now that you've learnt about how tooget started with geospatial support in Azure Cosmos DB, you can:</span></span>

* <span data-ttu-id="af5d6-237">Psaní s hello [ukázky kódu .NET geoprostorové na Githubu](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="af5d6-237">Start coding with hello [Geospatial .NET code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span></span>
* <span data-ttu-id="af5d6-238">Získat rukou na s geoprostorové dotazování na hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span><span class="sxs-lookup"><span data-stu-id="af5d6-238">Get hands on with geospatial querying at hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span></span>
* <span data-ttu-id="af5d6-239">Další informace o [Azure Cosmos DB dotazu](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="af5d6-239">Learn more about [Azure Cosmos DB Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="af5d6-240">Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="af5d6-240">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>

