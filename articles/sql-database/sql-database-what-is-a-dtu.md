---
title: 'SQL Database: Co je DTU? | Dokumentace Microsoftu'
description: "Vysvětlení jednotky transakce Azure SQL Database."
keywords: "možnosti databáze, výkon databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: 89e3e9ce-2eeb-4949-b40f-6fc3bf520538
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 04/13/2017
ms.author: carlrab
ms.openlocfilehash: ba1fe580b32826259edaf6c8dc124faaf5b3d234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Vysvětlení jednotek databázové transakce (DTU) a elastických jednotek databázové transakce (eDTU)
Tento článek vysvětluje, že jednotky transakcí databáze (Dtu) a jednotky transakcí elastické databáze (Edtu) a co se stane, když je dosaženo hello maximální počet jednotek Dtu nebo Edtu.  

## <a name="what-are-database-transaction-units-dtus"></a>Co jsou jednotky databázové transakce (DTU)
Pro jednu databázi Azure SQL na úrovni výkonu specifických v rámci [vrstvy služby](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels), Microsoft zaručuje úroveň prostředky pro tuto databázi (nezávisle na jakékoli jiné databáze v cloudu Azure hello) a poskytování předvídatelný úroveň výkonu. Toto množství prostředků počítá se jako počet jednotky transakcí databáze nebo Dtu a je kombinaci měření procesoru, paměti a vstupu a výstupu (dat a transakční protokol vstupně-výstupních operací). Hello poměr mezi tyto prostředky se původně určený [OLTP srovnávacího testu zatížení](sql-database-benchmark-overview.md) určené toobe typické reálného OLTP úloh. Pokud vaše úlohy překročí hello množství kterýkoli z těchto prostředků, vaše propustnost je omezenému – což pomalejší výkon a vypršení časových limitů. Hello prostředky využívané třídou vaše úlohy nemají vliv hello prostředky k dispozici tooother databází SQL v hello cloudu Azure a prostředků hello používá jiná zatížení nemají vliv hello prostředky k dispozici tooyour SQL database.

![ohraničující pole](./media/sql-database-what-is-a-dtu/bounding-box.png)

Počet jednotek Dtu jsou zvláště užitečné pro pochopení hello relativní objem prostředků mezi databází SQL Azure na úrovních různých výkonu a úrovně služeb. Například zvýší hello Dtu zvýšením hello úroveň výkonu databáze znamená zároveň toodoubling hello sadu databáze k dispozici toothat prostředků. Například databáze Premium P11 se 1 750 DTU nabízí 350x více DTU výpočetního výkonu než databáze Basic s 5 DTU.  

toogain podrobnější přehled o spotřeby prostředků (DTU) hello vašich úloh, použijte [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) na:

- Identifikujte hello nejčastějších dotazů podle využití procesoru a doba trvání nebo provádění počtu, který lze ladit potenciálně pro zlepšení výkonu. Například dotaz intenzivním vstupně-výstupních operací může mít užitek z používání hello [techniky optimalizace v paměti](sql-database-in-memory.md) toomake lepší využití hello dostupné paměti na určité služby a výkonu úrovni.
- Rozbalit hello podrobnosti o dotazu, zobrazit historii využití prostředků a text.
- Výkon přístupu k ladění doporučení, které se zobrazí akce prováděné [Poradce pro funkci SQL Database](sql-database-advisor.md).

Můžete [Změna úrovně služeb](sql-database-service-tiers.md) kdykoli s minimálními výpadky tooyour aplikaci (obvykle průměrování než čtyři sekund). Mnoha firmám a aplikace je možné toocreate databází a vytočit výkonu nahoru nebo dolů na vyžádání, stačí, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud máte vzorce nepředvídatelné použití, může být pevný toomanage náklady a obchodní model. V tomto scénáři použijete fondu elastické databáze s počtem jednotek Edtu, která jsou sdílena mezi několika databáze ve fondu hello.

![Úvod tooSQL databáze: jeden Dtu databáze vrstvy a úroveň](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Co jsou elastické jednotky databázové transakce (eDTU)
Spíše než poskytují vyhrazené sadu prostředků (Dtu) tooa SQL Database, která je neustále dostupná bez ohledu na to, jestli není potřeba, můžete umístit do databáze [elastický fond](sql-database-elastic-pool.md) na databázi SQL serveru, který sdílí fondu zdrojů mezi tyto databáze. Hello sdílené prostředky v elastickém fondu měří elastické jednotky transakcí databáze nebo Edtu. Elastické fondy poskytují jednoduché nákladově efektivní řešení toomanage hello výkonnostní cíle pro více databází, které mají široce různých a nepředvídatelným vzorce. V elastickém fondu, může zaručit, že žádná databáze. jeden používá všechny prostředky hello v hello fondu a také že minimální objem prostředků je vždy k dispozici tooa databáze v elastickém fondu. V tématu [elastické fondy](sql-database-elastic-pool.md) Další informace.

![Úvod tooSQL databáze: Edtu vrstvy a úroveň](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Fond obdrží stanovený počet eDTU za stanovenou cenu. V rámci hello elastického fondu jsou uvedeny jednotlivé databáze hello flexibilitu tooauto rozsahu v rámci hranice hello nakonfigurované. V případě velkého zatížení databáze spotřebovat další vyžádání toomeet Edtu zatímco databáze pod světla zatížením nižší, až bod toohello, že databáze žádné zatížení využívat žádné Edtu. Zřizování prostředků pro celý fond hello, místo na databázi, úlohy správy jsou zjednodušené a mají předvídatelný rozpočtu hello fondu.

Další jednotek Edtu, které lze přidat existující fond tooan bez výpadků databáze a bez jakéhokoli dopadu na hello databáze ve fondu hello. Podobně platí, že pokud již přidané eDTU nejsou potřebné, lze je z existujícího fondu kdykoli odebrat. Můžete přidat nebo odečíst fondu toohello databáze nebo limit hello množství Edtu databázi můžete použít v případě velkého zatížení tooreserve Edtu pro jiné databáze. Pokud databázi je předvídatelné pod využití prostředků, můžete přesunout mimo hello fondu a nakonfigurovat ho jako jednu databázi s velikostí prostředků vyžaduje předvídatelný.

## <a name="how-can-i-determine-hello-number-of-dtus-needed-by-my-workload"></a>Jak zjistím hello počet jednotek Dtu Moje zatížení vyžaduje?
Pokud hledáte toomigrate existující místní nebo tooAzure zatížení virtuálního počítače systému SQL Server SQL Database, můžete použít hello [Kalkulačka DTU](http://dtucalculator.azurewebsites.net/) tooapproximate hello počet jednotek Dtu potřeby. Pro existující úlohy Azure SQL Database, můžete použít [SQL databáze Query Performance Insight](sql-database-query-performance.md) toounderstand vaší databáze prostředků spotřeby (Dtu) tooget podrobnější přehled o tom, jak toooptimize úlohy. Můžete taky hello [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV tooget hello prostředků informací o spotřebě pro hello poslední jednu hodinu. Alternativně hello zobrazení katalogu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) můžete také být předmětem dotazu tooget hello stejná data pro hello posledních 14 dní, i když na nižší přesnost průměry pět minut.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Jak poznám, že by pro mě používání elastického fondu prostředků bylo výhodné?
Fondy jsou vhodné pro velký počet databází s konkrétními vzory využití. Pro danou databázi je tento vzor charakterizován nízkou mírou průměrného využití s relativně málo častými nárůsty využití. SQL Database automaticky vyhodnotí využití historických prostředků hello databází v existující databázi SQL server a doporučuje hello odpovídajícímu fondu adres konfigurace v hello portálu Azure. Další informace najdete v tématu [Kdy je vhodné používat elastický fond?](sql-database-elastic-pool.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Co se stane, když dosáhnu maximálního počtu DTU
Jsou nastaveny úrovně výkonu a upraveny tooprovide hello potřebné prostředky toorun úloh toohello maximální omezení povolené úrovně vrstvy a výkonu vaší vybraných služeb vaší databáze. Pokud vaše úlohy je nedosáhli limitů hello v jednom z limity vstupně-výstupní operace procesoru/Data vstupně-výstupní operace nebo protokolu, pokračovat tooreceive hello prostředky maximální povolené úrovni hello, ale jsou pravděpodobně toosee vyšší latence pro své dotazy. Pokud hello zpomalení stane závažné, že dotazy spustí časování nezpůsobovalo neustálé všechny chyby, ale spíš zpomalení hello zatížení, tato omezení. Pokud dosahujete limitů pro maximální povolený počet souběžných relací/požadavků uživatelů (pracovních vláken), zobrazují se explicitní chyby. Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).

## <a name="next-steps"></a>Další kroky
* V tématu [vrstvy služby](sql-database-service-tiers.md) informace o hello Dtu a Edtu, které jsou k dispozici pro izolované databáze a pro elastické fondy.
* Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).
* V tématu [SQL databáze Query Performance Insight](sql-database-query-performance.md) toounderstand vaší spotřeby (Dtu).
* V tématu [přehledu srovnávacího testu SQL Database](sql-database-benchmark-overview.md) toounderstand hello metodika za hello OLTP srovnávacího testu zatížení použít toodetermine hello přizpůsobte DTU.
