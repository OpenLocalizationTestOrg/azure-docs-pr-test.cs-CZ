---
title: "Monitorování úložiště v paměti XTP | Microsoft Docs"
description: "Odhad a monitorování úložiště v paměti XTP používat, kapacity; Vyřešte chyby kapacity 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jodebrui
ms.openlocfilehash: 1e7088e80cc86e3c7cf8ae8ea180d797de613e71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitorování OLTP v paměti úložiště
Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti. Má maximální velikost úložiště OLTP v paměti, která je popsána v jednotlivých úrovních služby Premium [jedna databáze prostředků omezení](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) a [limitů elastického fondu prostředků](sql-database-resource-limits.md#elastic-pool-change-storage-size). Po překročení tohoto limitu vložení a aktualizace operace může spustit došlo k chybě 41823 pro samostatné databáze a chyba 41840 pro elastické fondy. V tomto okamžiku je nutné odstranit data získat paměť, nebo upgradovat úroveň výkonu databáze.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Určete, zda vyhovuje data v rámci limitu úložiště OLTP v paměti
Určete úložiště CAP o různých úrovních služeb Premium. V tématu [jedna databáze prostředků omezení](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) a [limitů elastického fondu prostředků](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Odhadnout požadavky na paměť pro paměťově optimalizované tabulky funguje pro SQL Server stejným způsobem jako se nepodporuje v Azure SQL Database. Trvat několik minut, přečtěte si tento článek na [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tabulky a proměnné řádky tabulky, jakož i indexy, započítávat velikost dat max uživatele. Příkaz ALTER TABLE navíc vyžaduje dostatek místa k vytvoření nové verze celou tabulku a jeho indexů.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Můžete sledovat využití úložiště v paměti jako procentní podíl limitu úložiště pro vaše úroveň výkonu v [portál Azure](https://portal.azure.com/): 

1. V okně databáze vyhledejte pole využití prostředků a klikněte na Upravit.
2. Vyberte metriku `In-Memory OLTP Storage percentage`.
3. Přidání oznámení, klikněte na pole využití prostředků a otevřete okno metriky a potom klikněte na Přidat upozornění.

Nebo použijte následující dotaz pro zobrazení využití úložiště v paměti:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Opravte-v-paměti OLTP úložiště situacích – chyby 41823 a 41840
Nedosáhli limitu úložiště OLTP v paměti ve výsledcích databáze v vložení, aktualizaci, ALTER a vytvořte operace došlo k chybě 41823 (pro samostatnou databáze) nebo Chyba 41840 (pro elastické fondy). Obě chyby způsobit aktivní transakci na zrušení.

Chybové zprávy 41823 a 41840 naznačují, že paměťově optimalizované tabulky a proměnné tabulky v databázi nebo fondu bylo dosaženo maximální velikost úložiště OLTP v paměti.

Chcete tuto chybu vyřešit buď:

* Odstranění dat z paměťově optimalizované tabulky potenciálně snižování zátěže dat do tabulky tradiční, založené na disku; nebo,
* Upgradujte úroveň služby s dostatečně velké úložiště v paměti pro data, která je potřeba udržovat v paměťově optimalizovaných tabulkách.

> [!NOTE] 
> Ve výjimečných případech může být chyby 41823 a 41840 přechodný, což znamená, není dostatek dostupné úložiště OLTP v paměti, a opakováním operace úspěšná. Proto doporučujeme pro oba monitorování celkového dostupné úložiště OLTP v paměti a opakovat, pokud nejprve zjištění chyby 41823 nebo 41840. Další informace o logika opakovaných pokusů, najdete v části [zjišťování konfliktů a opakujte postup s OLTP v paměti](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Další postup
Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).
