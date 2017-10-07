---
title: "aaaMonitor a zlepšit výkon - Azure SQL Database | Microsoft Docs"
description: "Dobrý den, které poskytuje Azure SQL Database výkon nástroje toohelp určit oblasti, které může zlepšit výkon aktuální dotaz."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2016
ms.author: sstein
ms.openlocfilehash: 84b8a1bc62698a29deb49e765f208bd7e14d0870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-improve-performance"></a>Monitorování a zlepšení výkonu
Azure SQL Database identifikuje potenciální problémy ve vaší databázi a doporučuje akce, které může zlepšit výkon vašich úloh zadáním inteligentního vyladění akce a doporučení.

tooreview výkon databáze, použijte hello **výkonu** dlaždici na stránce Přehled hello nebo přejděte dolů příliš "Podpora + řešení potíží" část:

   ![Zobrazení výkonu](./media/sql-database-performance/entries.png)

V části "Podpora + řešení potíží" hello můžete použít hello následující stránky:


1. [Přehled výkonnostní](#performance-overview) toomonitor výkon vaší databáze. 
2. [Doporučení pro optimální výkon](#performance-recommendations) toofind výkonu doporučení, které může zlepšit výkon vašich úloh.
3. [Dotaz na informace o výkonu](#query-performance-insight) toofind nejvyšší na prostředky dotazy.
4. [Automatické ladění](#automatic-tuning) toolet Azure SQL Database automaticky optimalizací databáze.

## <a name="performance-overview"></a>Přehled výkonnostní
Toto zobrazení obsahuje souhrn výkon databáze a pomůže vám s výkonem, ladění a řešení potíží. 

![Výkon](./media/sql-database-performance/performance.png)

* Hello **doporučení** dlaždice obsahuje rozpis systémů ladění doporučení pro vaši databázi (první tři doporučení jsou uvedené. Pokud existují další). Kliknutím na tuto dlaždici přejdete příliš**[výkonu doporučení](#performance-recommendations)**. 
* Hello **ladění aktivity** dlaždice poskytuje souhrn hello probíhající a dokončené ladění akce pro vaši databázi, která poskytuje rychlý přehled do historie hello ladění aktivity. Kliknutím na tuto dlaždici přejdete toohello úplná optimalizací zobrazení historie pro vaši databázi.
* Hello **automatické ladění** dlaždice ukazuje hello [automatické ladění konfigurace](sql-database-automatic-tuning-enable.md) pro vaši databázi (optimalizace pro možnosti, které jsou automaticky použité tooyour databáze). Kliknutím na tuto dlaždici, otevře se dialogové okno Konfigurace automatizace hello.
* Hello **dotazy na databázi** dlaždice zobrazuje hello souhrn hello výkon dotazů pro databázi (celkový počet jednotek DTU využití a horní na prostředky dotazy). Kliknutím na tuto dlaždici přejdete příliš**[Query Performance Insight](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Doporučení výkonu
Tato stránka obsahuje inteligentního [ladění doporučení](sql-database-advisor.md) , může zlepšit výkon vaší databáze. na této stránce se zobrazují následující typy doporučení Hello:

* Doporučení, na které toocreate indexy nebo drop.
* Doporučení, když jsou v databázi hello zjištěny problémy schématu.
* Doporučení pro dotazy využívat parametrizované dotazy.

![Výkon](./media/sql-database-performance/recommendations.png)

Můžete také získat úplnou historii ladění akce, které byly použity v posledních hello.

Zjistěte, jak toofind operátoru apply výkonu doporučení v [najít a použít výkonu doporučení](sql-database-advisor-portal.md) článku.

## <a name="automatic-tuning"></a>Automatické ladění
Databáze Azure SQL můžete vyladit výkon databáze automaticky použitím [výkonu doporučení](sql-database-advisor.md). toolearn víc, přečtěte si [automatické ladění článku](sql-database-automatic-tuning.md). tooenable, přečtěte si [jak tooenable automatické ladění](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Query Performance Insight
[Dotaz na informace o výkonu](sql-database-query-performance.md) vám umožní toospend méně času tím, že poskytuje řešení potíží s výkonem databáze:

* Podrobnější přehled o vaší spotřeby prostředků (DTU) databází. 
* Hello nejvíce využívají procesor dotazů, které lze ladit potenciálně pro zlepšení výkonu. 
* Hello možnost toodrill dolů do hello podrobnosti dotazu. 

  ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

Další informace o této stránce naleznete v článku hello  **[jak toouse Query Performance Insight](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Další zdroje
* [Azure SQL Database – Průvodce výkonem pro izolované databáze](sql-database-performance-guidance.md)
* [Pokud má být použita fondu elastické databáze?](sql-database-elastic-pool-guidance.md)

