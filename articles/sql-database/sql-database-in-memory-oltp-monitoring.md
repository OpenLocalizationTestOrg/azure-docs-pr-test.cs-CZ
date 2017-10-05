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
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: 5afb2209f18b1ba2aa0a916a439509b01afd80da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitorování úložiště OLTP v paměti
Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti. Má maximální velikost úložiště OLTP v paměti, která je popsána v jednotlivých úrovních služby Premium [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Po překročení tohoto limitu vložení a aktualizace operace může spustit (došlo k chybě 41823). V tomto okamžiku budete potřebovat buď odstranit data získat paměť, nebo upgradujte úroveň výkonu databáze.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Určení, zda budou data nevejdou do limitu úložiště v paměti
Určit úložiště cap: obrátit [článku úrovních služby databáze SQL](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) pro úložiště CAP o různých úrovních služeb Premium.

Odhadnout požadavky na paměť pro paměťově optimalizované tabulky funguje pro SQL Server stejným způsobem jako se nepodporuje v Azure SQL Database. Trvat několik minut, přečtěte si toto téma na [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Všimněte si, že tabulky a proměnné řádky tabulky, protože jako indexy, započítávat velikost dat max uživatele. Příkaz ALTER TABLE navíc vyžaduje dostatek místa k vytvoření nové verze celou tabulku a jeho indexů.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Můžete sledovat využití úložiště v paměti jako procentní podíl [limitu úložiště pro vaše úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) ve službě Azure [portál](https://portal.azure.com/): 

* V okně databáze vyhledejte pole využití prostředků a klikněte na Upravit.
* Vyberte metriku `In-Memory OLTP Storage percentage`.
* Přidání oznámení, klikněte na pole využití prostředků a otevřete okno metriky a potom klikněte na Přidat upozornění.

Nebo použijte následující dotaz pro zobrazení využití úložiště v paměti:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Opravte z důvodu nedostatku paměti situacích - chyba 41823
V operacích INSERT, UPDATE a vytvořit selhání s chybovou zprávou 41823 spuštěna výsledky z důvodu nedostatku paměti.

Chybová zpráva 41823 označuje, že paměťově optimalizované tabulky a proměnných tabulek, které překročily maximální velikost.

Chcete tuto chybu vyřešit buď:

* Odstranění dat z paměťově optimalizované tabulky potenciálně snižování zátěže dat do tabulky tradiční, založené na disku; nebo,
* Upgradujte úroveň služby s dostatečně velké úložiště v paměti pro data, která je potřeba udržovat v paměťově optimalizovaných tabulkách.

## <a name="next-steps"></a>Další kroky
Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).
