---
title: "úložiště v paměti XTP aaaMonitor | Microsoft Docs"
description: "Odhad a monitorování úložiště v paměti XTP používat, kapacity; Vyřešte chyby kapacity 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitorování úložiště OLTP v paměti
Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti. Každou úroveň služeb Premium je maximální velikost úložiště OLTP v paměti, která je popsána v hello [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Po překročení tohoto limitu vložení a aktualizace operace může spustit (došlo k chybě 41823). V tomto okamžiku budete potřebovat tooeither odstranit data tooreclaim paměti, nebo upgradovat úroveň výkonu hello vaší databáze.

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a>Určení, zda data se vejde do cap hello úložiště v paměti
Určit úložiště cap hello: najdete hello [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) pro hello úložiště CAP o hello různých úrovních služeb Premium.

Odhad paměti, že požadavky pro paměťově optimalizované tabulky funguje hello stejný způsob systému SQL Server, protože nemá ve službě Azure SQL Database. Trvat několik minut tooreview tohoto tématu [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Všimněte si, že tabulka hello a proměnné řádky tabulky, jakož i indexy, započítávat velikost dat max uživatele hello. Příkaz ALTER TABLE navíc musí dostatek místnosti toocreate novou verzi hello celou tabulku a jeho indexů.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Můžete sledovat využití úložiště v paměti jako procentní podíl hello [limitu úložiště pro vaše úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) v hello Azure [portál](https://portal.azure.com/): 

* V okně databáze hello vyhledejte hello prostředků využití políčko a klikněte na Upravit.
* Vyberte metriku hello `In-Memory OLTP Storage percentage`.
* tooadd výstrahu, klikněte na hello využití prostředků pole tooopen hello metriky okna a potom klikněte na Přidat upozornění.

Nebo použijte hello následující dotaz využití úložiště v paměti tooshow hello:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Opravte z důvodu nedostatku paměti situacích - chyba 41823
V operacích INSERT, UPDATE a vytvořit selhání s chybovou zprávou 41823 spuštěna výsledky z důvodu nedostatku paměti.

Chybová zpráva 41823 označuje, že hello paměťově optimalizované tabulky a proměnných tabulek, které překročily maximální velikost hello.

tooresolve tato chyba, buď:

* Odstranění dat z hello paměťově optimalizované tabulky, potenciálně přesměrovává hello data tootraditional, založené na disku tabulky; nebo,
* Upgrade tooone vrstvy služby hello s dostatečným místem úložiště v paměti pro hello data potřebujete tookeep v paměťově optimalizovaných tabulkách.

## <a name="next-steps"></a>Další kroky
Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).
