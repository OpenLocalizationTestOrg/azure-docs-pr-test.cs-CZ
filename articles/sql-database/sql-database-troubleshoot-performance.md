---
title: "aaaMonitoring & optimalizace výkonu – Azure SQL Database | Microsoft Docs"
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
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: v-shysun
ms.openlocfilehash: 9e196831902aa6ea841ef14caf5713e82ebfc62d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-performance-tuning"></a>Monitorování a optimalizace výkonu

Databáze SQL Azure je automaticky prováděna a flexibilní datová služba, kde můžete snadno sledovat využití, přidat nebo odebrat prostředky (procesor, paměť, vstupně-výstupní operace), najít doporučení, které může zlepšit výkon vaší databáze, nebo nechat databáze přizpůsobit tooyour zatížení a automaticky optimalizace výkonu.

Tento článek obsahuje přehled monitorování a možností, které jsou k dispozici ve službě Azure SQL Database ladění výkonu.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Monitorování a řešení potíží s výkonem databáze

Azure SQL Database umožňuje tooeasily můžete sledovat využití vaší databáze a identifikovat dotazy, které by mohly způsobit problémy s výkonem hello. Můžete sledovat výkon databáze pomocí Azure zobrazení portálu nebo systému. Máte následující možnosti pro monitorování a řešení potíží s výkonem databáze hello:

1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**, vyberte hello databáze a potom pomocí hello monitorování grafu toolook blíží jejich maximální počet zdrojů. Spotřeba DTU se zobrazí ve výchozím nastavení. Klikněte na tlačítko **upravit** toochange hello časové rozmezí a hodnoty zobrazené.
2. Použití [Query Performance Insight](sql-database-query-performance.md) tooidentify hello dotazy, které potřebují hello nejvíc prostředků.
3. Můžete použít zobrazení dynamické správy (zobrazení dynamické správy), rozšířených událostí (`XEvents`), a hello úložiště dotazů v aplikaci SSMS tooget výkonnostních parametrů v reálném čase.

V tématu hello [výkonu pokyny tématu](sql-database-performance-guidance.md) toofind techniky, které můžete použít tooimprove výkonu databáze SQL Azure, je-li identifikovat některé problém pomocí těchto sestav a zobrazení.

> [!IMPORTANT] 
> Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-tooimprove-performance"></a>Optimalizace výkonu databáze tooimprove

Databáze SQL Azure vám umožní tooimprove tooidentify příležitostí a optimalizovat výkon dotazů beze změny prostředky kontrolou [doporučení ladění výkonu](sql-database-advisor.md). Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. Můžete použít tyto vyladění výkonu tooimprove doporučení vašich úloh.
Můžete také umožňují Azure SQL database příliš[automaticky optimalizovat výkon vašich dotazů](sql-database-automatic-tuning.md) použitím všechny identifikovat doporučení a ověřování, že se zlepšit výkon databáze. Můžete použít následující možnosti tooimprove výkon vaší databáze hello:

1. Použití [Poradce pro funkci SQL Database](sql-database-advisor-portal.md) tooview doporučení pro vytváření a rušení indexů, parametrické dotazy a opravit problémy schématu.
2. [Povolit automatické ladění](sql-database-automatic-tuning-enable.md) a nechat Azure SQL databáze automaticky oprava identifikovat problémy s výkonem.

## <a name="improving-database-performance-with-more-resources"></a>Zvýšení výkonu databáze s další prostředky

Nakonec pokud nejsou žádné řešitelné položky, které může zlepšit výkon vaší databáze, můžete změnit hello objem prostředků, které jsou k dispozici ve službě Azure SQL Database. Další zdroje informací můžete přiřadit změnou hello [vrstvy služby](sql-database-service-tiers.md) samostatné databáze nebo kdykoli zvýšit hello počet jednotek Edtu fondu elastické databáze.
1. Pro samostatné databáze, můžete [Změna úrovně služeb](sql-database-service-tiers.md) výkonu databáze tooimprove na vyžádání.
2. V případě více databází, zvažte použití [elastické fondy](sql-database-elastic-pool-guidance.md) tooscale prostředky automaticky.

## <a name="tune-and-refactor-application-or-database-code"></a>Ladění a refactor aplikace nebo kódu databáze

Toomore kód aplikace, můžete změnit optimálně použijte hello databáze, změňte indexy, vynutit plány nebo použijte pomocné parametry toomanually přizpůsobit hello databáze tooyour zatížení. Najít některé pokyny a tipy pro ruční ladění a přepisování hello kód v hello [výkonu pokyny tématu](sql-database-performance-guidance.md) článku.


## <a name="next-steps"></a>Další kroky

- tooenable automatické ladění v Azure SQL Database a umožňují automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- toouse ruční ladění, můžete zkontrolovat [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a použít ručně hello ty, které zlepšit výkon vašich dotazů.
- Změnit prostředky, které jsou k dispozici ve vaší databázi změnou [úrovních služby databáze SQL Azure](sql-database-performance-guidance.md)