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
# <a name="working-with-dates-in-azure-cosmos-db"></a>Práce s daty v Azure Cosmos DB
Azure Cosmos DB nabízí flexibilitu schémat a bohaté indexování prostřednictvím nativní [JSON](http://www.json.org) datového modelu. Všechny prostředky Azure Cosmos DB včetně databází, kolekcí, dokumentů a uložené procedury jsou modelovány a ukládány jako dokumenty JSON. Jako požadavek na vrácení přenosné, JSON (a Azure Cosmos DB) podporuje pouze omezenou sadu základních typů: řetězec, číslo, logickou hodnotu, pole, objekt a hodnotu Null. Ale JSON je flexibilní a povolit vývojáři a architektury toorepresent složitější typy pomocí těchto primitivních elementů a skládání je jako objekty nebo pole. 

Základní typy toohello přidání mnoho aplikace potřebují hello [data a času](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) zadejte toorepresent data a časová razítka. Tento článek popisuje, jak mohou vývojáři ukládání, načíst a dotaz na data v Azure DB Cosmos pomocí hello .NET SDK.

## <a name="storing-datetimes"></a>Ukládání data a času
Ve výchozím nastavení, hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializuje hodnoty DateTime jako [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) řetězce. Většina aplikací můžete použít hello výchozí řetězcovou reprezentaci pro data a času pro hello následujících důvodů:

* Lze porovnat s hodnotou řetězce a hello relativní řazení hodnot data a času hello se zachová, i když jsou transformovaných toostrings. 
* Tento přístup nevyžaduje žádné vlastní kód nebo atributy pro převod z formátu JSON.
* Hello kalendářních dat, jak je uložen ve formátu JSON jsou lidské čitelné.
* Tento postup můžete využít výhod Azure Cosmos DB index pro výkon rychlé dotazů.

Například následující fragment kódu úložiště hello `Order` objekt obsahující dvě vlastnosti data a času - `ShipDate` a `OrderDate` jako dokument hello pomocí .NET SDK:

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

Tento dokument se ukládají v Azure Cosmos DB následujícím způsobem:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Alternativně můžete uložit data a času jako systému Unix časová razítka, to znamená, číslo reprezentující počet hello uplynulá počet sekund od 1. ledna 1970. Interní časové razítko Azure Cosmos DB (`_ts`) vlastnost následuje tento přístup. Můžete použít hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) třídy tooserialize data a času jako čísla. 

## <a name="indexing-datetimes-for-range-queries"></a>Indexování pro dotazy na rozsah data a času
Dotazy na rozsah jsou běžné s hodnotami data a času. Například pokud potřebujete toofind všechny objednávky vytvářené od včerejška, nebo vyhledat objednávky poskytuje hello posledních pět minut, je třeba tooperform rozsah dotazy. tooexecute tyto dotazy efektivně, je nutné nakonfigurovat kolekce pro rozsah indexování řetězce.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Další informace o tooconfigure indexování zásady na [Azure Cosmos DB indexování zásady](indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>Dotazování na data a času v technologii LINQ
Hello DocumentDB .NET SDK automaticky podporuje dotazování na data uložená v Azure DB Cosmos prostřednictvím LINQ. Například hello následující fragment kódu ukazuje dotaz LINQ této odeslaných v hello poslední tři dny objednávek filtry.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

Další informace o databázi Cosmos Azure SQL dotazu jazyka a hello LINQ poskytovatele v [dotazování DB Cosmos](documentdb-sql-query.md).

V tomto článku jsme se podívali na tom, jak toostore, index a dotaz na data a času v Azure Cosmos DB.

## <a name="next-steps"></a>Další kroky
* Stažení a spuštění hello [ukázky kódů v Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Další informace o [dotaz rozhraní API DocumentDB](documentdb-sql-query.md)
* Další informace o [Azure Cosmos DB indexování zásady](indexing-policies.md)
