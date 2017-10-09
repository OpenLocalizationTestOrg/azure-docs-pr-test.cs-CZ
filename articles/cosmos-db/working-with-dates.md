---
title: aaaWorking s daty v Azure Cosmos DB | Microsoft Docs
description: Informace o tom, jak toowork se data v Azure Cosmos DB.
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
ms.openlocfilehash: 27ec170e4bef72c0b5b456738f1275ef02543024
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="ab694-103">Práce s daty v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ab694-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="ab694-104">Azure Cosmos DB nabízí flexibilitu schémat a bohaté indexování prostřednictvím nativní [JSON](http://www.json.org) datového modelu.</span><span class="sxs-lookup"><span data-stu-id="ab694-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="ab694-105">Všechny prostředky Azure Cosmos DB včetně databází, kolekcí, dokumentů a uložené procedury jsou modelovány a ukládány jako dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="ab694-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="ab694-106">Jako požadavek na vrácení přenosné, JSON (a Azure Cosmos DB) podporuje pouze omezenou sadu základních typů: řetězec, číslo, logickou hodnotu, pole, objekt a hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="ab694-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="ab694-107">Ale JSON je flexibilní a povolit vývojáři a architektury toorepresent složitější typy pomocí těchto primitivních elementů a skládání je jako objekty nebo pole.</span><span class="sxs-lookup"><span data-stu-id="ab694-107">However, JSON is flexible and allow developers and frameworks toorepresent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="ab694-108">Základní typy toohello přidání mnoho aplikace potřebují hello [data a času](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) zadejte toorepresent data a časová razítka.</span><span class="sxs-lookup"><span data-stu-id="ab694-108">In addition toohello basic types, many applications need hello [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type toorepresent dates and timestamps.</span></span> <span data-ttu-id="ab694-109">Tento článek popisuje, jak mohou vývojáři ukládání, načíst a dotaz na data v Azure DB Cosmos pomocí hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="ab694-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using hello .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="ab694-110">Ukládání data a času</span><span class="sxs-lookup"><span data-stu-id="ab694-110">Storing DateTimes</span></span>
<span data-ttu-id="ab694-111">Ve výchozím nastavení, hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializuje hodnoty DateTime jako [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) řetězce.</span><span class="sxs-lookup"><span data-stu-id="ab694-111">By default, hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="ab694-112">Většina aplikací můžete použít hello výchozí řetězcovou reprezentaci pro data a času pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ab694-112">Most applications can use hello default string representation for DateTime for hello following reasons:</span></span>

* <span data-ttu-id="ab694-113">Lze porovnat s hodnotou řetězce a hello relativní řazení hodnot data a času hello se zachová, i když jsou transformovaných toostrings.</span><span class="sxs-lookup"><span data-stu-id="ab694-113">Strings can be compared, and hello relative ordering of hello DateTime values is preserved when they are transformed toostrings.</span></span> 
* <span data-ttu-id="ab694-114">Tento přístup nevyžaduje žádné vlastní kód nebo atributy pro převod z formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ab694-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="ab694-115">Hello kalendářních dat, jak je uložen ve formátu JSON jsou lidské čitelné.</span><span class="sxs-lookup"><span data-stu-id="ab694-115">hello dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="ab694-116">Tento postup můžete využít výhod Azure Cosmos DB index pro výkon rychlé dotazů.</span><span class="sxs-lookup"><span data-stu-id="ab694-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="ab694-117">Například následující fragment kódu úložiště hello `Order` objekt obsahující dvě vlastnosti data a času - `ShipDate` a `OrderDate` jako dokument hello pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="ab694-117">For example, hello following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using hello .NET SDK:</span></span>

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

<span data-ttu-id="ab694-118">Tento dokument se ukládají v Azure Cosmos DB následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ab694-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="ab694-119">Alternativně můžete uložit data a času jako systému Unix časová razítka, to znamená, číslo reprezentující počet hello uplynulá počet sekund od 1. ledna 1970.</span><span class="sxs-lookup"><span data-stu-id="ab694-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing hello number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="ab694-120">Interní časové razítko Azure Cosmos DB (`_ts`) vlastnost následuje tento přístup.</span><span class="sxs-lookup"><span data-stu-id="ab694-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="ab694-121">Můžete použít hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) třídy tooserialize data a času jako čísla.</span><span class="sxs-lookup"><span data-stu-id="ab694-121">You can use hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class tooserialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="ab694-122">Indexování pro dotazy na rozsah data a času</span><span class="sxs-lookup"><span data-stu-id="ab694-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="ab694-123">Dotazy na rozsah jsou běžné s hodnotami data a času.</span><span class="sxs-lookup"><span data-stu-id="ab694-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="ab694-124">Například pokud potřebujete toofind všechny objednávky vytvářené od včerejška, nebo vyhledat objednávky poskytuje hello posledních pět minut, je třeba tooperform rozsah dotazy.</span><span class="sxs-lookup"><span data-stu-id="ab694-124">For example, if you need toofind all orders created since yesterday, or find all orders shipped in hello last five minutes, you need tooperform range queries.</span></span> <span data-ttu-id="ab694-125">tooexecute tyto dotazy efektivně, je nutné nakonfigurovat kolekce pro rozsah indexování řetězce.</span><span class="sxs-lookup"><span data-stu-id="ab694-125">tooexecute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="ab694-126">Další informace o tooconfigure indexování zásady na [Azure Cosmos DB indexování zásady](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="ab694-126">You can learn more about how tooconfigure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="ab694-127">Dotazování na data a času v technologii LINQ</span><span class="sxs-lookup"><span data-stu-id="ab694-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="ab694-128">Hello DocumentDB .NET SDK automaticky podporuje dotazování na data uložená v Azure DB Cosmos prostřednictvím LINQ.</span><span class="sxs-lookup"><span data-stu-id="ab694-128">hello DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="ab694-129">Například hello následující fragment kódu ukazuje dotaz LINQ této odeslaných v hello poslední tři dny objednávek filtry.</span><span class="sxs-lookup"><span data-stu-id="ab694-129">For example, hello following snippet shows a LINQ query that filters orders that were shipped in hello last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="ab694-130">Další informace o databázi Cosmos Azure SQL dotazu jazyka a hello LINQ poskytovatele v [dotazování DB Cosmos](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="ab694-130">You can learn more about Azure Cosmos DB's SQL query language and hello LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="ab694-131">V tomto článku jsme se podívali na tom, jak toostore, index a dotaz na data a času v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ab694-131">In this article, we looked at how toostore, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab694-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab694-132">Next Steps</span></span>
* <span data-ttu-id="ab694-133">Stažení a spuštění hello [ukázky kódů v Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="ab694-133">Download and run hello [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="ab694-134">Další informace o [dotaz rozhraní API DocumentDB](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="ab694-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="ab694-135">Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="ab694-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
