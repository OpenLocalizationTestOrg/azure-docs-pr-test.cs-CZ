---
title: "Tabulka aaaIntroduction tooAzure Cosmos DB na rozhraní API | Microsoft Docs"
description: "Zjistěte, jak můžete použít Azure Cosmos DB toostore a dotaz ohromné objemy dat klíč hodnota s nízkou latencí pomocí hello oblíbených rozhraní API operačních systémů MongoDB."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: arramac
ms.openlocfilehash: 4c5678898a772808f4bcd1465a23d436b0f8fc0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-table-api"></a>Úvod tooAzure Cosmos DB: tabulka rozhraní API

[Azure Cosmos DB](introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Poskytuje Azure Cosmos DB [globální distribuční klíč](distribute-data-globally.md), [elastické škálování propustnost a úložiště](partition-data.md) po celém světě, jednociferné milisekundu latence v hello 99th percentilu, [pět dobře definované úrovně konzistence](consistency-levels.md)a zaručit vysoká dostupnost, všechny zálohovány pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) aniž byste si toodeal se správou schéma a index. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Rozhraní Azure Table Storage API a Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB poskytuje hello tabulku rozhraní API (preview) pro aplikace, které potřebují úložiště dvojic klíč hodnota s flexibilní schématu, předvídatelný výkon, globální distribuce a vysoké propustnosti. Hello API tabulka poskytuje hello stejné funkce jako Azure Table storage, ale využívá výhod hello hello Azure Cosmos databázový stroj. 

Můžete pokračovat v toouse Azure Table storage pro tabulky s vysokou úložiště a nižší požadavky na propustnost. Azure Cosmos DB zavádí podporu pro úložiště optimalizované tabulky v budoucí aktualizaci a stávající a nové účty úložiště bude Azure Table upgradovat tooAzure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Rozhraní API tabulky Premium a Standard
Pokud aktuálně používáte Azure Table storage, získáte následující výhody přesunutím preview tooAzure Cosmos DB "premium tabulka" hello:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Preview) |
| --- | --- | --- |
| Latence | Rychlá, bez horních omezení latence | Jednociferné milisekundu latence pro čtení a zápisu, zálohovaný s < 10ms latencí přečte a < 15 ms latence zapíše na 99th percentilu hello v jakémkoli měřítku, kdekoli v hello, world |
| Propustnost | Vysoká škálovatelnost, žádný vyhrazený model propustnosti Tabulky mají omezení škálovatelnosti 20 000 operací za sekundu. | Vysoká škálovatelnost s [vyhrazenou rezervovanou propustností na tabulku](request-units.md), podložená smlouvami SLA Účty nemají žádné horní omezení propustnosti a podporují více než 10 milionů operací za sekundu na tabulku. |
| Globální distribuce | Jedna oblast s jednou volitelnou čitelnou sekundární oblastí čtení pro vysokou dostupnost Nemůžete zahájit převzetí služeb při selhání. | [Globální distribuční klíč](distribute-data-globally.md) z jednoho too30 + oblastí, podporu pro [automatickou a ruční převzetí služeb při selhání](regional-failover.md) kdykoli kdekoli v hello, world |
| Indexování | PartitionKey a RowKey používají pouze primární index. Žádné sekundární indexy | Automatické a úplné indexování u všech vlastností, žádná správa indexů |
| Dotaz | Při provádění dotazu se používá index pro primární klíč, jinak dochází k prohledávání. | Dotazy mohou ke zrychlení použít výhod automatického indexování vlastností. Modul databáze Azure Cosmos DB je schopný zajistit podporu agregací, geoprostorového indexování a řazení. |
| Konzistence | Silná v rámci primární oblasti, případně v sekundární oblasti | [Pět dobře definované úrovně konzistence](consistency-levels.md) tootrade vypnout dostupnosti, latenci, propustnosti a konzistence podle potřebám vaší aplikace |
| Ceny | Optimalizované úložiště  | Optimalizovaná propustnost |
| Smlouvy SLA | Dostupnost 99,9 % | 99,99 % dostupnost v rámci jedné oblasti a možnost tooadd další oblasti pro vyšší dostupnosti. [Nejlepší komplexní smlouvy SLA v oboru](https://azure.microsoft.com/support/legal/sla/cosmos-db/) týkající se obecné dostupnosti |

## <a name="how-tooget-started"></a>Způsob spuštění tooget

Vytvoření účtu Azure Cosmos DB na hello [portál Azure](https://portal.azure.com)a začněte používat naše [rychlý start pro tabulku rozhraní API pomocí rozhraní .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Další kroky

Tady je několik tooget ukazatele, kterou jste zahájili:
* [Vytvoření aplikace .NET pomocí hello tabulky rozhraní API](create-table-dotnet.md)
* [Vývoj s hello rozhraní API pro tabulky v rozhraní .NET](tutorial-develop-table-dotnet.md)
* [Dotazovat data tabulky pomocí hello tabulky rozhraní API](tutorial-query-table.md)
* [Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API tabulky hello](tutorial-global-distribution-table.md)
* [Sada SDK rozhraní API tabulky Azure Cosmos DB pro .NET](table-sdk-dotnet.md)
