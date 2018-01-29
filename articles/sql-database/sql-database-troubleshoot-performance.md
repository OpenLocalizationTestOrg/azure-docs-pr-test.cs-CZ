---
title: "Monitorování a optimalizace výkonu – Azure SQL Database | Microsoft Docs"
description: "Tipy k ladění ve službě Azure SQL Database pomocí vyhodnocení a zlepšování výkonu."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: "ladění, ladění, ladění tipy, výkonu sql databáze výkonu výkonu SQL optimalizace výkonu databáze sql"
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: v-shysun
ms.openlocfilehash: 1791c56f86ee1997177daa95638c4f14068f8115
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="monitoring-and-performance-tuning"></a>Monitorování a optimalizace výkonu

Databáze SQL Azure je automaticky prováděna a flexibilní datová služba, kde můžete snadno sledovat využití, přidat nebo odebrat prostředky (procesor, paměť, vstupně-výstupní operace), najít doporučení, které může zlepšit výkon vaší databáze, nebo nechat databáze přizpůsobit pro vaši úlohu a automaticky optimalizace výkonu.

Tento článek obsahuje přehled monitorování a možností, které jsou k dispozici ve službě Azure SQL Database ladění výkonu.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Monitorování a řešení potíží s výkonem databáze

Databáze SQL Azure umožňuje snadno monitorovat využití vaší databáze a identifikovat dotazy, které by mohly způsobit problémy s výkonem. Můžete sledovat výkon databáze pomocí Azure zobrazení portálu nebo systému. Máte následující možnosti pro monitorování a řešení potíží s výkonem databáze:

1. V [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**, vyberte databázi a pak vyhledejte prostředky blíží jejich maximální pomocí graf sledování. Spotřeba DTU se zobrazí ve výchozím nastavení. Klikněte na tlačítko **upravit** změnit časový rozsah a hodnoty zobrazené.
2. Použití [Query Performance Insight](sql-database-query-performance.md) k identifikaci dotazy, které potřebují nejvíc prostředků.
3. Můžete použít zobrazení dynamické správy (zobrazení dynamické správy), rozšířených událostí (`XEvents`) a úložiště dotazů v aplikaci SSMS se získat parametry výkonu v reálném čase.

Najdete v článku [výkonu pokyny tématu](sql-database-performance-guidance.md) najít technik, které můžete použít ke zlepšení výkonu databáze SQL Azure, je-li identifikovat některé problém pomocí těchto sestav a zobrazení.

> [!IMPORTANT] 
> Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Optimalizace databáze za účelem zvýšení výkonu

Databáze SQL Azure umožňuje identifikovat příležitosti pro zlepšení a optimalizovat výkon dotazů beze změny prostředky kontrolou [doporučení ladění výkonu](sql-database-advisor.md). Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. Můžete použít tyto vyladění doporučení pro zlepšení výkonu vašich úloh.
Můžete také umožňují Azure SQL database k [automaticky optimalizovat výkon vašich dotazů](sql-database-automatic-tuning.md) použitím všechny identifikovat doporučení a ověřování, že se zlepšit výkon databáze. Pro zlepšení výkonu databáze můžete použít následující možnosti:

1. Použití [Poradce pro funkci SQL Database](sql-database-advisor-portal.md) zobrazíte doporučení k vytváření a rušení indexů, parametrické dotazy a opravit problémy schématu.
2. [Povolit automatické ladění](sql-database-automatic-tuning-enable.md) a nechat Azure SQL databáze automaticky oprava identifikovat problémy s výkonem.

## <a name="improving-database-performance-with-more-resources"></a>Zvýšení výkonu databáze s další prostředky

Nakonec pokud nejsou žádné řešitelné položky, které může zlepšit výkon vaší databáze, můžete změnit objem prostředků, které jsou k dispozici ve službě Azure SQL Database. Další zdroje informací můžete přiřadit změnou [vrstvy služby](sql-database-service-tiers.md) samostatná databáze nebo zvýšení jednotky Edtu fondu elastické databáze kdykoli.
1. Pro samostatné databáze, můžete [Změna úrovně služeb](sql-database-service-tiers.md) na vyžádání ke zlepšení výkonu databáze.
2. V případě více databází, zvažte použití [elastické fondy](sql-database-elastic-pool-guidance.md) prostředky automaticky škálovat.

## <a name="tune-and-refactor-application-or-database-code"></a>Ladění a refactor aplikace nebo kódu databáze

Kód aplikace více optimálně použít databázi, změňte indexy, vynutit plány nebo ručně přizpůsobit databázi do úlohy pomocí odkazů na servery, můžete změnit. Najít některé pokyny a tipy pro ruční ladění a přepisování kód [výkonu pokyny tématu](sql-database-performance-guidance.md) článku.


## <a name="next-steps"></a>Další kroky

- Povolit automatické ladění ve službě Azure SQL Database a nechat automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- Chcete-li použít ruční ladění, můžete zjistit [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a ty, které zlepšit výkon vašich dotazů použít ručně.
- Změnit prostředky, které jsou k dispozici ve vaší databázi změnou [úrovních služby databáze SQL Azure](sql-database-performance-guidance.md)
