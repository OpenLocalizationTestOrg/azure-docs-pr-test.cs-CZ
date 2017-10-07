---
title: propustnost aaaProvision pro Azure Cosmos DB | Microsoft Docs
description: "Zjistěte, jak tooset zřízené propustnosti pro containsers, kolekce, grafy a tabulky Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: c143f4aace466b7109168a50e2eb80ddeca6400e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Nastavit propustnost pro Azure Cosmos DB kontejnery

Můžete nastavit propustnost pro Azure Cosmos DB kontejnerů v hello portál Azure nebo pomocí hello klientské sady SDK. 

Hello následující tabulka uvádí hello propustnost, které jsou k dispozici pro kontejnery:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Jeden oddíl kontejneru</strong></p></td>
            <td valign="top"><p><strong>Oddílů kontejneru</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimální propustnost</p></td>
            <td valign="top"><p>400 jednotek žádosti za sekundu</p></td>
            <td valign="top"><p>2 500 jednotek žádosti za sekundu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximální propustnost</p></td>
            <td valign="top"><p>10 000 jednotek žádosti za sekundu</p></td>
            <td valign="top"><p>Unlimited</p></td>
        </tr>
    </tbody>
</table>

## <a name="tooset-hello-throughput-by-using-hello-azure-portal"></a>propustnost hello tooset pomocí hello portálu Azure

1. V novém okně, otevřete hello [portál Azure](https://portal.azure.com).
2. Na levém panelu hello, klikněte na tlačítko **Azure Cosmos DB**, nebo klikněte na tlačítko **více služeb** v dolní části hello, posuňte se příliš**databáze**a pak klikněte na tlačítko **Azure Cosmos DB**.
3. Vyberte svůj účet Cosmos DB.
4. V novém okně hello, klikněte na tlačítko **Data Explorer (Preview)** v navigační nabídce hello.
5. V novém okně hello, rozbalte databázi a kontejneru a pak klikněte na tlačítko **škálování & nastavení**.
6. V novém okně hello, zadejte novou hodnotu propustnosti hello v hello **propustnost** pole a pak klikněte na **Uložit**.

<a id="set-throughput-sdk"></a>

## <a name="tooset-hello-throughput-by-using-hello-documentdb-api-for-net"></a>propustnost hello tooset pomocí hello DocumentDB rozhraní API pro .NET

```C#
//Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput toohello new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Propustnost – nejčastější dotazy

**Můžete nastavit Moje tooless propustnost než 400 RU/s**

400 RU/s je minimální propustnost hello k dispozici na kolekce tvořené jedním oddílem Cosmos DB (hello minimální pro dělené kolekce je 2 500 RU/s). Žádost jednotky jsou nastaveny v intervalech 100 RU/s, ale propustnost nelze nastavit too100 RU/s nebo jakoukoli hodnotu menší než 400 RU/s. Pokud hledáte nákladově efektivní způsob toodevelop a testování Cosmos DB, můžete použít hello volné [emulátoru DB Cosmos Azure](local-emulator.md), které můžete nasadit místně bez nákladů. 

**Jak lze nastavit pomocí hello MongoDB API througput?**

Neexistuje žádné propustnost tooset rozšíření rozhraní API MongoDB. Hello doporučení je toouse hello DocumentDB rozhraní API, jak je znázorněno v [tooset hello propustnost pomocí hello DocumentDB rozhraní API pro .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Další kroky

toolearn Další informace o zřizování a probíhající planetu měřítku s Cosmos databáze, najdete v části [dělení a škálování s Cosmos DB](partition-data.md).
