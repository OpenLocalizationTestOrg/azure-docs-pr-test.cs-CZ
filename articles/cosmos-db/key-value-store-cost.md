---
title: "aaaAzure DB Cosmos jako hodnota klíče úložiště – přehled nákladů | Microsoft Docs"
description: "Další informace o hello nízké náklady na používání Azure Cosmos DB jako úložiště hodnota klíče."
keywords: "Hodnota klíče úložiště"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: de7207760a8e1fca0e30f951109748835dabf4a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure DB Cosmos jako hodnota klíče úložiště – přehled nákladů

Azure Cosmos DB je globálně distribuované a více modelech databáze služby pro vytváření aplikací s vysokou dostupností, velké škálování snadno. Ve výchozím nastavení Azure Cosmos DB automaticky indexuje všechny hello data, která ingestuje efektivně. To umožňuje rychlou a konzistentní [SQL](documentdb-sql-query.md) (a [JavaScript](programming.md)) dotazy na jakýkoli druh data. 

Tento článek popisuje hello náklady na databázi Azure Cosmos pro jednoduché zápis a operace čtení, pokud se používá jako úložiště dvojic klíč/hodnota. Zápisu operace zahrnují vložení, nahradí, odstranění a upserts dokumentů. Kromě zajištění vysoké dostupnosti 99,99 %, nabídky Azure Cosmos DB zaručit < 10 ms latenci pro čtení a < 15 ms latence pro hello (indexované), zapíše na 99th percentilu hello. 

## <a name="why-we-use-request-units-rus"></a>Proč používáme jednotky žádosti (ruština)

Azure Cosmos DB výkonu podle množství hello zřízené [jednotky žádosti](request-units.md) (RU) pro oddíl hello. zřizování Hello je na druhý rozlišením a zakoupen v RUs za sekundu a RUs za minutu ([není toobe zaměňovat s každou hodinu fakturace hello](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs by měly být považovány měny, který zjednodušuje hello zřizování požadované propustnost pro aplikace hello. Naše zákazníky nemají toothink rozlišení mezi číst a zapisovat jednotek kapacity. model jedné měny Hello RUs vytvoří efektivitu tooshare hello zřídit kapacitu mezi čtení a zápisy. Tento model zřízená kapacita umožňuje tooprovide služby hello předvídatelný a konzistentní propustnost, nízkou latencí a vysokou dostupnost zaručit. Nakonec používáme RU toomodel propustnost, ale každý zřízené RU má také definované objem prostředků (paměť, jádra). RU/s není pouze IOPS.

Jako systém globálně distribuovanou databázi je Cosmos DB hello pouze služba Azure, která poskytuje SLA na latenci, propustnosti a konzistence v přidání toohigh dostupnosti. Hello propustnost, který zřídíte je použité tooeach oblastí hello spojené s vaším účtem databáze Cosmos DB. Pro čtení, Cosmos DB nabízí více, dobře definované [úrovně konzistence](consistency-levels.md) pro toochoose z. 

Hello následující tabulka uvádí hello počet RUs požadované tooperform čtení a zápisu transakce na základě dokumentu velikosti 1KB a 100KBs.

|Velikost položky|1 pro čtení|1 zápisu|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Náklady na čtení a zápisu

Pokud zřizujete 1000 RU za sekundu, to částky too3.6m RU za hodinu a bude náklady $0.08 hello hodinu (v hello USA a Evropě). Pro dokument velikosti 1KB, to znamená, že můžete využívat 3,6 m čtení nebo zapíše 0,72 m (3.6mRU / 5) pomocí zřízené propustnosti. Normalizovaný toomillion čte a zapisuje, hello náklady by měly být $0,022 /m čtení ($0.08 / 3,6) a zapisuje $0.111/ m ($0.08 / 0,72). Hello náklady za milionů stane minimální, jak ukazuje následující tabulka hello.

|Velikost položky|1 milion pro čtení|1 milion zápisu|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


Většina základní objekt blob hello nebo objekt úložiště služby ceně $0,40 za mil. čtení transakce a 5 na mil. zápisu transakce. Pokud se používá optimálně, může být Cosmos DB too98 % levnější než tato řešení (pro transakce 1KB).

## <a name="next-steps"></a>Další kroky

Nenechte ujít nových článků pro optimalizaci zřizování prostředků Azure Cosmos DB. V té doby hello působí volné toouse naše [RU kalkulačky](https://www.documentdb.com/capacityplanner).

