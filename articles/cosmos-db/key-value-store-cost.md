---
title: "Azure DB Cosmos jako hodnota klíče úložiště – přehled nákladů | Microsoft Docs"
description: "Další informace o nízké náklady na používání Azure Cosmos DB jako úložiště hodnota klíče."
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
ms.openlocfilehash: 33eef1b51a5ee00b0fa67096030ed9ce92cf768e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure DB Cosmos jako hodnota klíče úložiště – přehled nákladů

Azure Cosmos DB je globálně distribuované a více modelech databáze služby pro vytváření aplikací s vysokou dostupností, velké škálování snadno. Ve výchozím nastavení Azure Cosmos DB automaticky indexuje všechna data, která ingestuje efektivně. To umožňuje rychlou a konzistentní [SQL](documentdb-sql-query.md) (a [JavaScript](programming.md)) dotazy na jakýkoli druh data. 

Tento článek popisuje náklady na databázi Azure Cosmos pro jednoduché zápis a operace čtení, pokud se používá jako úložiště dvojic klíč/hodnota. Zápisu operace zahrnují vložení, nahradí, odstranění a upserts dokumentů. Kromě zajištění vysoké dostupnosti 99,99 %, nabídky Azure Cosmos DB zaručit < 10 ms latenci pro čtení a < 15 ms latence (indexované), zapíše na 99th percentil. 

## <a name="why-we-use-request-units-rus"></a>Proč používáme jednotky žádosti (ruština)

Azure Cosmos DB výkonu podle množství zřízené [jednotky žádosti](request-units.md) (RU) pro oddíl. Zřizování je na druhý rozlišením a zakoupen v RUs za sekundu a RUs za minutu ([Nezaměňovat s každou hodinu fakturace](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs by měly být považovány měny, který zjednodušuje zřizování požadované propustnost pro aplikaci. Naše zákazníky nemají zamyslet nad rozlišování číst a zapisovat jednotek kapacity. Model jedné měny RUs vytvoří efektivitu sdílet zřízená kapacita mezi čtení a zápisy. Tento model zřízená kapacita povoluje službu zajistit předvídatelný a konzistentní propustnost, nízkou latencí a vysokou dostupnost zaručit. Nakonec RU používáme k modelu propustnost, ale každý zřízené RU má také definované objem prostředků (paměť, jádra). RU/s není pouze IOPS.

Jako systém globálně distribuovanou databázi je Cosmos DB pouze Azure služba, která poskytuje SLA na latenci, propustnosti a konzistence kromě vysoké dostupnosti. Propustnost, který zřídíte je použitá pro každé z oblastí spojené s vaším účtem databáze Cosmos DB. Pro čtení, Cosmos DB nabízí více, dobře definované [úrovně konzistence](consistency-levels.md) můžete vybírat. 

Následující tabulka uvádí počet RUs vyžaduje k provedení pro čtení a zápisu transakce na základě dokumentu velikosti 1 KB a 100KBs.

|Velikost položky|1 pro čtení|1 zápisu|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Náklady na čtení a zápisu

Pokud zřizujete 1000 RU za sekundu, tento objemy 3,6 m RU za hodinu a bude nákladů $0.08 za hodinu (v USA a Evropě). Pro dokument velikosti 1KB, to znamená, že můžete využívat 3,6 m čtení nebo zapíše 0,72 m (3.6mRU / 5) pomocí zřízené propustnosti. Normalizovány na mil. čtení a zápisu, náklady na by $0,022 /m čtení ($0.08 / 3,6) a zapisuje $0.111/ m ($0.08 / 0,72). Náklady na milionů stane minimální, jak je znázorněno v následující tabulce.

|Velikost položky|1 milion pro čtení|1 milion zápisu|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


Většina základních objektů blob nebo objekt úložiště služby ceně $0,40 za mil. čtení transakce a 5 na mil. zápisu transakce. Pokud se používá optimálně, Cosmos databáze může být až 98 % levnější než tato řešení (pro transakce 1KB).

## <a name="next-steps"></a>Další kroky

Nenechte ujít nových článků pro optimalizaci zřizování prostředků Azure Cosmos DB. Do té doby, klidně používat naše [RU kalkulačky](https://www.documentdb.com/capacityplanner).

