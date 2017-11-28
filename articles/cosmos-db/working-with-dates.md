---
title: "Práce s daty v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o postupu při práci s daty v Azure Cosmos DB."
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: b6a77e33eea24000037ffb31d7aae3cb1d345ce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="7d797-103">Práce s daty v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7d797-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="7d797-104">Azure Cosmos DB nabízí flexibilitu schémat a bohaté indexování prostřednictvím nativní [JSON](http://www.json.org) datového modelu.</span><span class="sxs-lookup"><span data-stu-id="7d797-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="7d797-105">Všechny prostředky Azure Cosmos DB včetně databází, kolekcí, dokumentů a uložené procedury jsou modelovány a ukládány jako dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="7d797-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="7d797-106">Jako požadavek na vrácení přenosné, JSON (a Azure Cosmos DB) podporuje pouze omezenou sadu základních typů: řetězec, číslo, logickou hodnotu, pole, objekt a hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="7d797-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="7d797-107">Ale JSON je flexibilní a umožňují vývojářům a architektury představují složitější typy pomocí těchto primitivních elementů a skládání je jako objekty nebo pole.</span><span class="sxs-lookup"><span data-stu-id="7d797-107">However, JSON is flexible and allow developers and frameworks to represent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="7d797-108">Kromě základních typů mnoho aplikace potřebují [data a času](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) typ představují data a časová razítka.</span><span class="sxs-lookup"><span data-stu-id="7d797-108">In addition to the basic types, many applications need the [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type to represent dates and timestamps.</span></span> <span data-ttu-id="7d797-109">Tento článek popisuje, jak mohou vývojáři ukládání, načíst a dotaz na data v databázi Cosmos Azure pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="7d797-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using the .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="7d797-110">Ukládání data a času</span><span class="sxs-lookup"><span data-stu-id="7d797-110">Storing DateTimes</span></span>
<span data-ttu-id="7d797-111">Ve výchozím nastavení [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializuje hodnoty DateTime jako [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) řetězce.</span><span class="sxs-lookup"><span data-stu-id="7d797-111">By default, the [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="7d797-112">Většina aplikací můžete použít výchozí řetězcovou reprezentaci pro data a času z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="7d797-112">Most applications can use the default string representation for DateTime for the following reasons:</span></span>

* <span data-ttu-id="7d797-113">Lze porovnat s hodnotou řetězce a relativní řazení hodnoty DateTime se zachová, i když jsou transformovány na řetězce.</span><span class="sxs-lookup"><span data-stu-id="7d797-113">Strings can be compared, and the relative ordering of the DateTime values is preserved when they are transformed to strings.</span></span> 
* <span data-ttu-id="7d797-114">Tento přístup nevyžaduje žádné vlastní kód nebo atributy pro převod z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7d797-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="7d797-115">Data, jak je uložen ve formátu JSON jsou lidské čitelné.</span><span class="sxs-lookup"><span data-stu-id="7d797-115">The dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="7d797-116">Tento postup můžete využít výhod Azure Cosmos DB index pro výkon rychlé dotazů.</span><span class="sxs-lookup"><span data-stu-id="7d797-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="7d797-117">Například následující fragment kódu ukládá `Order` objekt obsahující dvě vlastnosti data a času - `ShipDate` a `OrderDate` jako dokument pomocí sady .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="7d797-117">For example, the following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using the .NET SDK:</span></span>

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

<span data-ttu-id="7d797-118">Tento dokument se ukládají v Azure Cosmos DB následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7d797-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="7d797-119">Alternativně můžete uložit data a času jako systému Unix časová razítka, to znamená, číslo reprezentující počet uplynulá počet sekund od 1. ledna 1970.</span><span class="sxs-lookup"><span data-stu-id="7d797-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing the number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="7d797-120">Interní časové razítko Azure Cosmos DB (`_ts`) vlastnost následuje tento přístup.</span><span class="sxs-lookup"><span data-stu-id="7d797-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="7d797-121">Můžete použít [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) třída určená k serializaci data a času jako čísla.</span><span class="sxs-lookup"><span data-stu-id="7d797-121">You can use the [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class to serialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="7d797-122">Indexování pro dotazy na rozsah data a času</span><span class="sxs-lookup"><span data-stu-id="7d797-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="7d797-123">Dotazy na rozsah jsou běžné s hodnotami data a času.</span><span class="sxs-lookup"><span data-stu-id="7d797-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="7d797-124">Například pokud budete potřebovat najít všechny objednávky vytvářené od včerejška nebo najít všechny objednávky odeslané za posledních pět minut, budete muset provádět dotazy rozsahu.</span><span class="sxs-lookup"><span data-stu-id="7d797-124">For example, if you need to find all orders created since yesterday, or find all orders shipped in the last five minutes, you need to perform range queries.</span></span> <span data-ttu-id="7d797-125">K úspěšnému provedení tyto dotazy, je nutné nakonfigurovat kolekce pro rozsah indexování řetězce.</span><span class="sxs-lookup"><span data-stu-id="7d797-125">To execute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="7d797-126">Další informace o tom, jak nakonfigurovat zásady indexování na [Azure Cosmos DB indexování zásady](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7d797-126">You can learn more about how to configure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="7d797-127">Dotazování na data a času v technologii LINQ</span><span class="sxs-lookup"><span data-stu-id="7d797-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="7d797-128">Sadu DocumentDB .NET SDK automaticky podporuje dotazování na data uložená v Azure DB Cosmos prostřednictvím LINQ.</span><span class="sxs-lookup"><span data-stu-id="7d797-128">The DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="7d797-129">Například následující fragment kódu ukazuje dotaz LINQ této odeslaných za poslední tři dny objednávek filtry.</span><span class="sxs-lookup"><span data-stu-id="7d797-129">For example, the following snippet shows a LINQ query that filters orders that were shipped in the last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="7d797-130">Další informace o dotazovací jazyk SQL Azure Cosmos DB a poskytovateli LINQ na [dotazování DB Cosmos](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="7d797-130">You can learn more about Azure Cosmos DB's SQL query language and the LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="7d797-131">V tomto článku jsme se podívali na postup ukládání, index a dotaz na data a času v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7d797-131">In this article, we looked at how to store, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d797-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d797-132">Next Steps</span></span>
* <span data-ttu-id="7d797-133">Stažení a spuštění [ukázky kódů v Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="7d797-133">Download and run the [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="7d797-134">Další informace o [dotaz rozhraní API DocumentDB](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="7d797-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="7d797-135">Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="7d797-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
